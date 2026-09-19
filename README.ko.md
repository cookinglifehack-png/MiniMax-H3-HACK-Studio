# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | **한국어** | [Deutsch](README.de.md) | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [설치 프로그램 다운로드 (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

ComfyUI 위에서 동작하는 동영상 생성 모델 **MiniMax H3**를, 설치만 하면 바로 쓸 수
있는 Windows 앱으로 만들었습니다. ComfyUI 워크플로 편집도, PHP 지식도 필요
없습니다——더블클릭으로 실행한 뒤, 브라우저 폼을 다루듯 조작만으로 T2V・I2V・
R2V 동영상 생성을 할 수 있습니다.

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
