# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | **Deutsch** | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [Installer herunterladen (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

Wir haben das auf ComfyUI laufende Videogenerierungsmodell **MiniMax H3** in eine
Windows-App verpackt, die sofort nach der Installation einsatzbereit ist. Kein
Bearbeiten von ComfyUI-Workflows, keine PHP-Kenntnisse nötig — einfach
doppelklicken zum Starten, danach läuft die gesamte T2V-, I2V- und
R2V-Videogenerierung über Formularsteuerung wie im Browser.

## Einrichtung (ComfyUI + MiniMax-H3-Modelle)

Der Installer enthält die App selbst, aber **nicht** ComfyUI oder die
MiniMax-H3-Modellgewichte — du brauchst weiterhin eine ComfyUI-Instanz mit
installierten H3-Modellen und registrierst sie im Einstellungsbildschirm der
App als Backend. Lade die folgenden Dateien herunter und lege sie unter
`ComfyUI/models/<Ordner>/` ab (die Dateinamen müssen exakt mit dem
übereinstimmen, was die App erwartet — richtest du das mit einem KI-Agenten/
einer KI-CLI ein, verwende einfach die Spalte „Dateiname" unten genau so).

### ComfyUI selbst

- Offizielle Installationsanleitung (Windows Portable): https://docs.comfy.org/installation/comfyui_portable_windows

### ① Minimum (nur um H3 überhaupt laufen zu lassen)

Alle diese Dateien stammen aus dem offiziellen Repository
[`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3).

| Rolle | Dateiname | Zielordner |
|---|---|---|
| Text-Encoder | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| Audio-VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| Video-VAE (fp16) | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va (int8-quantisiert) | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va (int8-quantisiert) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

Damit laufen T2V/I2V/R2V bereits vollständig (quant=int8, ohne
Beschleunigungs-LoRA oder TensorRT).

### ② Empfohlen (schnellste gemessene Konfiguration)

Diese Kombination war in Benchmarks am schnellsten (Basis 442s → 182s, etwa
59% schneller). Zusätzlich zu ① installieren.

| Rolle | Quelle | Dateiname / Ablage |
|---|---|---|
| Turbo, pruned-build-Version LoRA | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Turbo-Node | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | in `custom_nodes/` clonen (`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`) |
| VAE-TRT-Node | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | in `custom_nodes/` clonen, `pip install tensorrt` |
| VAE-TRT-ONNX-Gewichte | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | in `models/vae/` ablegen, dann einmalig über den Node „MiniMax-H3 TRT VAE Compiler" zu `.engine` kompilieren (bei unter 12GB VRAM den w4a16_awq-Low-VRAM-Decoder wählen) |
| SageAttention-Node | https://github.com/kijai/ComfyUI-KJNodes | in `custom_nodes/` clonen (`PathchSageAttentionKJ`) |
| SageAttention selbst (Windows-Wheel) | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows`, danach das zu deiner PyTorch/CUDA-Version passende Wheel installieren |

### ③ Alles (weitere Quantisierungen, Beschleunigungsmethoden und dekorative LoRAs)

| Rolle | Quelle | Dateiname / Ablage |
|---|---|---|
| UNet w4a8-quantisiert | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| Video-VAE int8-quantisiert (Kijais ConvRot-Build) | https://huggingface.co/Comfy-Org/MiniMax-H3 (Ordner `vae/`) | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/` (erfordert ComfyUI 0.31.0+) |
| Turbo, lightx2v-Version (fl2v/ref2v getrennt) | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc (8-Step, offiziell) | Modell: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>Node: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate (3-Step) | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | die ausgelieferte `TaoMate-H3-3step-ComfyUI.safetensors` in **`minimax_h3_taomate_3step.safetensors`** umbenennen und in `models/loras/` ablegen (am besten für T2V/I2V — für R2V nicht empfohlen, da Referenzbilder dort kaum durchschlagen) |
| Prompt-Rewriter (lokales LLM, optional — der Reiter „LLM-Integration" im Einstellungsbildschirm bietet alternativ eine API-Key-basierte Lösung) | Node: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>LoRA-Adapter: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | die Basis-GGUF-Modelle (Qwen3.6-27B / Qwen3-VL-8B-Instruct / Qwen2.5-Omni-7B) findest du im README des Nodes |
| Dekorative LoRA: realistische Menschen | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | die ausgelieferte `h3-realism-people-t2v-i2v-r2v.safetensors` in **`minimax_h3_realism_people_lora.safetensors`** umbenennen und in `models/loras/` ablegen |
| Dekorative LoRA: räumliche Physik | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | die ausgelieferte `wushu_spatial_physics_*_pruned.safetensors` (zwei Varianten vorhanden — unklar, welche hier verwendet wurde) in **`minimax_h3_spatial_physics_lora.safetensors`** umbenennen und in `models/loras/` ablegen |
| Dekorative LoRA: 16-Bit-Pixelart | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | die ausgelieferte `16bit-pixel.safetensors` in **`minimax_h3_16bit_pixel_lora.safetensors`** umbenennen und in `models/loras/` ablegen |

Eine umfassendere, laufend aktualisierte Liste findest du in der von der
Community gepflegten Liste
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
(verfolgt über 150 Quantisierungsvarianten, Fine-Tunes und LoRAs).

## Generierungsbildschirm — ein einziger, klarer Bildschirm

![Generierungsbildschirm](screenshots/generation.jpg)

Einfach zwischen T2V (Text), I2V (Bild) und R2V (mehrere Referenzen) per Tab
wechseln. In der Warteschlange rechts wird ein fertiges Video sofort inline
abgespielt, sobald der Job abgeschlossen ist — auch laufende Jobs zeigen ihren
Fortschritt, sodass sich die Wartezeit nie wie verlorene Zeit anfühlt. Dauer,
Auflösung und Seitenverhältnis lassen sich sofort über Schieberegler und Presets
anpassen, mit der tatsächlich übermittelten Auflösung (z. B. 1024×576 px) in
Echtzeit angezeigt.

## Zusatzfunktionen — Beschleunigungsoptionen ohne Rätselraten wählen

![Zusatzfunktionen-Bildschirm](screenshots/extra-features.jpg)

Quantisierung (int8/w4a8), Beschleunigungs-LoRAs der Turbo-Familie, VAE-
Beschleunigung — rund um H3 gibt es viele Optionen, und eine falsche Kombination
kann die Generierung zerstören. Dieser Bildschirm **erklärt direkt neben jeder
Option deren Wirkung und Fallstricke**, sodass du nie raten musst. T2V/I2V und
R2V haben jeweils ihre eigene optimale Beschleunigungswahl, da die
unterstützten LoRAs je nach Generierungsmodus unterschiedlich sind.

## LLM-Integration — Prompt-Unterstützung an einem Ort verwalten

![LLM-Integration-Bildschirm](screenshots/llm-integration.jpg)

Registriere API-Schlüssel für xAI, OpenAI, Anthropic und Google Gemini und
verwalte sie alle von einem einzigen Bildschirm aus. Deutlich hervorgehoben
wird, dass **Google Gemini vollständig im Rahmen seines kostenlosen Kontingents
genutzt werden kann, ohne Kreditkartenregistrierung** — eine niedrige
Einstiegshürde zum Ausprobieren. Registrierte Schlüssel werden maskiert
angezeigt, und du kannst jedem frei eine Rolle zuweisen: Umschreiben von
Prompts im Generierungsformular oder stapelweises Erzeugen von Szenen für R2V.

## Für wen das geeignet ist

- Für alle, die H3-Videos allein über Formularsteuerung generieren möchten,
  ohne den Node-Graph von ComfyUI anzufassen
- Für alle, die Quantisierung, Beschleunigungs-LoRAs und VAE-Beschleunigung
  nicht verwenden möchten, ohne zu verstehen, was jede einzelne bewirkt
- Für alle, die Prompt-Unterstützung ausprobieren möchten, während sie
  zwischen mehreren LLM-API-Schlüsseln wechseln

## Download

Der Windows-Installer (in sich geschlossen, keine zusätzliche Laufzeitumgebung
nötig) ist über Releases erhältlich.
