---
title: 'Referring Layer Decomposition'
layout: project_page
author: Fangyi Chen, Yaojie Shen, Lu Xu, Ye Yuan, Shu Zhang, Yulei Niu, Longyin Wen
date: 2026-02-18
aliases:
  - /project/rld/
---

<!--more-->

<section class="hero">
  <div class="hero-body">
    <div class="container is-max-desktop">
      <div class="columns is-centered">
        <div class="column has-text-centered">
          <h1 class="title is-1 publication-title">Referring Layer Decomposition</h1>
          <!-- <br> -->
          <div class="is-size-5 publication-authors">
            <span class="author-block">
              <a href="https://fangyi-chen.github.io/">Fangyi Chen</a>,</span>
            <span class="author-block">
              <a href="https://yaojie-shen.github.io">Yaojie Shen</a>,</span>
            <span class="author-block">
              <a href="https://www.linkedin.com/in/lu-xu-34b200160/">Lu Xu</a>,</span>
            <span class="author-block">
              <a href="https://scholar.google.com/citations?user=L0W1LjMAAAAJ&hl=en">Ye Yuan</a>,</span>
            <span class="author-block">
              <a href="https://scholar.google.com/citations?user=k9zsuBIAAAAJ&hl=en">Shu Zhang</a>,</span>
            <span class="author-block">
              <a href="https://yuleiniu.github.io/">Yulei Niu</a>,</span>
            <span class="author-block">
              <a href="https://scholar.google.com/citations?user=PO9WFl0AAAAJ&hl=en">Longyin Wen</a></span>
          </div>
          <div class="is-size-5 publication-authors">
            <span class="author-block">Intelligent Editing Team, Intelligent Creation, ByteDance Inc.</span>
          </div>
          <div class="column has-text-centered">
            <div class="publication-links">
              <span class="link-block">
                <a href="https://iclr.cc/virtual/2026/poster/10011003" class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                    <i class="fas fa-file-pdf"></i>
                  </span>
                  <span>Paper</span>
                </a>
              </span>
              <span class="link-block">
                <a href="https://arxiv.org/abs/" class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                    <i class="ai ai-arxiv"></i>
                  </span>
                  <span>arXiv</span>
                </a>
              </span>
              <span class="link-block">
                <a href="https://iclr.cc/virtual/2026/poster/10011003" class="external-link button is-normal is-rounded is-dark">
                  <span class="icon">
                    <i class="fab fa-github"></i>
                  </span>
                  <span>Code (coming soon)</span>
                </a>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>



<style>

h1 {
  margin-top: 60px;
  font-size: 42px;
  letter-spacing: -1px;
}

.main-row {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2%;
  margin: 5vh auto;
  padding: 0 2vw;
  max-width: 1300px;
  box-sizing: border-box;
  overflow-x: auto;
}

.image-block {
  flex: 0 0 25%; /* Original 和 Prompt 各占25% */
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: visible; /* 避免阴影被裁剪 */
  padding-bottom: 15px;
}

.image-block-first {
  padding-left: 10px;
}

.image-block-last {
  padding-right: 10px;
}

.flow-block {
  flex: 0 0 10%; /* RefLayer方框占15% */
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 20px;
}

.image-block img {
  width: 100%;
  height: auto;
  object-fit: contain;
  border-radius: 16px;
  box-shadow: 0 5px 10px rgba(0,0,0,0.4);
  transition: opacity 0.6s ease, transform 0.6s ease;
  display: block;
  background: white;
}

.image-block h3 {
  margin-bottom: 15px;
}

.arrow {
  font-size: 36px;
  margin: 0 0px;
  user-select: none;
}

/* ===== Model Box ===== */
.model-box {
  display: inline-block;
  width: 95px; /* 固定宽度 */
  padding: 10px 0; /* 上下留一点空白 */
  background: linear-gradient(135deg, #6C63FF, #00C6FF);
  color: white;
  font-size: 15px; /* 固定字体 */
  font-weight: 600;
  border-radius: 16px;
  box-shadow: 0 0 20px rgba(108,99,255,0.6);
  animation: pulse 2s infinite;
  white-space: nowrap;
  text-align: center;
}

@keyframes pulse {
  0% { box-shadow: 0 0 clamp(5px, 1.5vw, 25px) rgba(108,99,255,0.4); }
  50% { box-shadow: 0 0 clamp(20px, 2.5vw, 50px) rgba(108,99,255,0.9); }
  100% { box-shadow: 0 0 clamp(5px, 1.5vw, 25px) rgba(108,99,255,0.4); }
}

/* ===== Result ===== */

/* Fade */
.fade-out {
  opacity: 0;
  transform: scale(0.97);
}
</style>
</head>

<body>


<div class="main-row">

  <div class="image-block image-block-first">
    <h3>Original</h3>
    <img id="oriImage" src="images/spatial/1_ori.jpg">
  </div>

  <div class="image-block">
    <h3>Prompt</h3>
    <img id="promptImage" src="images/spatial/1_prompt_1.jpg">
  </div>

  <div class="flow-block">
    <div class="model-box">RefLayer</div>
    <div class="arrow">→</div>
  </div>

  <div class="image-block image-block-last">
    <h3>Layered Result</h3>
    <img id="resultImage" src="images/spatial/1_result_1.jpg">
  </div>

</div>

<script>
let folders = ["spatial", "linguistic", "multi_granularity"];
let folderIndex = 0;
let imageId = 1;
let promptId = 1;

const oriImage = document.getElementById("oriImage");
const promptImage = document.getElementById("promptImage");
const resultImage = document.getElementById("resultImage");

function updateImages() {
  const folder = folders[folderIndex];

  // 先淡出 Prompt 和 Result
  promptImage.classList.add("fade-out");
  resultImage.classList.add("fade-out");

  // 更新 Original 图片
  const newOriSrc = `images/${folder}/${imageId}_ori.jpg`;
  oriImage.src = newOriSrc;

  oriImage.onload = () => {
    // 延迟 200ms 后更新 Prompt
    setTimeout(() => {
      const newPromptSrc = `images/${folder}/${imageId}_prompt_${promptId}.jpg`;
      promptImage.src = newPromptSrc;

      promptImage.onload = () => {
        promptImage.classList.remove("fade-out");

        // 延迟 300ms 更新 Result
        setTimeout(() => {
          const newResultSrc = `images/${folder}/${imageId}_result_${promptId}.jpg`;
          resultImage.src = newResultSrc;

          resultImage.onload = () => {
            resultImage.classList.remove("fade-out");
          };
        }, 300);
      };
    }, 200);
  };
}

// 使用 async 递归方式确保一组加载完成再更新下一组
async function nextImageSet() {
  const startTime = Date.now();

  updateImages();

  // 等待图片加载完成，假设最长延迟 1000ms +加载时间
  await new Promise((resolve) => setTimeout(resolve, 1200));

  // 计算已显示时间，确保至少 3秒停留
  const elapsed = Date.now() - startTime;
  const minDisplayTime = 3000; // 3秒
  if (elapsed < minDisplayTime) {
    await new Promise((resolve) => setTimeout(resolve, minDisplayTime - elapsed));
  }

  // 更新下一组的索引
  promptId++;
  if (promptId > 2) {
    promptId = 1;
    imageId++;
    if (imageId > 2) {
      imageId = 1;
      folderIndex++;
      if (folderIndex >= folders.length) folderIndex = 0;
    }
  }

  nextImageSet(); // 递归更新下一组
}

// 启动动画
nextImageSet();
</script>




<section class="section">
  <div class="container is-max-desktop">
    <div class="columns is-centered has-text-centered">
      <div class="column is-four-fifths">
        <h2 class="title is-3">Abstract</h2>
        <div class="content has-text-justified">
          <p>
Precise, object-aware control over visual content is essential for advanced image editing and compositional generation. Yet, most existing approaches operate on entire images holistically, limiting the ability to isolate and manipulate individual scene elements. In contrast, layered representations, where scenes are explicitly separated into objects, environmental context, and visual effects, provide a more intuitive and structured framework for interpreting and editing visual content. 
To bridge this gap and enable both compositional understanding and controllable editing, we introduce the Referring Layer Decomposition (RLD) task, which predicts complete RGBA layers from a single RGB image, conditioned on flexible user prompts, such as spatial inputs (<i>e.g.</i>, points, boxes, masks), natural language descriptions, or combinations thereof.
At the core is the RefLade, a large-scale dataset comprising 1.11M image–layer–prompt triplets produced by our scalable data engine, along with 100K manually curated, high-fidelity layers. Coupled with a perceptually grounded, human-preference-aligned automatic evaluation protocol, RefLade establishes RLD as a well-defined and benchmarkable research task.
Building on this foundation, we present RefLayer, a simple baseline designed for prompt-conditioned layer decomposition, achieving high visual fidelity and semantic alignment.
Extensive experiments show our approach enables effective training, reliable evaluation, and high-quality image decomposition, while exhibiting strong zero-shot generalization capabilities.
          </p>
        </div>
      </div>
    </div>
  </div>
</section>

<section class="section">
  <div class="container is-max-desktop">
    <div class="columns is-centered">
      <div class="column is-full-width">
        <h3 class="title is-4">RefLade: Data Engine, Dataset and Evaluation Protocol</h3>
        <h3 class="title is-5">Data Engine</h3>
          <div class="content has-text-justified">
            <p>
            We establish a scalable, modular and automated data engine designed to get diverse, realistic, and high-fidelity RGBA layers from natural images.
            </p>
          </div>
          <div class="columns is-centered">
            <div class="column has-text-centered">
              <img src="images/data_engine_overview.jpg" width="100%">
              <h2 class="subtitle has-text-centered">
                Overview of the data engine.
              </h2>
            </div>
          </div>
        <h3 class="title is-5">Dataset</h3>
          <div class="content has-text-justified">
            <p>
            </p>
          </div>
        <h3 class="title is-5">Evaluation Protocol</h3>
          <div class="content has-text-justified">
            <p>
              from three aspects:
            </p>
            <p>
              <b>Aspect 1: Preservation.</b>
              <br>
              \[\mathcal{S}_{\text{vis}} = \mathbb{E}_{(p, g) \sim \mathcal{D}} [ \text{LPIPS}(g_{\text{rgb}} \odot g_v,\, p_{\text{rgb}} \odot g_v) ] \]
            </p>
            <p>
              <b>Aspect 2: Completion.</b>
              <br>
              \[\mathcal{S}_{\text{gen}} = \mathbb{E}_{(p, g) \sim \mathcal{D}} \left[\cos\left( f(g_{\text{rgb}}) - f(g_{\text{rgb}} \odot g_v), \,f(p_{\text{rgb}}) - f(g_{\text{rgb}} \odot g_v) \right)\right]\]
            </p>
            <p>
              <b>Aspect 3: Faithfulness.</b>
              <br>
              \[\hat{p} = p_{\text{rgb}} \odot p_a + i_{\text{bkgd}} \odot (1 - p_a), \quad
\hat{g} = g_{\text{rgb}} \odot g_a + i_{\text{bkgd}} \odot (1 - g_a)\]
\[\mathcal{S}_{\text{fid}} = \text{FID}\left( \left\{ \hat{p} \mid p \in \mathcal{D} \right\}, \left\{ \hat{g} \mid g \in \mathcal{D} \right\} \right)\]
            </p>
          </div>
      </div>
    </div>
  </div>
  <br>
</section>

<section class="section">
  <div class="container is-max-desktop">
    <div class="columns is-centered">
      <div class="column is-full-width">
        <h3 class="title is-4">RefLayer: A Baseline Model</h3>
        <div class="columns is-centered">
          <div class="column has-text-centered">
            <img src="images/reflayer_model.jpg" width="100%">
            <h2 class="subtitle has-text-centered">
              RefLayer model architecture.
            </h2>
          </div>
        </div>
        <div class="content has-text-centered">
          <table style="font-size: 0.9em;">
            <tr>
              <td rowspan="2"><b>Dataset</b></td>
              <td rowspan="2"><b>#layers</b></td>
              <td colspan="4"><b>Foreground</b></td>
              <td colspan="4"><b>Background</b></td>
            </tr>
            <tr>
              <td><b>HPA &uarr;</b></td>
              <td><b>FID &darr;</b></td>
              <td><b>LPIPS &darr;</b></td>
              <td><b>DIR &uarr;</b></td>
              <td><b>HPA &uarr;</b></td>
              <td><b>FID &darr;</b></td>
              <td><b>LPIPS &darr;</b></td>
              <td><b>DIR &uarr;</b></td>
            </tr>
            <tr>
              <td>MuLAn</td>
              <td>50K</td>
              <td>0.3852</td>
              <td>22.68</td>
              <td>0.1403</td>
              <td>0.2031</td>
              <td>0.3459</td>
              <td>21.84</td>
              <td>0.1588</td>
              <td>0.6385</td>
            </tr>
            <tr>
              <td>RefLade</td>
              <td>50K</td>
              <td>0.4629</td>
              <td>10.98</td>
              <td>0.1411</td>
              <td>0.2543</td>
              <td>0.5932</td>
              <td>16.87</td>
              <td>0.0520</td>
              <td>0.7206</td>
            </tr>
            <tr>
              <td>RefLade</td>
              <td>100K</td>
              <td>0.4621</td>
              <td>11.27</td>
              <td>0.1428</td>
              <td>0.2589</td>
              <td>0.5935</td>
              <td>16.73</td>
              <td>0.0530</td>
              <td>0.7213</td>
            </tr>
            <tr>
              <td>RefLade</td>
              <td>200K</td>
              <td>0.4631</td>
              <td>10.99</td>
              <td>0.1434</td>
              <td>0.2547</td>
              <td>0.5461</td>
              <td>19.84</td>
              <td>0.0552</td>
              <td>0.6950</td>
            </tr>
            <tr>
              <td>RefLade</td>
              <td>400K</td>
              <td>0.4678</td>
              <td>10.66</td>
              <td>0.1404</td>
              <td>0.2575</td>
              <td>0.5792</td>
              <td>18.36</td>
              <td>0.0493</td>
              <td>0.7129</td>
            </tr>
            <tr>
              <td>RefLade</td>
              <td>1M</td>
              <td>0.4685</td>
              <td>11.10</td>
              <td>0.1377</td>
              <td>0.2561</td>
              <td>0.5587</td>
              <td>17.35</td>
              <td>0.0730</td>
              <td>0.7190</td>
            </tr>
            <tr>
              <td>RefLadeQ</td>
              <td>100K</td>
              <td>0.4698</td>
              <td>10.60</td>
              <td>0.1378</td>
              <td>0.2531</td>
              <td>0.6657</td>
              <td>12.99</td>
              <td>0.0487</td>
              <td>0.7721</td>
            </tr>
            <tr class="ours">
              <td><b>RefLade+Q</b></td>
              <td><b>1.1M</b></td>
              <td><b>0.4813</b></td>
              <td>10.50</td>
              <td>0.1330</td>
              <td>0.2652</td>
              <td><b>0.6682</b></td>
              <td>13.14</td>
              <td>0.0437</td>
              <td>0.7673</td>
            </tr>
          </table>
        </div>
        <h2 class="subtitle has-text-centered">
          Benchmarking RefLade with Different Training Set and Scale. Results are reported on the RefLade testing set with multimodal text+box prompts.
        </h2>
      </div>
    </div>
  </div>
  <br>
</section>

<section class="section">
  <div class="container is-max-desktop">
    <div class="columns is-centered">
      <div class="column is-three-quarters has-text-justified">
        <h3 class="title is-4">BibTeX</h3>
        <div class="content">
          <pre><code>@inproceedings{chen2026referring,
  title     = {Referring Layer Decomposition},
  author    = {Chen, Fangyi and Shen, Yaojie and Xu, Lu and Yuan, Ye and Zhang, Shu and Niu, Yulei and Wen, Longyin},
  booktitle = {ICLR 2026 (Virtual)},
  year      = {2026},
  url       = {https://iclr.cc/virtual/2026/poster/10011003}
}</code></pre>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  body {
    background: linear-gradient(180deg, #f8f9ff, #eef1ff);
  }

  table tr td:first-child {
    text-align: left;
  }

  table tr.ours {
    background-color: rgb(255, 255, 237);
  }

  table tr td {
    color: rgb(75, 75, 75);
    padding-top: 0.3em !important;
    padding-bottom: 0.3em !important;
  }
</style>
