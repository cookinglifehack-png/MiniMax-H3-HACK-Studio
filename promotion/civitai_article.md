# CivitAI Article 下書き

投稿先: CivitAI（要アカウント）→ プロフィール → Articles → Create Article
形式: 記事形式なので他より長め・見出し立てOK。画像は複数枚推奨（既にある3枚の
　　　スクリーンショットがそのまま使える）。

---

## タイトル案
- "MiniMax H3 HACK Studio — A Standalone Windows Frontend for Local H3 Generation"

## 本文

### What is this?

MiniMax H3 HACK Studio is a self-contained Windows application for running
**MiniMax H3** — the open-weights video generation model with native
synchronized audio — through a simple form-based UI instead of ComfyUI's node
graph.

It connects to a ComfyUI backend you already have running (local or LAN), so
it's not a replacement for ComfyUI — think of it as a dedicated frontend for
people who want fast, no-friction iteration on H3 generations specifically.

### Generation screen

[screenshots/generation.jpg]

Switch between T2V (text), I2V (image), and R2V (multiple references) with
tabs. Finished jobs play inline in the queue on the right, with progress
visible for jobs still running. Duration, resolution, and aspect ratio adjust
live via sliders and presets.

### Extra Features — acceleration options explained inline

[screenshots/extra-features.jpg]

Quantization (int8/w4a8), Turbo-family LoRAs, VAE acceleration (TensorRT) —
each option is presented with its effect and caveats right next to the radio
button, so picking a broken combination is harder to do by accident. T2V/I2V
and R2V get separate optimal choices since supported LoRAs differ by mode.

### LLM Integration — prompt assistance, your choice of provider

[screenshots/llm-integration.jpg]

Register API keys for xAI, OpenAI, Anthropic, or Google Gemini (free tier, no
credit card) to use for prompt rewriting or R2V multi-scene JSON generation.

### Requirements

- A ComfyUI instance with the MiniMax H3 models already installed
- Windows 10/11, self-contained installer (no extra runtime needed)

### Download

Still in beta (v0.9b). [Download link]

Feedback and bug reports welcome.

---

※ 投稿前に確認: リポジトリが公開されている必要があります（今はPrivate）。
　 画像はスクリーンショットをCivitAIの記事エディタに直接アップロードする形になります
　（このリポジトリの`screenshots/`フォルダにある3枚）。
