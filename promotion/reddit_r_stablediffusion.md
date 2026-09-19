# r/StableDiffusion 投稿下書き

投稿先: https://www.reddit.com/r/StableDiffusion/
形式: r/comfyuiより層が広い（画像/動画生成全般）ので、H3自体の紹介も少し厚めに。
　　　こちらも画像/動画のサンプル添付が必須級。

---

## タイトル案
- "MiniMax H3 HACK Studio — a lightweight Windows frontend for local MiniMax H3 video+audio generation"

## 本文

For anyone running **MiniMax H3** locally via ComfyUI (open-weights T2V/I2V/R2V
model with native synchronized audio) — I built a small Windows app on top of
it so I didn't have to touch the node graph every time: **MiniMax H3 HACK
Studio**.

What it does:
- Simple form UI for T2V / I2V / R2V, with a live job queue (finished videos
  play inline)
- Lets you switch between quantization/acceleration options (int8/w4a8,
  Turbo LoRA variants, VAE TensorRT, SageAttention) without hunting through
  workflow JSON — each option is explained in the UI
- Optional LLM-assisted prompt rewriting / R2V scene generation (bring your
  own API key — xAI, OpenAI, Anthropic, or Gemini, which has a free tier)

Requires your own ComfyUI instance with the H3 models already installed (this
isn't a replacement for ComfyUI, just a frontend). Windows installer,
self-contained, still in beta (v0.9b).

[Download link] / [Screenshots/GIF]

---

※ 投稿前に確認: リポジトリが公開されている必要があります（今はPrivate）。
