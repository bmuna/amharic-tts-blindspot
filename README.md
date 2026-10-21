# Amharic same-spelling stress TTS

Some Amharic words are written **exactly the same** but mean different things depending on whether speech is **stressed** or **relaxed**. A text-to-speech model has to use sentence meaning to pick the right sound — the letters alone are not enough.

This repo is a small pilot testing that on [Lab-et/omnivoice-amharic](https://huggingface.co/Lab-et/omnivoice-amharic) (~0.6B).

## Main finding

- **0/4** pairs switched pronunciation with context
- **40/40** regenerations stayed stable (same wrong default each time)
- Defaults: ዋና / ሰፊ → always stressed; ገና / አያት → always relaxed

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
