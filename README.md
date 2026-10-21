# Amharic same-spelling gemination TTS

Some Amharic words are written **exactly the same** but mean different things depending on **gemination** vs **non-gemination** (ጠብቆ vs ላልቶ: the consonant is held longer, or not). A text-to-speech model has to use sentence meaning to pick the right sound — the letters alone are not enough.

In file names, **stressed** = gemination and **relaxed** = non-gemination (not English syllable stress).

Example: **አለ** can mean “he said” or “there is,” depending on how it is said.

This repo is a small pilot testing that on [Lab-et/omnivoice-amharic](https://huggingface.co/Lab-et/omnivoice-amharic) (~0.6B).

## Main finding

- In all **4/4** pairs, both context sentences got the same pronunciation
- **40/40** separate regenerations stayed stable
- Defaults: ዋና / ሰፊ → always gemination; ገና / አያት → always non-gemination

## Explore the repo

```
hf_bucket/
  README.md                   full write-up
  audio/human_recordings/     my spoken references (8)
  audio/tts_single_generation/ first model outputs (8)
  audio/tts_five_repeats/     5 regenerations per sentence (40)
  results/                    sentences, listening labels, summary
  docs/pair_names.md          file-name legend
```

Start with [`hf_bucket/README.md`](hf_bucket/README.md), then listen to human vs TTS under `hf_bucket/audio/`.

Hugging Face dataset: https://huggingface.co/datasets/munation/amharic-tts-blindspot  
Hugging Face bucket: https://huggingface.co/buckets/munation/amharic-tts-blindspot
