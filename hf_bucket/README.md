---
pretty_name: Amharic TTS Same-Spelling Gemination Blindspot
license: cc-by-4.0
language:
- am
- en
task_categories:
- text-to-speech
tags:
- audio
- amharic
- tts
- speech
- pronunciation
- gemination
size_categories:
- n<1K
---

# Amharic same-spelling gemination: a blind spot in open TTS

## What I noticed (Question 1)

As an Amharic speaker, I keep noticing words that are written **exactly the same** but mean different things depending on how people say them. In school we called this **ጠብቆ** vs **ላልቶ**. In linguistics this is **gemination** vs **non-gemination**: the consonant is held longer, or it is not. The writing does **not** show it.

**Note on labels:** in this README and in file names, **“stressed” means gemination** (a held / doubled consonant = ጠብቆ). **“Relaxed” means non-gemination** (ላልቶ). This is **not** English syllable stress (louder syllable).

A clear everyday example is **አለ**:
- አለ said **ላልቶ** (relaxed / non-gemination) → “he said”
- አለ said **ጠብቆ** (stressed / gemination) → “there is / exists”

Same letters. Different meaning. Only speech carries the difference.

I have heard TTS systems sound fluent in Amharic but still miss this. They read the letters and pick one default sound. That is the blind spot I tested: **sounds that change the meaning of a word**, while the writing stays the same.

So a TTS model cannot “see” the answer in the spelling. It has to use the **sentence meaning** to pick gemination vs non-gemination (ጠብቆ vs ላልቶ).

Benchmarks usually check if speech sounds clear. They rarely check this: same letters, two meanings — does the model switch the pronunciation?

File names use `stressed` / `relaxed` so folders stay easy to sort. They mean gemination / non-gemination, **not** extra letters on the page.

## Model

I used [Lab-et/omnivoice-amharic](https://huggingface.co/Lab-et/omnivoice-amharic) (~0.6B parameters).

Why this model:
- open Amharic TTS
- fits the challenge size range (0.6B–6B)
- runs in Google Colab

## What I did

I built 4 pairs. Each pair is **one spelling**, two meanings (gemination / ጠብቆ vs non-gemination / ላልቶ). For each meaning I wrote a short context sentence myself.

| Same spelling | Gemination (stressed / ጠብቆ) | Non-gemination (relaxed / ላልቶ) |
|---------------|------------------------------|--------------------------------|
| ዋና | key / important | swim |
| ሰፊ | wide | tailor's shop |
| ገና | gena (Christmas game) | yet / still |
| አያት | grandparent | looked intently |

Steps:
1. Generate TTS for all 8 sentences (first pass).
2. Listen as a native speaker and label each clip as **stressed** (gemination) or **relaxed** (non-gemination).
3. Regenerate each sentence **5 times** (40 clips) to check if the failure is stable or random. Each of the 5 clips per sentence is a **separate model generation** (a different audio file), not a copy of the same file. I did this so the result does not depend on a single run.
4. Record my own human reference audio for all 8 sentences (so you can hear the contrast the model misses).

Full sentences and labels are in `results/`.

## Results

**First pass:** in all 4 pairs, both context sentences got the **same** pronunciation (0/4 switched).

**5× regeneration:** every sentence stayed the same across all 5 **separate** generations (40/40 stable). Still, in all 4 pairs, both sentences kept that same pronunciation (0/4 switched).

What I heard:
- ዋና and ሰፊ → always **gemination** (stressed / ጠብቆ), in both contexts and all repeats
- ገና and አያት → always **non-gemination** (relaxed / ላልቶ), in both contexts and all repeats

So in this pilot, the model did not use sentence meaning to choose gemination vs non-gemination for these same-spelling pairs.

## Limits (this is a pilot)

- Only 4 pairs
- One listener (me)
- Listening was **not blind** — I knew which sentence was which when I labeled
- I wrote the context sentences myself
- Labels from listening, not yet measured consonant duration
- One model

## How to reproduce

- Google Colab
- Model: `Lab-et/omnivoice-amharic` via the OmniVoice package
- Language: Amharic
- Settings used: `num_step=32`, `guidance_scale=2.0`
- Generate each of the 8 sentences once, then regenerate each sentence 5 times
- Compare against the human recordings in `audio/human_recordings/`

## Path forward (Question 3)

To fix this, we need training data that makes the choice explicit:
- same spelling (identical text)
- two meanings
- two gold audios (gemination vs non-gemination)
- clear context sentences for each

Then fine-tune the TTS so it must pick the right spoken form from context — not one default for both sentences of a pair.

A stronger option later: give the model an explicit control for this contrast (hold length / gemination).

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
