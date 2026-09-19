# MiniMax H3 HACK Studio

**English** | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [Download the installer (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

A Windows app that lets you use the video generation model **MiniMax H3**, which
runs on ComfyUI, the moment you install it. No ComfyUI workflow editing, no PHP
knowledge required — just double-click to launch, then generate T2V, I2V, and
R2V videos entirely through browser-style form controls.

## Generation screen — a single, no-guesswork layout

![Generation screen](screenshots/generation.jpg)

Just switch between T2V (text), I2V (image), and R2V (multiple references) with
tabs. The queue on the right plays finished videos inline the moment a job
completes — and you can watch jobs still in progress, so the wait never feels
like dead time. Duration, resolution, and aspect ratio are all adjustable on the
spot via sliders and presets, with the actual resolution that will be submitted
(e.g. 1024×576 px) shown live.

## Extra Features — pick acceleration options with confidence

![Extra Features screen](screenshots/extra-features.jpg)

Quantization (int8/w4a8), Turbo-family acceleration LoRAs, VAE acceleration —
there are a lot of options around H3, and picking the wrong combination can
break generation. This screen **explains the effect and caveats of each option**
right next to its radio button, so you're never guessing. T2V/I2V and R2V each
get their own optimal acceleration choice, since the supported LoRAs differ by
generation mode.

## LLM Integration — manage prompt assistance in one place

![LLM Integration screen](screenshots/llm-integration.jpg)

Register API keys for xAI, OpenAI, Anthropic, and Google Gemini and manage them
all from a single screen. It's clearly highlighted that **Google Gemini can be
used entirely within its free tier, with no credit card required** — a low
barrier for trying it out first. Registered keys are shown masked, and you can
freely assign each one's role: rewriting prompts on the generation form, or
batch-generating scenes for R2V.

## Who this is for

- People who want to generate H3 videos through form controls alone, without
  touching ComfyUI's node graph
- People who'd rather not use quantization, acceleration LoRAs, and VAE
  acceleration without understanding what each one does
- People who want to try prompt assistance while switching between multiple
  LLM API keys

## Download

The Windows installer (self-contained, no additional runtime required) is
available from Releases.
