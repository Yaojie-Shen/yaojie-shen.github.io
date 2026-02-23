---
title: Btrfs文件系统
date: 2020-04-01T18:20:17+08:00
updated: 2020-04-01T18:20:17+08:00
tags:
 - Linux
categories:
 - Linux
enableComment: true
---

Btrfs文件系统已经逐渐被各种Linux发行版本支持（作为系统分区格式），Btrfs具备CoW（写时复制）的特性，相比于之前的很多文件系统增添了很多特殊的功能，本文对其中的常用功能进行了介绍。
但是由于文件系统操作不当容易丢失数据，操作之前记得做好额外备份。

<!--more-->

## 写时复制

写时复制（Copy-on-write, CoW）指了在多个调用者请求相同资源时，只有在某个调用者试图修改资源的内容时，系统才会为其复制一份专用副本。这样没有写操作的时候，就不会有多余的副本被创建。[^wiki-copy-on-write]
CoW的缺点之一在于对于像VM镜像、数据库文件这样的就地更改（updated-in-place）的文件，会导致写入**碎片化**。[^btrfs-wiki-copy-on-write]所以对于这一类数据，不妨建一个子卷然后禁用CoW来储存他们。（别忘了修改`fstab`）

Btrfs默认启用写时复制，要停止使用写时复制，使用`nodatacow`选项，但是这一更改只会影响新创建的文件，对于已有文件（夹）使用下列命令进行修改，但仍存在一些细节问题，使用前务必参见参考资料中关于此节的详细描述[^arch-wiki-btrfs-copy-on-write]。

```shell
chattr +C </path/to/file/or/folder>
```

## 子卷（Subvolume）[^arch-wiki-btrfs-subvolume]

Btrfs通过Subvolume来实现在备份时排除某些文件夹。

### 挂载子卷

通过设置挂载的选项可以挂载指定的子卷：

```shell
mount -o subvol=<subvol> <device> <mount_path>
```

### 子卷的修改操作

#### 列出子卷

```shell
btrfs subvolume list -p path
```

使用后会列出对应`path`下的所有子卷，其数量可能会很多，因为所有的快照也以subvolume的形式储存，有意思的是Docker镜像也被保存为了subvolume：

#### 创建子卷

```shell
btrfs subvolume create </path/to/subvolume>
```

> 这里的`path`指的是子卷的绝对路径，比如当前挂载了`@`到`/mnt/@`目录下，则使用路径`/mnt/@/home`创建出来的子卷为`@/home`。

#### 删除子卷

```shell
btrfs subvolume delete /path/to/subvolume
```

> 如果只移除文件目录，而不使用`btrfs subvolume delete`命令并不会真正删除一个子卷。

#### 默认子卷

```shell
# 获取默认子卷
btrfs subvolume get-default /
# 设置默认子卷
btrfs subvolume set-default <subvolume-id> /
```

#### 临时挂载

```shell
# 使用路径挂载
mount -t btrfs -o subvol=<subvolume> </mount/point>
# 使用id挂载
mount -t btrfs -o subvolid=<id> </dev/device> </mount/point>
```

### Btrfs子卷组织形式的探究[^some-info-about-subvolume-in-opensuse]

在openSUSE中查看当前的Btrfs的子卷，可能会显示大量的子卷，因为snapshot实际也是通过子卷来实现的，另外值得注意的是Docker镜像也被作为snapshot独立开了：

```shell
~$ sudo btrfs subvolume list /
ID 256 gen 90 top level 5 path @
ID 257 gen 113574 top level 256 path @/var
...
ID 263 gen 113574 top level 256 path @/home
ID 266 gen 112569 top level 256 path @/.snapshots
ID 298 gen 98293 top level 257 path @/var/lib/docker/btrfs/subvolumes/ce11ad5...    # docker镜像
...
```

其中`@`代表了文件系统的根（rootfs），但事实上它也仍然是一个snapshot，最顶层的卷是以0为标号的子卷，不过通常不使用。
同时默认的`/`同样也不是`@`子卷，一般也是某一个子卷，只是默认被挂载为了`/`，通过查看默认子卷可以得知：

```shell
~$ btrfs subvolume get-default /
ID 267 gen 113599 top level 266 path @/.snapshots/1/snapshot  # 一个snapshot被作为默认子卷，挂载为了文件系统的 `/` 目录
```

可见当前系统的`/`实际上是一个路径为`@/.snapshots/1/snapshot`的子卷，真正的`@`在openSUSE中是隔离开的，作为独立的根来储存需要永久保存的子卷。

### 创建子卷的正规步骤

正如上述讨论，由于目前的系统目录也是一个（临时）快照。
如果我们此时要创建一个子卷，不可以建立在一个一个已有的快照下，否则在进行rollback操作后就不能再删除这个子卷了。正确的操作因该是将这个子卷建立在`@`子卷下。

```shell
sudo mount /dev/sda2 -o subvol=@ /mnt
sudo btrfs subvolume create /mnt/usr/important
sudo umount /mnt
```

## 快照

Btrfs的快照是建立在其“写时复制”的功能基础上的。
创建快照可以使用如下命令：

```shell
btrfs subvolume snapshot </path/to/source> </path/to/dest>
```

> 对于openSUSE，目标目录通常为`/.shapshots`，这一目录为默认的统一存放快照的目录。
> 另外添加参数`-r`可以创建只读快照，在只读快照上再创建一个快照可以获得只读快照的一个可写版本。

注意快照**不是递归包含**的，意味着子卷里的子卷在快照中会是空目录。
这也是为什么openSUSE下部分目录被排除在默认的snapper备份之外：它们都被创建为了额外的子卷，由于上述非递归性，他们在对`/`创建的快照中均被忽略了。


## Btrfs启用压缩[^mount-compress]

在openSUSE中是支持Btrfs的压缩功能的，通过`mount`的参数可以启用压缩：

```shell
mount -o compress </dev/sdx> </mount/point>
```

`compress`的默认规则是：如果你创建了一个文件，Btrfs压缩后发现压缩率低，那对于之后的写入它都不再会进行压缩。如果不希望这样，可以使用`compress-force`。
对于已经写入的文件，均不会被压缩，**压缩仅对新写入的文件有效**。

压缩有三种算法可选：

1. **lzo**：压缩率低但是CPU资源占用少。
2. **zlib**：压缩率高但是资源占用多。
3. **zstd**：旧版本内核和`GRUB`引导对其缺乏支持，暂时忽略。

在`fstab`中永久启用压缩，并指定压缩算法（算法以不指定）：

```shell
UUID=1a2b3c4d /home btrfs subvol=@/home,compress=lzo  0   0
```

## 使用snapper进行管理[^suse-restore-from-snapper]

`snapper`通过一系列的配置来管理Btrfs分区，配置文件默认位于`/etc/snapper/configs/`下。
默认的方案只为`/`创建快照，且内容还要排除名下的子卷。

### 创建一个新的配置

```shell
sudo snapper -c <config-name> create-config </path>
```

这一操作会创建一个快照并从`/etc/snapper/config-templates/default`获取一套默认配置。

### 配置快照的设置

见openSUSE[相关文档的对应章节](https://documentation.suse.com/zh-cn/sles/12-SP4/html/SLES-all/cha-snapper.html#sec-snapper-config-modify)，以获得更准确的信息。

使用`snapper -c home set-config "<KEY>=<value>"`来修改设置。

<!-- footnotes -->

[^mount-compress]: [在挂载时启用压缩功能 | #Compressed btrfs filesystems - SDB:BTRFS - openSUSE Wiki](https://en.opensuse.org/SDB:BTRFS#Compressed_btrfs_filesystems)
[^wiki-copy-on-write]: [写入时复制 | 维基百科](https://zh.wikipedia.org/wiki/%E5%AF%AB%E5%85%A5%E6%99%82%E8%A4%87%E8%A3%BD)
[^arch-wiki-btrfs-copy-on-write]:  [写时复制 | Btrfs (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Btrfs_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%86%99%E6%97%B6%E5%A4%8D%E5%88%B6_(CoW))
[^btrfs-wiki-copy-on-write]: [关于CoW的缺点 | SysadminGuide - btrfs Wiki](https://btrfs.wiki.kernel.org/index.php/SysadminGuide#Copy_on_Write_.28CoW.29)
[^some-info-about-subvolume-in-opensuse]: [关于openSUSE上的Btrfs结构的讨论 | LEAP 42.2 btrfs root filesystem subvolume structure](https://forums.opensuse.org/showthread.php/521277-LEAP-42-2-btrfs-root-filesystem-subvolume-structure)
[^arch-wiki-btrfs-subvolume]: [子卷 | Btrfs (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Btrfs_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%AD%90%E5%8D%B7)
[^suse-restore-from-snapper]: [通过 Snapper 进行系统恢复和快照管理 | 管理指南 | SUSE Linux Enterprise Server 12 SP4](https://documentation.suse.com/zh-cn/sles/12-SP4/html/SLES-all/cha-snapper.html#sec-snapper-config)
