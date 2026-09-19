# r/comfyui 投稿下書き

投稿先: https://www.reddit.com/r/comfyui/
形式: タイトル + 本文 + 画像/GIF必須（Redditは画像無しだと埋もれる）
注意: r/comfyuiは「ComfyUIの代替/ラッパーツール」への反応が分かれることがある
　　　（"why not just use ComfyUI directly" 的なコメントが付きやすい）ので、
　　　「ノードグラフを触らずに使いたい人向け」という立ち位置を明確にしておくと良い

---

## タイトル案（どれか1つ）
- "Made a standalone Windows app for MiniMax H3 — no node graph needed"
- "MiniMax H3 HACK Studio: a form-based frontend for H3 T2V/I2V/R2V (connects to your ComfyUI backend)"

## 本文

I've been using ComfyUI for MiniMax H3 generation and wanted something my
non-technical use cases (and honestly, myself when I just want to iterate
fast) could use without touching the node graph. So I built **MiniMax H3
HACK Studio** — a Windows app that connects to your existing ComfyUI backend
and exposes T2V/I2V/R2V generation through a plain form.

Some things it does:
- Quantization (int8/w4a8), Turbo-family LoRAs, VAE TRT acceleration — all
  selectable with inline explanations of what each option does/breaks
- Multiple ComfyUI backends with automatic load balancing (queue depth based)
- Built-in LLM prompt rewriting (xAI/OpenAI/Anthropic/Gemini) and R2V
  multi-scene JSON generation
- Job queue with inline video playback

It's **not** a ComfyUI replacement — you still need ComfyUI running with the
H3 models installed, this just gives you a simpler frontend on top. Still in
beta (v0.9b). Windows installer, self-contained.

[Download link] / [Screenshots]

Happy to answer questions about the setup or take feedback.

---

※ 投稿前に確認: リポジトリが公開されている必要があります（今はPrivate）。
