# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | **中文** | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

我们把运行在 ComfyUI 上的视频生成模型 **MiniMax H3**，做成了一个装好即可使用的
Windows 应用。无需编辑 ComfyUI 工作流，也不需要 PHP 相关知识——双击启动后，
只需像操作网页表单一样，就能完成 T2V・I2V・R2V 的视频生成。

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
