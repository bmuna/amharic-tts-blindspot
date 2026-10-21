# Amharic same-spelling stress: a blind spot in open TTS

## What I noticed (Question 1)

In Amharic, some words are written **exactly the same** but mean different things depending on how you say them: one reading is **stressed** (held / taut consonant), the other is **relaxed**.

This is **not** like pairs that look different on the page (for example አለ vs አለለ). Those have different letters. My pairs do not. The text is identical; only the spoken form changes with meaning.

So a TTS model cannot “see” the answer in the spelling. It has to use the **sentence meaning** to pick stressed vs relaxed.

Benchmarks usually check if speech sounds clear. They rarely check this: same letters, two meanings — does the model switch the pronunciation?

That is the blind spot I tested: **sounds that change the meaning of a word**, while the writing stays the same.

(Linguists often call the stressed form gemination. I label clips **stressed** vs **relaxed** so it is clear we mean pronunciation, not adding letters.)

## Model

I used [Lab-et/omnivoice-amharic](https://huggingface.co/Lab-et/omnivoice-amharic) (~0.6B parameters).

Why this model:
- open Amharic TTS
- fits the challenge size range (0.6B–6B)
- runs in Google Colab

## What I did

I built 4 pairs. Each pair is **one spelling**, two meanings (stressed vs relaxed). For each meaning I wrote a short context sentence.

| Same spelling | Stressed meaning | Relaxed meaning |
|---------------|------------------|-----------------|
| ዋና | key / important | swim |
| ሰፊ | wide | tailor's shop |
| ገና | gena (Christmas game) | yet / still |
| አያት | grandparent | looked intently |

Steps:
1. Generate TTS for all 8 sentences (first pass).
2. Listen as a native speaker and label each clip as **stressed** or **relaxed**.
3. Regenerate each sentence **5 times** (40 clips) to check if the failure is stable or random.
4. Record my own human reference audio for all 8 sentences (so you can hear the contrast the model misses).

Full sentences and labels are in `results/`.

## Results

**First pass:** the model did not switch for any pair (0/4).

**5× regeneration:** every sentence stayed the same across all 5 runs (40/40 stable). Still no pair switched by context (0/4).

What the model defaulted to:
- ዋና and ሰፊ → always **stressed**
- ገና and አያት → always **relaxed**

So the model picks one pronunciation per spelling and sticks to it. It does not use sentence meaning to choose stressed vs relaxed.

## Limits (this is a pilot)

- Only 4 pairs
- One listener (me)
- Labels from listening, not yet measured consonant duration
- One model

## Path forward (Question 3)

To fix this, we need training data that makes the choice explicit:
- same spelling (identical text)
- two meanings
- two gold audios (stressed vs relaxed)
- clear context sentences for each

Then fine-tune the TTS so it must pick the right spoken form from context — not one default per spelling.

A stronger option later: give the model an explicit control for this contrast (stress / hold length).

## What’s in this bucket

```
audio/human_recordings/       my spoken references (8)
audio/tts_single_generation/  first TTS outputs (8)
audio/tts_five_repeats/       5 regenerations per sentence (40)
results/dataset_pairs.txt     pairs, sentences, first-pass labels
results/listening_five_repeats.txt
results/summary.txt
docs/pair_names.md            file-name legend
```

Model: https://huggingface.co/Lab-et/omnivoice-amharic
