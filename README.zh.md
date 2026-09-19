# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | **中文** | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [下载安装程序（Windows）](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

我们把运行在 ComfyUI 上的视频生成模型 **MiniMax H3**，做成了一个装好即可使用的
Windows 应用。无需编辑 ComfyUI 工作流，也不需要 PHP 相关知识——双击启动后，
只需像操作网页表单一样，就能完成 T2V・I2V・R2V 的视频生成。

## 安装设置（ComfyUI + MiniMax H3 模型）

安装程序中包含应用本体，但**不包含 ComfyUI 本体和 MiniMax H3 的模型本体**——需要
另行在 ComfyUI 实例中安装 H3 模型，并在应用的设置画面中将其注册为后端。请从以下
来源下载，并放置到 `ComfyUI/models/<文件夹>/` 中（文件名必须与应用引用的名称
完全一致——如果使用 AI 代理/CLI 自动配置，直接使用下表「文件名」列即可）。

### ComfyUI 本体

- 官方安装指南（Windows 便携版）: https://docs.comfy.org/installation/comfyui_portable_windows

### ① 最低限（仅运行 H3）

以下全部来自官方仓库 [`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3)。

| 角色 | 文件名 | 存放文件夹 |
|---|---|---|
| 文本编码器 | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| 音频 VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| 视频 VAE（fp16） | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va（int8 量化） | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va（int8 量化） | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

仅凭这些即可运行 T2V/I2V/R2V（quant=int8，不含加速 LoRA/TensorRT 的基础配置）。

### ② 推荐（实测最快配置）

实测结果，以下组合速度最快（基准 442 秒 → 182 秒，约缩短 59%）。请在①的基础上
追加安装。

| 角色 | 来源 | 文件名 / 存放位置 |
|---|---|---|
| Turbo 剪枝版 LoRA | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Turbo 节点 | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | clone 到 `custom_nodes/`（`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`） |
| VAE TRT 节点 | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | clone 到 `custom_nodes/`，`pip install tensorrt` |
| VAE TRT 用 ONNX | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | 放入 `models/vae/` 后，用「MiniMax-H3 TRT VAE Compiler」节点编译一次为 `.engine`（显存低于 12GB 时选择 w4a16_awq 低显存版解码器） |
| SageAttention 节点 | https://github.com/kijai/ComfyUI-KJNodes | clone 到 `custom_nodes/`（`PathchSageAttentionKJ`） |
| SageAttention 本体（Windows wheel） | https://github.com/woct0rdho/SageAttention/releases | 先 `pip install triton-windows`，再 pip install 与你的 PyTorch/CUDA 版本匹配的 wheel |

### ③ 全部（其他量化方式・加速方式・装饰性 LoRA）

| 角色 | 来源 | 文件名 / 存放位置 |
|---|---|---|
| UNet w4a8 量化 | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| 视频 VAE int8 量化（Kijai 版 ConvRot） | https://huggingface.co/Comfy-Org/MiniMax-H3（`vae/` 文件夹） | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/`（需要 ComfyUI 0.31.0 及以上版本） |
| Turbo lightx2v 版（fl2v/ref2v 分开） | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc（8 步，官方） | 模型: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>节点: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate（3 步） | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | 将发布文件名 `TaoMate-H3-3step-ComfyUI.safetensors` **重命名为 `minimax_h3_taomate_3step.safetensors`**，放入 `models/loras/`（适合 T2V/I2V，R2V 下参考图像效果较弱，不推荐） |
| 提示词重写器（本地 LLM，可选——设置画面的「LLM 集成」标签页也可用 API 密钥方式替代） | 节点: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>LoRA 适配器: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | 基础 GGUF 模型（Qwen3.6-27B/Qwen3-VL-8B-Instruct/Qwen2.5-Omni-7B）请参见该节点自己的 README |
| 装饰性 LoRA：真实人物 | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | 将发布文件名 `h3-realism-people-t2v-i2v-r2v.safetensors` **重命名为 `minimax_h3_realism_people_lora.safetensors`**，放入 `models/loras/` |
| 装饰性 LoRA：空间物理 | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | 将发布文件名 `wushu_spatial_physics_*_pruned.safetensors`（有两个版本，未确认具体使用哪一个）**重命名为 `minimax_h3_spatial_physics_lora.safetensors`**，放入 `models/loras/` |
| 装饰性 LoRA：16 位像素风 | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | 将发布文件名 `16bit-pixel.safetensors` **重命名为 `minimax_h3_16bit_pixel_lora.safetensors`**，放入 `models/loras/` |

更全面且持续更新的列表，请参考社区维护的
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
（追踪 150 余项量化变体、微调版本与 LoRA）。

## 生成画面 — 一个画面搞定，不会迷路

![生成画面](screenshots/generation.jpg)

只需在 T2V（文本）・I2V（图像）・R2V（多参考图）之间切换标签即可。右侧队列中，
提交的任务一旦完成就能立即在原地播放视频——生成中的任务也能看到进度，
等待过程不再有压力。时长・分辨率・宽高比都可以用滑块和预设即时调整，实际
提交的分辨率（例如 1024×576 px）也会实时显示。

## 附加功能 — 放心挑选加速选项

![附加功能画面](screenshots/extra-features.jpg)

量子化（int8/w4a8）、Turbo 系加速 LoRA、VAE 加速等，H3 周边的加速选项种类繁多，
选错组合还可能导致生成失败，但这个画面会**在每个选项旁边说明其效果与注意事项**，
配合单选按钮操作，不会迷茫。T2V/I2V 用和 R2V 用还分别提供各自最优的加速方式
（因为不同生成模式支持的 LoRA 不同）。

## LLM 集成 — 一站式管理提示词辅助

![LLM 集成画面](screenshots/llm-integration.jpg)

xAI・OpenAI・Anthropic・Google Gemini 等多个 LLM 提供商的 API 密钥，可在一个
画面中集中管理。**Google Gemini 无需注册信用卡即可在免费额度内使用**这一点
也做了清晰的高亮提示，非常适合先从它开始尝试。已注册的密钥会以掩码形式显示，
并可自由分配其用途——用于生成表单的提示词重写，或 R2V 的场景批量生成。

## 适合这样的人

- 不想碰 ComfyUI 的节点图，只想通过表单操作生成 H3 视频
- 不希望在完全不了解量子化・加速 LoRA・VAE 加速区别的情况下盲目使用
- 想一边切换多个 LLM API 密钥，一边尝试提示词辅助功能

## 下载

Windows 安装程序（自包含・无需额外运行时）可从 Releases 页面获取。
