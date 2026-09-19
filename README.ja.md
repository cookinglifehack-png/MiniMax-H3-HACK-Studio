# MiniMax H3 HACK Studio

[English](README.md) | **日本語** | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [インストーラーをダウンロード（Windows）](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

ComfyUI 上で動く動画生成モデル **MiniMax H3** を、インストールするだけですぐ使える
Windowsアプリにしました。ComfyUIのワークフローもPHPの知識も不要——ダブルクリックで
起動し、ブラウザ感覚のフォーム操作だけでT2V・I2V・R2Vの動画生成ができます。

## セットアップ（ComfyUI + MiniMax H3 モデル）

インストーラーにはアプリ本体が入っていますが、**ComfyUI本体・MiniMax H3のモデル
本体は含まれていません**——別途ComfyUIインスタンスにH3モデルを導入し、アプリの
設定画面からバックエンドとして登録する必要があります。以下からダウンロードし、
`ComfyUI/models/<フォルダ>/` に配置してください（ファイル名はアプリが参照する
名前と完全一致させる必要があります——AIエージェント/CLIで自動セットアップする
場合は、下表の「ファイル名」列をそのまま使ってください）。

### ComfyUI本体

- 公式インストールガイド（Windows portable）: https://docs.comfy.org/installation/comfyui_portable_windows

### ① 最低限（H3を動かすだけ）

全て公式リポジトリ [`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3) から入手できます。

| 役割 | ファイル名 | 配置先フォルダ |
|---|---|---|
| テキストエンコーダ | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| 音声VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| 動画VAE（fp16） | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va（int8量子化） | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va（int8量子化） | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

これだけでT2V/I2V/R2Vすべて動きます（quant=int8、加速LoRA/TensorRT無しの基本構成）。

### ② オススメ（実測最速構成）

実測した結果、以下の組み合わせが最速でした（ベースラインの442秒→182秒、約59%
短縮）。①に加えて導入してください。

| 役割 | 入手先 | ファイル名 / 配置先 |
|---|---|---|
| Turbo プルーニング対応版LoRA | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Turboノード | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | `custom_nodes/`にclone（`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`） |
| VAE TRTノード | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | `custom_nodes/`にclone、`pip install tensorrt` |
| VAE TRT用ONNX | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | `models/vae/`に配置後、「MiniMax-H3 TRT VAE Compiler」ノードで`.engine`に一度だけコンパイル（12GB未満VRAMならw4a16_awq低VRAM版デコーダを選択） |
| SageAttentionノード | https://github.com/kijai/ComfyUI-KJNodes | `custom_nodes/`にclone（`PathchSageAttentionKJ`） |
| SageAttention本体（Windows wheel） | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows`後、PyTorch/CUDA版に合うwheelをpip install |

### ③ 全部（他の量子化・加速方式・装飾LoRA）

| 役割 | 入手先 | ファイル名 / 配置先 |
|---|---|---|
| UNet w4a8量子化 | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| 動画VAE int8量子化（Kijai版ConvRot） | https://huggingface.co/Comfy-Org/MiniMax-H3（`vae/`フォルダ） | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/`（要ComfyUI 0.31.0+） |
| Turbo lightx2v版（fl2v/ref2v別） | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc（8step、公式） | モデル: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>ノード: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate（3step） | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | 配布名`TaoMate-H3-3step-ComfyUI.safetensors`を**`minimax_h3_taomate_3step.safetensors`にリネームして**`models/loras/`へ（T2V/I2V向け、R2Vは参照画像が効きにくいため非推奨） |
| プロンプトリライター（ローカルLLM、任意——設定画面のLLM連携タブのAPIキー方式でも代替可） | ノード: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>LoRAアダプタ: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | ベースGGUFモデル（Qwen3.6-27B/Qwen3-VL-8B-Instruct/Qwen2.5-Omni-7B）はノードのREADME参照 |
| 装飾LoRA: リアル人物 | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | 配布名`h3-realism-people-t2v-i2v-r2v.safetensors`を**`minimax_h3_realism_people_lora.safetensors`にリネームして**`models/loras/`へ |
| 装飾LoRA: 空間物理 | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | 配布名`wushu_spatial_physics_*_pruned.safetensors`（2種類あり、どちらを使ったか要確認）を**`minimax_h3_spatial_physics_lora.safetensors`にリネームして**`models/loras/`へ |
| 装飾LoRA: 16bitドット絵風 | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | 配布名`16bit-pixel.safetensors`を**`minimax_h3_16bit_pixel_lora.safetensors`にリネームして**`models/loras/`へ |

網羅的な最新リストは、コミュニティ管理の
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
も参考にしてください（量子化バリエーション・ファインチューン・LoRA等150件以上を随時追跡）。

## 生成画面 — 迷わず使える1画面構成

![生成画面](screenshots/generation.jpg)

T2V（テキスト）・I2V（画像）・R2V（複数参照）をタブで切り替えるだけのシンプルな
構成。右側のキューでは、送信したジョブが完了するとその場で動画がインライン再生
できます——生成中のジョブも進捗が見えるので、待ち時間もストレスになりません。
秒数・解像度・アスペクト比はスライダーとプリセットでその場で調整でき、
実際に送信される解像度（例: 1024×576 px）もリアルタイムに表示されます。

## 追加機能 — 高速化オプションを迷わず選べる

![追加機能画面](screenshots/extra-features.jpg)

量子化（int8/w4a8）、Turbo系加速LoRA、VAE高速化など、H3まわりの高速化オプションは
種類が多く選択を誤ると生成が壊れることもありますが、この画面では**選択肢ごとに
効果・注意点を説明**しながらラジオボタンで選べるので迷いません。T2V/I2V用と
R2V用で別々に最適な加速方式を選べるようになっているのもポイントです（対応LoRAが
動画の生成方式ごとに違うため）。

## LLM連携 — プロンプト補助をワンストップで管理

![LLM連携画面](screenshots/llm-integration.jpg)

xAI・OpenAI・Anthropic・Google Geminiと、複数のLLMプロバイダのAPIキーを1画面で
一元管理。**Google Geminiはクレジットカード登録なしの無料枠だけで使える**ことを
分かりやすくハイライトしているので、まず試してみるハードルが低いのも魅力です。
登録したキーはマスク表示され、生成フォームのプロンプト書き換えやR2Vのシーン
一括生成に使う役割を自由に割り当てられます。

## こんな人におすすめ

- ComfyUIのノードグラフをいじらず、フォーム操作だけでH3の動画生成をしたい
- 量子化・加速LoRA・VAE高速化の違いがよく分からないまま使うのは避けたい
- 複数のLLM APIキーを切り替えながらプロンプト補助を試したい

## ダウンロード

インストーラー（Windows、自己完結型・追加ランタイム不要）は Releases から入手して
ください。
