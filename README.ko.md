# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | **한국어** | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [설치 프로그램 다운로드 (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

ComfyUI 위에서 동작하는 동영상 생성 모델 **MiniMax H3**를, 설치만 하면 바로 쓸 수
있는 Windows 앱으로 만들었습니다. ComfyUI 워크플로 편집도, PHP 지식도 필요
없습니다——더블클릭으로 실행한 뒤, 브라우저 폼을 다루듯 조작만으로 T2V・I2V・
R2V 동영상 생성을 할 수 있습니다.

## 설치 설정(ComfyUI + MiniMax H3 모델)

설치 프로그램에는 앱 본체가 포함되어 있지만, **ComfyUI 본체・MiniMax H3 모델
본체는 포함되어 있지 않습니다**——별도로 ComfyUI 인스턴스에 H3 모델을 설치하고,
앱의 설정 화면에서 백엔드로 등록해야 합니다. 아래에서 다운로드하여
`ComfyUI/models/<폴더>/`에 배치해 주세요(파일명은 앱이 참조하는 이름과 완전히
일치해야 합니다——AI 에이전트/CLI로 자동 설정하는 경우, 아래 표의 "파일명" 열을
그대로 사용하면 됩니다).

### ComfyUI 본체

- 공식 설치 가이드(Windows portable): https://docs.comfy.org/installation/comfyui_portable_windows

### ① 최소한(H3만 동작시키기)

전부 공식 저장소 [`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3)에서 받을 수 있습니다.

| 역할 | 파일명 | 배치 폴더 |
|---|---|---|
| 텍스트 인코더 | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| 오디오 VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| 비디오 VAE(fp16) | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va(int8 양자화) | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va(int8 양자화) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

이것만으로 T2V/I2V/R2V가 모두 동작합니다(quant=int8, 가속 LoRA/TensorRT 없는 기본 구성).

### ② 추천(실측 최고속 구성)

실측 결과, 다음 조합이 가장 빨랐습니다(기준 442초 → 182초, 약 59% 단축). ①에
추가로 도입해 주세요.

| 역할 | 출처 | 파일명 / 배치 위치 |
|---|---|---|
| Turbo 프루닝판 LoRA | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Turbo 노드 | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | `custom_nodes/`에 clone(`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`) |
| VAE TRT 노드 | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | `custom_nodes/`에 clone, `pip install tensorrt` |
| VAE TRT용 ONNX | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | `models/vae/`에 배치 후, "MiniMax-H3 TRT VAE Compiler" 노드로 `.engine`으로 한 번만 컴파일(VRAM 12GB 미만이면 w4a16_awq 저VRAM판 디코더 선택) |
| SageAttention 노드 | https://github.com/kijai/ComfyUI-KJNodes | `custom_nodes/`에 clone(`PathchSageAttentionKJ`) |
| SageAttention 본체(Windows wheel) | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows` 후, PyTorch/CUDA 버전에 맞는 wheel을 pip install |

### ③ 전부(다른 양자화・가속 방식・장식용 LoRA)

| 역할 | 출처 | 파일명 / 배치 위치 |
|---|---|---|
| UNet w4a8 양자화 | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| 비디오 VAE int8 양자화(Kijai판 ConvRot) | https://huggingface.co/Comfy-Org/MiniMax-H3(`vae/` 폴더) | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/`(ComfyUI 0.31.0 이상 필요) |
| Turbo lightx2v판(fl2v/ref2v 별도) | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc(8스텝, 공식) | 모델: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>노드: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate(3스텝) | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | 배포명 `TaoMate-H3-3step-ComfyUI.safetensors`를 **`minimax_h3_taomate_3step.safetensors`로 이름을 바꿔서** `models/loras/`에(T2V/I2V용, R2V는 참조 이미지가 잘 반영되지 않아 비권장) |
| 프롬프트 리라이터(로컬 LLM, 선택사항——설정 화면의 LLM 연동 탭의 API 키 방식으로도 대체 가능) | 노드: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>LoRA 어댑터: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | 베이스 GGUF 모델(Qwen3.6-27B/Qwen3-VL-8B-Instruct/Qwen2.5-Omni-7B)은 해당 노드의 README 참조 |
| 장식용 LoRA: 리얼 인물 | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | 배포명 `h3-realism-people-t2v-i2v-r2v.safetensors`를 **`minimax_h3_realism_people_lora.safetensors`로 이름을 바꿔서** `models/loras/`에 |
| 장식용 LoRA: 공간 물리 | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | 배포명 `wushu_spatial_physics_*_pruned.safetensors`(2종류 있으며, 어느 쪽을 사용했는지 미확인)를 **`minimax_h3_spatial_physics_lora.safetensors`로 이름을 바꿔서** `models/loras/`에 |
| 장식용 LoRA: 16비트 픽셀 아트 | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | 배포명 `16bit-pixel.safetensors`를 **`minimax_h3_16bit_pixel_lora.safetensors`로 이름을 바꿔서** `models/loras/`에 |

더 포괄적이고 계속 갱신되는 목록은 커뮤니티가 관리하는
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
도 참고해 주세요(양자화 변형・파인튜닝・LoRA 등 150건 이상을 수시로 추적).

## 생성 화면 — 헤매지 않는 단일 화면 구성

![생성 화면](screenshots/generation.jpg)

T2V(텍스트)・I2V(이미지)・R2V(다중 참조)를 탭으로 전환하기만 하면 되는 간단한
구성입니다. 오른쪽 큐에서는 제출한 작업이 완료되면 그 자리에서 바로 동영상을
재생할 수 있습니다——생성 중인 작업도 진행 상황이 보이므로 대기 시간도
스트레스가 되지 않습니다. 초수・해상도・화면비는 슬라이더와 프리셋으로 그
자리에서 조정할 수 있고, 실제로 제출되는 해상도(예: 1024×576 px)도 실시간으로
표시됩니다.

## 추가 기능 — 고속화 옵션을 헤매지 않고 선택

![추가 기능 화면](screenshots/extra-features.jpg)

양자화(int8/w4a8), Turbo계 가속 LoRA, VAE 고속화 등 H3 주변의 고속화 옵션은
종류가 많아 잘못 선택하면 생성이 깨지기도 하지만, 이 화면에서는 **선택지마다
효과・주의점을 설명**하면서 라디오 버튼으로 고를 수 있어 헤매지 않습니다.
T2V/I2V용과 R2V용으로 각각 최적의 가속 방식을 따로 고를 수 있다는 점도
포인트입니다(대응 LoRA가 동영상 생성 방식마다 다르기 때문입니다).

## LLM 연동 — 프롬프트 보조를 한 곳에서 관리

![LLM 연동 화면](screenshots/llm-integration.jpg)

xAI・OpenAI・Anthropic・Google Gemini 등 여러 LLM 제공업체의 API 키를 한
화면에서 통합 관리합니다. **Google Gemini는 신용카드 등록 없이 무료 한도
내에서 사용할 수 있다**는 점을 알기 쉽게 강조하고 있어, 먼저 시험해 보기에
진입 장벽이 낮은 것도 매력입니다. 등록한 키는 마스킹되어 표시되며, 생성
폼의 프롬프트 재작성이나 R2V의 장면 일괄 생성에 쓰일 역할을 자유롭게
할당할 수 있습니다.

## 이런 분께 추천합니다

- ComfyUI의 노드 그래프를 건드리지 않고 폼 조작만으로 H3 동영상을 생성하고 싶다
- 양자화・가속 LoRA・VAE 고속화의 차이를 잘 모른 채로 사용하는 것은 피하고 싶다
- 여러 LLM API 키를 전환해 가며 프롬프트 보조를 시험해 보고 싶다

## 다운로드

Windows 설치 프로그램(자체 완결형・추가 런타임 불필요)은 Releases에서
받을 수 있습니다.
