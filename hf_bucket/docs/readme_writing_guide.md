# README outline — write this in your own words

Create file: hf_bucket/README.md

## Required sections

### 1. Title
Something like: Amharic same-spelling stress blind spot in open TTS

### 2. Question 1 — Blind spot
Write from your experience:
- Same spelling, two meanings: stressed vs relaxed pronunciation
- NOT different written words like አለ vs አለለ
- Standard benchmarks rarely test whether TTS uses sentence meaning to pick the right pronunciation
- Do NOT say “only Ethiopians can say this”
- DO say “sounds that change the meaning of a word”
- Optional: mention linguists call stressed form gemination; we label stressed/relaxed

### 3. Model
- Lab-et/omnivoice-amharic
- ~0.6B parameters
- Why chosen: open Amharic TTS, fits 0.6–6B, runnable in Colab

### 4. Method
- 4 same-spelling pairs (identical letters)
- 2 context sentences each (stressed meaning vs relaxed meaning)
- Generate TTS audio
- Native listening: stressed vs relaxed
- Then regenerate each sentence 5 times (40 clips) to check stability
- Record human references for all 8 sentences

### 5. Results
Include these facts:
- First pass: 0/4 pairs switched
- 5x test: 40/40 stable within sentence
- Still 0/4 pairs switched by context
- ዋና + ሰፊ defaulted to stressed
- ገና + አያት defaulted to relaxed

### 6. Limitations
- Pilot: 4 pairs, one listener
- Listening labels, not yet acoustic duration measurement
- One model

### 7. Question 3 — Path forward
Propose:
- Build stressed/relaxed-labeled Amharic TTS data (same spelling, two meanings, two gold audios, context sentences)
- Fine-tune so model must realize correct stressed/relaxed form from context
- Optional: explicit stress / hold-length control in the model

### 8. Bucket contents
Point to:
- audio/human_recordings/
- audio/tts_single_generation/
- audio/tts_five_repeats/
- results/dataset_pairs.txt
- results/listening_five_repeats.txt
- results/summary.txt
- docs/pair_names.md
- model link: https://huggingface.co/Lab-et/omnivoice-amharic
