<div align="center">
  <picture>
    <source srcset="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/Xiaomi_MiMo_darkmode.png?raw=true" media="(prefers-color-scheme: dark)">
    <img src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/Xiaomi_MiMo.png?raw=true" width="60%" alt="Xiaomi-MiMo" />
  </picture>
</div>

<h3 align="center">
  <b>
    <span>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</span>
    <br/>
    MiMo-VL Technical Report
    <br/>
    <span>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</span>
    <br/>
  </b>
</h3>

<br/>

<div align="center" style="line-height: 1;">
  |
  <a href="https://huggingface.co/collections/XiaomiMiMo/mimo-vl-68382ccacc7c2875500cd212" target="_blank">🤗 HuggingFace</a>
  &nbsp;|
  <a href="https://www.modelscope.cn/collections/MiMo-VL-bb651017e02742" target="_blank">🤖️ ModelScope</a>
  &nbsp;|
  <a href="https://github.com/XiaomiMiMo/MiMo-VL/blob/main/MiMo-VL-Technical-Report.pdf" target="_blank">📔 Technical Report</a>
  &nbsp;|
  <br/>
</div>

<br/>

## I. Introduction

In this report, we share our efforts to build a compact yet powerful VLM, MiMo-VL-7B. MiMo-VL-7B comprises (1) a native resolution ViT encoder that preserves fine-grained visual details, (2) an MLP projector for efficient cross-modal alignment, and (3) our [MiMo-7B language model](https://github.com/XiaomiMiMo/MiMo), specifically optimized for complex reasoning tasks. 

The development of MiMo-VL-7B involves two sequential training processes: (1) A four-stage pre-training phase, which includes projector warmup, vision-language alignment, general multi-modal pre-training, and long-context Supervised Fine-Tuning (SFT). This phase yields the MiMo-VL-7B-SFT model. (2) A subsequent post-training phase, where we introduce Mixed On-policy Reinforcement Learning (MORL), a novel framework that seamlessly integrates diverse reward signals spanning perception accuracy, visual grounding precision, logical reasoning capabilities, and human/AI preferences. This phase yields the MiMo-VL-7B-RL model.

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/benchmarks.png?raw=true">
</p>

We open-source MiMo-VL-7B series, including checkpoints of the SFT and RL model.
We believe this report along with the models will provide valuable insights to develop powerful reasoning VLMs that benefit the larger community.

### 🛤️ During this journey, we find

- **Incorporating high-quality, broad-coverage reasoning data from the pre-training stage is crucial for enhancing model performance**
  - We curate high-quality reasoning data by identifying diverse queries, employing large reasoning models to regenerate responses with long CoT, and applying rejection sampling to ensure quality.
  - Rather than treating this as supplementary fine-tuning data, we incorporate substantial volumes of this synthetic reasoning data directly into the later pre-training stages, where extended training yields continued performance improvements without saturation.
- **Mixed On-policy Reinforcement Learning further enhances model performance, while achieving stable simultaneous improvements remains challenging**
  - We apply RL across diverse capabilities, including reasoning, perception, grounding, and human preference alignment, spanning modalities including text, images, and videos. While this hybrid training approach further unlock model’s potential, interference across data domains remains a challenge.

## II. Model Details

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/architecture.png?raw=true">
</p>

> Models are available at [Huggingface Collections: MiMo-VL](https://huggingface.co/collections/XiaomiMiMo/mimo-vl-68382ccacc7c2875500cd212) and [ModelScope Collections: MiMo-VL](https://www.modelscope.cn/collections/MiMo-VL-bb651017e02742)

|   **Model**    |                            **Description**                            |                           **Download (HuggingFace)**                            |                                 **Download (ModelScope)**                                 |
| :------------: | :-------------------------------------------------------------------: | :-----------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------: |
| MiMo-VL-7B-SFT | VLM with extraordinary reasoning potential after 4-stage pre-training | [🤗 XiaomiMiMo/MiMo-VL-7B-SFT](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-SFT) | [🤖️ XiaomiMiMo/MiMo-VL-7B-SFT](https://www.modelscope.cn/models/XiaomiMiMo/MiMo-VL-7B-SFT) |
| MiMo-VL-7B-RL  |           RL model leapfrogging existing open-source models           |  [🤗 XiaomiMiMo/MiMo-VL-7B-RL](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-RL)  |  [🤖️ XiaomiMiMo/MiMo-VL-7B-RL](https://www.modelscope.cn/models/XiaomiMiMo/MiMo-VL-7B-RL)  |

## III. Evaluation Results

### General Capabilities

In general visual-language understanding, MiMo-VL-7B models achieve state-of-the-art open-source results.

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/benchmarks_general.png?raw=true">
</p>

### Reasoning Tasks

In multi-modal reasoning, both the SFT and RL models significantly outperform all compared open-source baselines across these benchmarks.

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/benchmarks_reasoning.png?raw=true">
</p>

> [!IMPORTANT]
> Results marked with \* are obtained using our evaluation framework.
> Tasks with ${\dagger}$ are evaluated by GPT-4o.

### GUI Tasks

MiMo-VL-7B-RL possess exceptional GUI understanding and grounding capabilities. As a general-purpose VL model, MiMo-VL achieves comparable or even superior performance to GUI-specialized models.

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/benchmarks_gui.png?raw=true">
</p>

### Elo Rating

With our in-house evaluation dataset and GPT-4o judgments, MiMo-VL-7B-RL achieves the highest Elo rating among all evaluated open-source vision-language models, ranking first across models spanning from 7B to 72B parameters.

<p align="center">
  <img width="95%" src="https://github.com/XiaomiMiMo/MiMo-VL/raw/main/figures/benchmarks_elo.png?raw=true">
</p>

## IV. Deployment

The MiMo-VL-7B series maintain full compatibility with the `Qwen2_5_VLForConditionalGeneration` architecture for deployment and inference.

## V. Citation

```bibtex
@misc{coreteam2025mimovltechnicalreport,
      title={MiMo-VL Technical Report}, 
      author={LLM-Core-Team Xiaomi},
      year={2025},
      eprint={2506.03569},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2506.03569}, 
}
```


## VI. Contact

Please contact us at [mimo@xiaomi.com](mailto:mimo@xiaomi.com) or open an issue if you have any questions.
