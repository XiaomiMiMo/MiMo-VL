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
  <a href="https://arxiv.org/abs/2506.03569" target="_blank">📔 Technical Report</a>
  &nbsp;|
  <a href="https://github.com/XiaomiMiMo/lmms-eval" target="_blank">📊 Evaluation Framework</a>
  &nbsp;|
  <a href="./demo/README.md" target="_blank">🔥 Gradio Demo</a>
  &nbsp;|
  <br/>
</div>

<br/>


### 🔥🔥🔥 MiMo-VL 2508 Updates

We're excited to announce improvements to our MiMo-VL ([MiMo-VL-7B-RL-2508](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-RL-2508) and [MiMo-VL-7B-SFT-2508](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-SFT-2508)), featuring enhanced performance across multiple benchmarks, improved thinking control capabilities, and better user experience.

#### 📈 Performance Improvements

MiMo-VL-7B-RL-2508 demonstrates consistent improvements across both image and video benchmarks, achieving notable milestones of **70.6 on MMMU** and **70.8 on VideoMME**.

<img src="./figures/mimo-2508.png" alt="Benchmark Improvements" width="768">


Full evaluation results can be found [below](#full-evaluation-results).

#### 🤔 Thinking Control Feature

A thinking control capability that allows users to turn off the model's reasoning mode using the no_think parameter:
- Thinking mode (default behavior): Full reasoning process visible with 100% control success rate;
- Non-thinking mode: Direct responses without reasoning, with a 99.84% control success rate
  
#### ❤️ Enhanced User Experience

Our internal VLM Arena ratings show meaningful improvement in real-world performance:
- Current model (MiMo-VL-7B-RL-2508): 1131.2 rating
- Previous version (MiMo-VL-7B-RL): 1093.9 rating

These updates deliver a more capable, flexible, and reliable vision-language model for both academic evaluation and practical applications.


#### 😉 Case Study: What are the appealing features of this car?

![Xiaomi SU7](./figures/su7.gif)

**MiMo-VL-7B-RL-2508's Answer**:  

```markdown
<think>...</think>
To determine the appealing features of the Xiaomi SU7, we analyze visual elements and context from the video:
1. Aesthetic Design
- The car boasts a sleek, aerodynamic exterior with smooth, modern lines.
- Distinctive lighting (sharp headlights and a continuous, glowing taillight strip) enhances its futuristic and sporty look.
- Vibrant color and polished finish make it visually striking.
2. Performance & Speed
- Multiple dynamic shots of the car driving at high speeds on diverse roads (desert, forest, city) emphasize agility and power.
- Close - ups of spinning wheels and the engine (with lightning-like effects) suggest advanced engineering and high-performance capabilities (likely electric or tech-driven).
3. Versatility
- The car is shown in varied environments (desert at dusk, forested roads, urban streets), highlighting its adaptability to different terrains and use cases.
4. Technological Innovation
- The engine close-up with electric-like lightning effects implies cutting-edge technology, positioning the car as a tech-forward vehicle (aligning with Xiaomi’s tech brand identity).
5. Brand Appeal
Prominent “Xiaomi SU7” branding leverages Xiaomi’s reputation in consumer tech, adding credibility and allure for tech-savvy audiences.


In summary, the Xiaomi SU7 appeals through its striking design, high-performance capabilities, versatility, technological innovation, and strong brand association.
```

#### Full Evaluation Results
<img src="./figures/metrics.jpeg" alt="Evaluation Results" width="768">

#### Model Recommendation

Both versions of the MiMo-VL-7B-2508 model are now open-sourced on Hugging Face:  
- [🤗 **MiMo-VL-7B-RL-2508**](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-RL-2508 )
  - Recommended for most users to experience and utilize.  
- [🤗 **MiMo-VL-7B-SFT-2508**](https://huggingface.co/XiaomiMiMo/MiMo-VL-7B-SFT-2508  )
  - Users may perform SFT and RL based on this model. Compared to the previous SFT version, this model demonstrates higher RL stability.  

#### Deployment Parameters
- temperature=0.3, topp=0.95  
- The system prompt is already set in `chat_template.json` and does not require additional configuration.
  


#### Thinking Control

Users can control the thinking mode by appending `/no_think` to queries:  
- **Thinking mode query** (default):  
  *"What is the answer to the question in the image?"*  
- **Non-thinking mode query**:  
  *"Identify the text in the image. /no_think"*. 

❗️Important: The `/no_think` command must be the very last part of user message, which means after `/no_think`, there shouldn't be any user content like image or video.
  
#### Placing Visual Input
For prompts with a single image or video, always place the visual media before the text. For example:

✅ Good:
```
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "image": image_path},
            {"type": "text",  "text": "Describe the image. /no_think"},
        ],
    }
]
```

❌ Bad:
```
messages = [
    {
        "role": "user",
        "content": [
            {"type": "text",  "text": "Describe the image. /no_think"},
            {"type": "image", "image": image_path},
        ],
    }
]
```

---


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

### Evaluation Framework

To facilitate open research, we open-source our evaluation framework with all prompts used in [https://github.com/XiaomiMiMo/lmms-eval](https://github.com/XiaomiMiMo/lmms-eval).

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

## Use Restrictions

You are solely responsible for your use of XiaomiMiMo/MiMo-VL and any outputs, actions, or consequences arising therefrom, and you agree not to use the XiaomiMiMo/MiMo-VL or any derivatives thereof:
- For any unlawful, fraudulent, or malicious purpose, or in any manner that violates any applicable laws or regulations；
- To infringe upon the intellectual property rights, privacy rights, publicity rights, or other lawful rights or interests of any third party;
- To exploit, harm, harass, defame, unlawfully discriminate against, or otherwise adversely affect any individual or group, including minors or vulnerable persons;
- For any military purpose or application;
- To engage in or facilitate malicious cyber activities, including but not limited to malware, ransomware, phishing, credential theft, or unauthorized access, or any activity intended to compromise or unlawfully control computer systems, networks, or encryption systems;
- To access, collect, process, disclose, or disseminate personal data or other protected information without appropriate authorization or lawful basis; or to access, interact with, scrape, or automate any third-party platform or service in violation of any applicable laws, policies, or authorization requirements;
- To use the XiaomiMiMo/MiMo-VL in a manner that autonomously executes high-risk actions without appropriate human oversight or authorization.

This notice is an informational reminder only and does not amend the applicable model licence or impose additional licence conditions. The permissions granted under that licence remain unaffected. Nothing in this notice restricts uses permitted by applicable law, including applicable exceptions and limitations.

### Usage enquiries and complaints

[mimo-legal@xiaomi.com](mailto:mimo-legal@xiaomi.com). For information on submitting a complaint, please visit the https://mimo.xiaomi.com.
