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
  padding: 0 1vw;
  max-width: 1400px;
  box-sizing: border-box;
  overflow-x: auto;
}

.image-block {
  flex: 0 0 25%;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: visible;
  padding-bottom: 15px;
}

.image-block-first {
  padding-left: 10px;
}

.image-block-last {
  padding-right: 10px;
}

.flow-block {
  flex: 0 0 10%;
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
  width: 95px;
  padding: 10px 0;
  background: linear-gradient(135deg, #6C63FF, #00C6FF);
  color: white;
  font-size: 15px;
  font-weight: 600;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(108,99,255,0.6);
  animation: pulse 3s infinite;
  white-space: nowrap;
  text-align: center;
}

@keyframes pulse {
  0% { box-shadow: 0 0 clamp(5px, 1.5vw, 25px) rgba(108,99,255,0.4); }
  50% { box-shadow: 0 0 clamp(20px, 2.5vw, 50px) rgba(108,99,255,0.9); }
  100% { box-shadow: 0 0 clamp(5px, 1.5vw, 25px) rgba(108,99,255,0.4); }
}

/* ===== Result ===== */

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

  promptImage.classList.add("fade-out");
  resultImage.classList.add("fade-out");

  const newOriSrc = `images/${folder}/${imageId}_ori.jpg`;
  oriImage.src = newOriSrc;

  oriImage.onload = () => {
    setTimeout(() => {
      const newPromptSrc = `images/${folder}/${imageId}_prompt_${promptId}.jpg`;
      promptImage.src = newPromptSrc;

      promptImage.onload = () => {
        promptImage.classList.remove("fade-out");

        setTimeout(() => {
          const newResultSrc = `images/${folder}/${imageId}_result_${promptId}.jpg`;
          resultImage.src = newResultSrc;

          resultImage.onload = () => {
            resultImage.classList.remove("fade-out");
          };
        }, 500);
      };
    }, 500);
  };
}

async function nextImageSet() {
  const startTime = Date.now();

  updateImages();

  await new Promise((resolve) => setTimeout(resolve, 1200));

  const elapsed = Date.now() - startTime;
  const minDisplayTime = 3000;
  if (elapsed < minDisplayTime) {
    await new Promise((resolve) => setTimeout(resolve, minDisplayTime - elapsed));
  }

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

  nextImageSet();
}

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
        <h3 class="title is-3">RefLade: Data Engine, Dataset and Evaluation Protocol</h3>
        <h4 class="title is-4">Data Engine</h4>
          <div class="content has-text-justified">
            <p>
            We introduce a scalable and modular automated data engine capable of generating diverse, realistic, and high-fidelity RGBA layers from natural images at scale.
            </p>
          </div>
          <div class="columns is-centered">
            <div class="column has-text-centered">
              <img src="images/data_engine_overview.jpg" width="100%">
              <h2 class="subtitle has-text-centered">
                Overview of the data engine
              </h2>
            </div>
          </div>
        <h4 class="title is-4">Dataset</h4>
        <div class="columns is-vcentered">
          <div class="column is-two-quarters">
            <div class="content has-text-centered">
              <img src="images/image_style_vis.png">
              <p class="subtitle">Image source</p>
            </div>
          </div>
          <div class="column is-two-quarters">
            <div class="content has-text-centered">
              <table style="font-size: 0.8em;">
                <tr>
                  <td><b>Dataset</b></td>
                  <td><b>Task</b></td>
                  <td><b># Images</b></td>
                  <td><b>Average<br>Resolutions</b></td>
                  <td><b># Cls</b></td>
                  <td><b># Instances</b></td>
                  <td><b>Occlusion<br>Rate</b></td>
                  <td><b>Image<br>Source</b></td>
                </tr>
                <tr>
                  <td>SAIL-VOS</td>
                  <td>Amodal</td>
                  <td>111,654</td>
                  <td>800×1280</td>
                  <td>162</td>
                  <td>1,896,296</td>
                  <td>56.3%</td>
                  <td>Synthetic</td>
                </tr>
                <tr>
                  <td>OVD</td>
                  <td>Amodal</td>
                  <td>34,100</td>
                  <td>500×375</td>
                  <td>196</td>
                  <td>-</td>
                  <td>-</td>
                  <td>Real</td>
                </tr>
                <tr>
                  <td>WALT</td>
                  <td>Amodal</td>
                  <td>15M</td>
                  <td>-</td>
                  <td>2</td>
                  <td>36M</td>
                  <td>-</td>
                  <td>Real</td>
                </tr>
                <tr>
                  <td>AHP</td>
                  <td>Amodal</td>
                  <td>56,599</td>
                  <td>-</td>
                  <td>1</td>
                  <td>56,599</td>
                  <td>-</td>
                  <td>Real</td>
                </tr>
                <tr>
                  <td>DYCE</td>
                  <td>Amodal</td>
                  <td>5,500</td>
                  <td>1000×1000</td>
                  <td>79</td>
                  <td>85,975</td>
                  <td>27.7%</td>
                  <td>Real</td>
                </tr>
                <tr>
                  <td>OMLD</td>
                  <td>Amodal</td>
                  <td>13,000</td>
                  <td>384×512</td>
                  <td>40</td>
                  <td>-</td>
                  <td>-</td>
                  <td>Synthetic</td>
                </tr>
                <tr>
                  <td>CSD</td>
                  <td>Amodal</td>
                  <td>11,434</td>
                  <td>512×512</td>
                  <td>40</td>
                  <td>129,336</td>
                  <td>26.3%</td>
                  <td>Synthetic</td>
                </tr>
                <tr>
                  <td>MuLAn</td>
                  <td>LD</td>
                  <td>44,860</td>
                  <td>-</td>
                  <td>759</td>
                  <td>101,269</td>
                  <td>7.7%</td>
                  <td>Real</td>
                </tr>
                <tr class="ours">
                  <td><b>RefLade</b></td>
                  <td><b>RLD</b></td>
                  <td><b>430,488</b></td>
                  <td><b>1831×1437</b></td>
                  <td><b>12K</b></td>
                  <td><b>871,829</b></td>
                  <td><b>60.8%</b></td>
                  <td><b>Real</b></td>
                </tr>
              </table>
              <p class="subtitle">Comparison of RefLade with related existing datasets</p>
            </div>
            <div class="content has-text-centered">
              <img src="images/bar_wordcloud.png" width="95%">
              <p class="subtitle">Instance distribution of RefLade Dataset</p>
            </div>
          </div>
        </div>
        <!-- Evaluation Protocol -->
        <h4 class="title is-4">Evaluation Protocol</h4>
        <div class="columns">
          <div class="column">
            <h5 class="title is-5">The HPA (Human
Preference Aligned) Score</h5>
            <div class="content has-text-justified">
              <p>
                Following human judgment, we evaluate the decomposition quality from three aspects:
              </p>
              <p>
                <b>Aspect 1: Preservation. </b>Preserving original visible content.
                <br>
              </p>
              <p style="font-size: 0.8em;">
                \[\mathcal{S}_{\text{vis}} = \mathbb{E}_{(p, g) \sim \mathcal{D}} [ \text{LPIPS}(g_{\text{rgb}} \odot g_v,\, p_{\text{rgb}} \odot g_v) ] \]
              </p>
              <p>
                <b>Aspect 2: Completion.</b> Generating reasonable completions for the occluded regions.
                <br>
              </p>
              <p style="font-size: 0.8em;">
                \[\mathcal{S}_{\text{gen}} = \mathbb{E}_{(p, g) \sim \mathcal{D}} \left[\cos\left( f(g_{\text{rgb}}) - f(g_{\text{rgb}} \odot g_v), \,f(p_{\text{rgb}}) - f(g_{\text{rgb}} \odot g_v) \right)\right]\]
              </p>
              <p>
                <b>Aspect 3: Faithfulness.</b> The distributional similarity between predictions and ground-truth layers.
                <br>
              </p>
              <p style="font-size: 0.8em;">
                \[\hat{p} = p_{\text{rgb}} \odot p_a + i_{\text{bkgd}} \odot (1 - p_a), \quad
  \hat{g} = g_{\text{rgb}} \odot g_a + i_{\text{bkgd}} \odot (1 - g_a)\]
  \[\mathcal{S}_{\text{fid}} = \text{FID}\left( \left\{ \hat{p} \mid p \in \mathcal{D} \right\}, \left\{ \hat{g} \mid g \in \mathcal{D} \right\} \right)\]
              </p>
              <p>
                <b>Aggregation.</b> We apply min-max normalization to each metric and then averaging them to produce the final HPA score.
              </p>
            </div>
          </div>
          <div class="column">
            <h5 class="title is-5">Alignment with Human Preference</h5>
            <div class="column is-centered interpolation-panel">
              <div class="content has-text-centered">
                <img src="images/elo1.png">
                <p class="subtitle">Human ELO vs. the HPA Score</p>
              </div>
              <!-- <div class="content has-text-centered">
                <img src="images/elo2.png">
                <p class="subtitle">Human ELO vs. S<sub>vis</sub>, S<sub>fid</sub>, and S<sub>gen</sub></p>
              </div> -->
              <div class="content has-text-centered">
                <table style="font-size: 0.9em;">
                  <tr>
                    <td><b></b></td>
                    <td><b>HPA</b></td>
                    <td><b>S<sub>vis</sub></b></td>
                    <td><b>S<sub>gen</sub></b></td>
                    <td><b>S<sub>fid</sub></b></td>
                    <td><b>S<sub>vis</sub> + S<sub>gen</sub></b></td>
                    <td><b>S<sub>fid</sub> + S<sub>gen</sub></b></td>
                    <td><b>S<sub>vis</sub> + S<sub>fid</sub></b></td>
                  </tr>
                  <tr>
                    <td><b>Pearson correlation</b></td>
                    <td>0.96</td>
                    <td>0.90</td>
                    <td>0.96</td>
                    <td>0.94</td>
                    <td>0.95</td>
                    <td>0.95</td>
                    <td>0.94</td>
                  </tr>
                  <tr>
                    <td><b>Spearman correlation</b></td>
                    <td>1</td>
                    <td>0.60</td>
                    <td>0.98</td>
                    <td>0.67</td>
                    <td>0.92</td>
                    <td>0.97</td>
                    <td>1</td>
                  </tr>
                </table>
                <p class="subtitle">Pearson and Spearman correlations with human ELO across different metrics</p>
              </div>
            </div>
          </div>
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
        <h3 class="title is-3">RefLayer: A Baseline Model</h3>
        <div class="content">
          To establish a baseline, we formulate RLD as a conditional image generation problem and employ two decoders—a standard RGB decoder and a custom alpha decoder—to reconstruct the RGB content and the alpha transparency mask from the latent representation.
        </div>
        <div class="content has-text-centered">
          <img src="images/reflayer_model.jpg" width="100%">
          <p class="subtitle has-text-centered">
            RefLayer model architecture.
          </p>
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
    <h3 class="title is-3">Qualitative Results</h3>
    <div class="columns">
      <div class="column">
        <!-- More -->
        <div class="columns is-centered is-vcentered">
          <div class="column">
            <div class="content">
              <img src="images/qualitative_1.jpg" alt="Qualitative">
            </div>
          </div>
          <div class="column has-text-centered">
            <div class="content">
              <img src="images/qualitative_2.jpg" alt="Qualitative" width="75%">
            </div>
          </div>
        </div>
        <!-- Model Comparison -->
        <div class="columns">
          <div class="column">
            <h4 class="title is-4">Model Comparison</h4>
            <p>
              We compare our RefLayer, trained on the RefLade dataset, with the same model trained on the MuLAn dataset and with Google Gemini 3 (Nano Banana Pro). Our model generally produces higher-quality predictions. The latest general-purpose generative models (Nano Banana Pro) have limitations in preservation and completion, and cannot produce true RGBA images with an alpha channel.
            </p>
          </div>
        </div>
        <!-- Img -->
        <div class="columns is-centered interpolation-panel">
          <!-- RLD vs MuLAn -->
          <div class="column">
            <div class="content">
              <img src="images/rld_vs_mulan.jpg" alt="RLD vs MuLAn">
              <p class="subtitle has-text-centered">
                Comparison between RefLayer trained on the MuLAn dataset and the same model trained on our RefLade dataset.
              </p>
            </div>
          </div>
          <!-- RLD vs Google Gemini 3 (Nano Banana Pro) -->
          <div class="column">
            <div class="content">
              <img src="images/rld_vs_nano_banana.jpg" alt="RLD vs Google Gemini 3 (Nano Banana Pro)">
              <p class="subtitle has-text-centered">
                Comparison between RefLayer and Google Gemini 3 (Nano Banana Pro).
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>


<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
        <h3 class="title is-3">BibTeX</h3>
        <pre><code>@inproceedings{rld,
    title     = {Referring Layer Decomposition},
    author    = {Chen, Fangyi and Shen, Yaojie and Xu, Lu and Yuan, Ye and Zhang, Shu and Niu, Yulei and Wen, Longyin},
    booktitle = {ICLR 2026 (Virtual)},
    year      = {2026},
    url       = {https://iclr.cc/virtual/2026/poster/10011003}
}</code></pre>
  </div>
</section>

<style>
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
