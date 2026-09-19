# Usage Guide

[English](USAGE.md) | [日本語](USAGE.ja.md)

A walkthrough from installation to your first generated video, with
screenshots of the actual screens.

## 1. Install

Run the installer (`MiniMax_H3_HACK_Studio_Setup.exe`), agree to the license
agreement, and install. Once done, launch it from the desktop or Start menu.

## 2. Register a ComfyUI backend

On first launch, go to **Settings → ComfyUI Server** and register a ComfyUI
instance that already has the H3 models installed.

![ComfyUI backend setup screen](screenshots/backend-setup.jpg)

- **Name**: any label you like (e.g. `Local`)
- **URL**: the ComfyUI URL (usually `http://127.0.0.1:8188` for the same machine)
- **input_dir** (optional): only fill this in if ComfyUI is on the same
  machine — it speeds up uploading reference files slightly. Leave it blank
  for a ComfyUI instance on another machine

You can register multiple backends (other GPUs on the same machine, or other
machines entirely) — each submission is automatically routed to whichever is
least busy.

## 3. (Optional) Choose acceleration options in Extra Features

**Settings → Extra Features** lets you configure quantization, Turbo-family
acceleration LoRAs, VAE acceleration, and more. These aren't shown on the
generation form — whatever you choose here is always what's used.

![Extra Features screen](screenshots/extra-features.jpg)

The defaults (int8 quantization, no acceleration LoRA) work fine for first-time
use. Once you care about speed, read the explanations on this screen and try
enabling things gradually — the "Recommended (fastest measured configuration)"
combination in the README is a good reference point.

## 4. (Optional) Set up LLM Integration for prompt assistance

**Settings → LLM Integration** lets you register an API key for xAI, OpenAI,
Anthropic, or Google Gemini to enable automatic prompt rewriting and
multi-scene JSON generation for R2V.

![LLM Integration screen](screenshots/llm-integration.jpg)

Google Gemini can be used entirely within its free tier without a credit
card, making it a good one to try first. This feature is entirely optional —
skipping it has no effect on regular T2V/I2V/R2V generation.

## 5. Generate a video

Back on the main screen, pick a tab — T2V (text), I2V (image), or R2V
(multiple references) — and enter a prompt.

![T2V form example](screenshots/t2v-form.jpg)

- **Steps** / **Duration** / **Resolution**: adjust with sliders (the actual
  pixel resolution that will be submitted is shown live)
- **Aspect ratio**: pick from presets
- On the I2V/R2V tabs, you can add reference images, videos, and audio files here

Once everything's filled in, click **Generate** to add the job to the queue
on the right.

### 💡 Tip: name reference files to make @-mentions more useful

When you add reference files on the R2V tab, typing `@` in the prompt field
brings up a mention menu — pick one to insert a tag like `<Picture 1>`. If you
name the file **"name_feature"** (with a single underscore), it shows up in
that menu as "name (feature)" instead of the raw filename.

Example: upload `kana_white apron.jpg`, and typing `@` will suggest it as
"kana (white apron)". This makes it much easier to tell characters apart when
referencing several of them at once (filenames with no underscore, or more
than one, are shown as-is without the extra description).

## 6. Check results in the queue

Submitted jobs appear in the queue on the right; once a job finishes, its
video plays inline automatically. Jobs still in progress show their progress
too.

![Generation screen and queue](screenshots/generation.jpg)

The "Restore to form" button lets you bring back the exact settings from a
past job into the generation form.

## Need help?

Use [Issues](../../issues) for questions or bug reports.
