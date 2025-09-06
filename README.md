<div align="center">

## 🎙️ VibeVoice: A Frontier Long Conversational Text-to-Speech Model
[![Project Page](https://img.shields.io/badge/Project-Page-blue?logo=microsoft)](https://microsoft.github.io/VibeVoice)
[![Hugging Face](https://img.shields.io/badge/HuggingFace-Collection-orange?logo=huggingface)](https://huggingface.co/collections/microsoft/vibevoice-68a2ef24a875c44be47b034f)
[![Technical Report](https://img.shields.io/badge/Technical-Report-red?logo=adobeacrobatreader)](https://arxiv.org/pdf/2508.19205)
[![Colab](https://img.shields.io/badge/Run-Colab-orange?logo=googlecolab)](https://colab.research.google.com/drive/1scbH5WOhmujRn_9_gtWLNw6BEu7v9UWv#scrollTo=h4SyUtXXqu7P)
[![Live Playground](https://img.shields.io/badge/Live-Playground-green?logo=gradio)](https://v.vibevoice.info/)

</div>

<div align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="Figures/VibeVoice_logo_white.png">
  <img src="Figures/VibeVoice_logo.png" alt="VibeVoice Logo" width="300">
</picture>
</div>

J'ai modifié ce dépôt supprimé, et je l'ai adapté dans le contexte francophone.

VibeVoice est un nouveau framework conçu pour générer à partir de texte des enregistrements audio conversationnels **expressifs**, **longs** et **à plusieurs intervenants**, tels que des podcasts. Il répond aux défis importants posés par les systèmes traditionnels de synthèse vocale (TTS), notamment en matière d'évolutivité, de cohérence des intervenants et de prise de parole naturelle.

L'une des principales innovations de VibeVoice réside dans l'utilisation de tokeniseurs de parole continue (acoustiques et sémantiques) fonctionnant à une fréquence d'images ultra-faible de 7,5 Hz. Ces tokeniseurs préservent efficacement la fidélité audio tout en améliorant considérablement l'efficacité informatique pour le traitement de longues séquences. VibeVoice utilise un framework de diffusion du token suivant (https://arxiv.org/abs/2412.08635), qui s'appuie sur un modèle linguistique à grande échelle (LLM) pour comprendre le contexte textuel et le flux du dialogue, et sur une tête de diffusion pour générer des détails acoustiques haute fidélité.

Le modèle peut synthétiser jusqu'à **90 minutes** de parole avec jusqu'à **4 locuteurs distincts**, dépassant ainsi les limites habituelles de 1 à 2 locuteurs de nombreux modèles précédents. 


<p align="left">
  <img src="Figures/MOS-preference.png" alt="MOS Preference Results" height="260px">
  <img src="Figures/VibeVoice.jpg" alt="VibeVoice Overview" height="250px" style="margin-right: 10px;">
</p>


- **Testez sur leur site : [Demo](https://v.vibevoice.info/) !**
- **Je vous ai fait un [Colab](https://colab.research.google.com/drive/1scbH5WOhmujRn_9_gtWLNw6BEu7v9UWv#scrollTo=h4SyUtXXqu7P) . Seul VibeVoice-1.5B est supporté.**

### 📋 TODO

- [ ] Fusionner les modèles dans le référentiel officiel Hugging Face ([PR](https://github.com/huggingface/transformers/pull/40546)) 
- [ ] Publier un exemple de code de training et la documentation
- [ ] VibePod :  Solution de bout en bout qui crée des podcasts à partir de documents, de pages web ou même d'un simple sujet.

### 📋 DEMO

#### 🎬 Exemples vidéo

**Histoire** 
[![Exemple Histoire](https://img.youtube.com/vi/7Vp5lw7EtJM/0.jpg)](https://youtu.be/7Vp5lw7EtJM)

**Podcast** 
[![Exemple Podcast](https://img.youtube.com/vi/DOa3Y7VU0A4/0.jpg)](https://youtu.be/DOa3Y7VU0A4)


## Models
| Model | Context Length | Generation Length |  Weight |
|-------|----------------|----------|----------|
| VibeVoice-0.5B-Streaming | - | - | On the way |
| VibeVoice-1.5B | 64K | ~90 min | [HF link](https://huggingface.co/microsoft/VibeVoice-1.5B) |
| VibeVoice-Large| 32K | ~45 min | [HF link](https://huggingface.co/microsoft/VibeVoice-Large) |

## Installation (En Anglais jusqu'en bas. Utile que pour ceux qui veulent en savoir plus)
We recommend to use NVIDIA Deep Learning Container to manage the CUDA environment. 

1. Launch docker
```bash
# NVIDIA PyTorch Container 24.07 / 24.10 / 24.12 verified. 
# Later versions are also compatible.
sudo docker run --privileged --net=host --ipc=host --ulimit memlock=-1:-1 --ulimit stack=-1:-1 --gpus all --rm -it  nvcr.io/nvidia/pytorch:24.07-py3

## If flash attention is not included in your docker environment, you need to install it manually
## Refer to https://github.com/Dao-AILab/flash-attention for installation instructions
# pip install flash-attn --no-build-isolation
```

2. Install from github
```bash
git clone https://github.com/microsoft/VibeVoice.git
cd VibeVoice/

pip install -e .
```

## Usages

### Usage 1: Gradio demo
```bash
apt update && apt install ffmpeg -y # for demo

# For 1.5B model
python demo/gradio_demo.py --model_path microsoft/VibeVoice-1.5B --share

# For Large model
python demo/gradio_demo.py --model_path microsoft/VibeVoice-Large --share
```

### Usage 2: Inference from files directly
```bash
# We provide some LLM generated example scripts under demo/text_examples/ for demo
# 1 speaker
python demo/inference_from_file.py --model_path microsoft/VibeVoice-Large --txt_path demo/text_examples/1p_abs.txt --speaker_names Alice

# or more speakers
python demo/inference_from_file.py --model_path microsoft/VibeVoice-Large --txt_path demo/text_examples/2p_music.txt --speaker_names Alice Frank
```

## FAQ
#### Q1: Is this a pretrained model?
**A:** Yes, it's a pretrained model without any post-training or benchmark-specific optimizations. In a way, this makes VibeVoice very versatile and fun to use.

#### Q2: Randomly trigger Sounds / Music / BGM.
**A:** As you can see from our demo page, the background music or sounds are spontaneous. This means we can't directly control whether they are generated or not. The model is content-aware, and these sounds are triggered based on the input text and the chosen voice prompt.

Here are a few things we've noticed:
*   If the voice prompt you use contains background music, the generated speech is more likely to have it as well. (The Large model is quite stable and effective at this—give it a try on the demo!)
*   If the voice prompt is clean (no BGM), but the input text includes introductory words or phrases like "Welcome to," "Hello," or "However," background music might still appear.
*   Speaker voice related, using "Alice" results in random BGM than others (fixed).
*   In other scenarios, the Large model is more stable and has a lower probability of generating unexpected background music.

In fact, we intentionally decided not to denoise our training data because we think it's an interesting feature for BGM to show up at just the right moment. You can think of it as a little easter egg we left for you.

#### Q3: Text normalization?
**A:** We don't perform any text normalization during training or inference. Our philosophy is that a large language model should be able to handle complex user inputs on its own. However, due to the nature of the training data, you might still run into some corner cases.

#### Q4: Singing Capability.
**A:** Our training data **doesn't contain any music data**. The ability to sing is an emergent capability of the model (which is why it might sound off-key, even on a famous song like 'See You Again'). (The Large model is more likely to exhibit this than the 1.5B).

#### Q5: Some Chinese pronunciation errors.
**A:** The volume of Chinese data in our training set is significantly smaller than the English data. Additionally, certain special characters (e.g., Chinese quotation marks) may occasionally cause pronunciation issues.

#### Q6: Instability of cross-lingual transfer.
**A:** The model does exhibit strong cross-lingual transfer capabilities, including the preservation of accents, but its performance can be unstable. This is an emergent ability of the model that we have not specifically optimized. It's possible that a satisfactory result can be achieved through repeated sampling.

## Risks and limitations

While efforts have been made to optimize it through various techniques, it may still produce outputs that are unexpected, biased, or inaccurate. VibeVoice inherits any biases, errors, or omissions produced by its base model (specifically, Qwen2.5 1.5b in this release).
Potential for Deepfakes and Disinformation: High-quality synthetic speech can be misused to create convincing fake audio content for impersonation, fraud, or spreading disinformation. Users must ensure transcripts are reliable, check content accuracy, and avoid using generated content in misleading ways. Users are expected to use the generated content and to deploy the models in a lawful manner, in full compliance with all applicable laws and regulations in the relevant jurisdictions. It is best practice to disclose the use of AI when sharing AI-generated content.

English and Chinese only: Transcripts in languages other than English or Chinese may result in unexpected audio outputs.

Non-Speech Audio: The model focuses solely on speech synthesis and does not handle background noise, music, or other sound effects.

Overlapping Speech: The current model does not explicitly model or generate overlapping speech segments in conversations.

We do not recommend using VibeVoice in commercial or real-world applications without further testing and development. This model is intended for research and development purposes only. Please use responsibly.
