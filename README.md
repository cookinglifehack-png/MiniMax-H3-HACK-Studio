# MiniMax H3 HACK Studio

**English** | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [Download the installer (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

A Windows app that lets you use the video generation model **MiniMax H3**, which
runs on ComfyUI, the moment you install it. No ComfyUI workflow editing, no PHP
knowledge required — just double-click to launch, then generate T2V, I2V, and
R2V videos entirely through browser-style form controls.

## Setup (ComfyUI + MiniMax H3 models)

The installer bundles the app itself, but **not** ComfyUI or the MiniMax H3 model
weights — you still need a ComfyUI instance with the H3 models installed, and
register it as a backend from the app's Settings screen. Download the files below
and place them under `ComfyUI/models/<folder>/` (the filenames must exactly match
what the app expects — if you're setting this up with an AI agent/CLI, just use
the "Filename" column below as-is).

### ComfyUI itself

- Official install guide (Windows portable): https://docs.comfy.org/installation/comfyui_portable_windows

### ① Minimum (just to run H3)

All of these come from the official [`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3) repository.

| Role | Filename | Target folder |
|---|---|---|
| Text encoder | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| Audio VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| Video VAE (fp16) | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va (int8 quantized) | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va (int8 quantized) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

That's enough to run T2V/I2V/R2V (quant=int8, no acceleration LoRA or TensorRT).

### ② Recommended (fastest measured configuration)

This combination was the fastest in benchmarks (baseline 442s → 182s, about a 59%
reduction). Add these on top of ①.

| Role | Source | Filename / placement |
|---|---|---|
| Turbo pruned-build LoRA | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Turbo node | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | clone into `custom_nodes/` (`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`) |
| VAE TRT node | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | clone into `custom_nodes/`, `pip install tensorrt` |
| VAE TRT ONNX weights | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | place in `models/vae/`, then compile to `.engine` once via the "MiniMax-H3 TRT VAE Compiler" node (pick the w4a16_awq low-VRAM decoder if under 12GB VRAM) |
| SageAttention node | https://github.com/kijai/ComfyUI-KJNodes | clone into `custom_nodes/` (`PathchSageAttentionKJ`) |
| SageAttention itself (Windows wheel) | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows`, then pip install the wheel matching your PyTorch/CUDA version |

### ③ Everything (other quantizations, acceleration methods, and decorative LoRAs)

| Role | Source | Filename / placement |
|---|---|---|
| UNet w4a8 quantized | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| Video VAE int8 quantized (Kijai's ConvRot build) | https://huggingface.co/Comfy-Org/MiniMax-H3 (the `vae/` folder) | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/` (requires ComfyUI 0.31.0+) |
| Turbo lightx2v build (separate fl2v/ref2v files) | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc (8-step, official) | Model: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>Node: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate (3-step) | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | rename the shipped `TaoMate-H3-3step-ComfyUI.safetensors` to **`minimax_h3_taomate_3step.safetensors`** and place in `models/loras/` (best for T2V/I2V — not recommended for R2V, where reference images tend not to come through) |
| Prompt rewriter (local LLM, optional — the Settings screen's LLM Integration tab offers an API-key based alternative) | Node: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>LoRA adapter: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | See the node's own README for the base GGUF models (Qwen3.6-27B / Qwen3-VL-8B-Instruct / Qwen2.5-Omni-7B) |
| Decorative LoRA: realistic people | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | rename the shipped `h3-realism-people-t2v-i2v-r2v.safetensors` to **`minimax_h3_realism_people_lora.safetensors`** and place in `models/loras/` |
| Decorative LoRA: spatial physics | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | rename the shipped `wushu_spatial_physics_*_pruned.safetensors` (two variants exist — unclear which one was used here) to **`minimax_h3_spatial_physics_lora.safetensors`** and place in `models/loras/` |
| Decorative LoRA: 16-bit pixel art | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | rename the shipped `16bit-pixel.safetensors` to **`minimax_h3_16bit_pixel_lora.safetensors`** and place in `models/loras/` |

For a more exhaustive, continuously updated list, see the community-maintained
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
(tracks 150+ quantization variants, fine-tunes, and LoRAs).

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
