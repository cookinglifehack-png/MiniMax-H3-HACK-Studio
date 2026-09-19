# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | **Español** | [Bahasa Indonesia](README.id.md)

### ⬇️ [Descargar el instalador (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

Hemos convertido el modelo de generación de video **MiniMax H3**, que se ejecuta
sobre ComfyUI, en una aplicación de Windows lista para usar en cuanto la
instalas. No hace falta editar workflows de ComfyUI ni tener conocimientos de
PHP: solo haz doble clic para iniciarla, y luego genera videos T2V, I2V y R2V
completamente mediante controles de formulario, como en un navegador.

## Configuración (ComfyUI + modelos de MiniMax H3)

El instalador incluye la aplicación en sí, pero **no** ComfyUI ni los pesos del
modelo MiniMax H3 — sigues necesitando una instancia de ComfyUI con los
modelos H3 instalados, y registrarla como backend desde la pantalla de
configuración de la aplicación. Descarga los archivos siguientes y colócalos
bajo `ComfyUI/models/<carpeta>/` (los nombres de archivo deben coincidir
exactamente con lo que espera la aplicación; si lo configuras con un
agente/CLI de IA, usa tal cual la columna "Nombre de archivo" de las tablas
siguientes).

### ComfyUI en sí

- Guía oficial de instalación (Windows portable): https://docs.comfy.org/installation/comfyui_portable_windows

### ① Mínimo (solo para que H3 funcione)

Todos estos archivos provienen del repositorio oficial
[`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3).

| Rol | Nombre de archivo | Carpeta destino |
|---|---|---|
| Codificador de texto | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| VAE de audio | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| VAE de video (fp16) | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va (cuantizado en int8) | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va (cuantizado en int8) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

Con esto ya funcionan T2V/I2V/R2V (quant=int8, sin LoRA de aceleración ni TensorRT).

### ② Recomendado (configuración más rápida medida)

Esta combinación fue la más rápida en las pruebas (línea base de 442s → 182s,
una reducción de aproximadamente el 59%). Añádelo sobre ①.

| Rol | Fuente | Nombre de archivo / ubicación |
|---|---|---|
| LoRA Turbo, versión pruned | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Nodo Turbo | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | clonar en `custom_nodes/` (`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`) |
| Nodo VAE TRT | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | clonar en `custom_nodes/`, `pip install tensorrt` |
| Pesos ONNX para VAE TRT | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | colocar en `models/vae/`, luego compilar una vez a `.engine` con el nodo "MiniMax-H3 TRT VAE Compiler" (elige el decodificador w4a16_awq de VRAM baja si tienes menos de 12GB de VRAM) |
| Nodo SageAttention | https://github.com/kijai/ComfyUI-KJNodes | clonar en `custom_nodes/` (`PathchSageAttentionKJ`) |
| SageAttention en sí (wheel para Windows) | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows`, luego instalar con pip el wheel correspondiente a tu versión de PyTorch/CUDA |

### ③ Todo (otras cuantizaciones, métodos de aceleración y LoRAs decorativas)

| Rol | Fuente | Nombre de archivo / ubicación |
|---|---|---|
| UNet cuantizado en w4a8 | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| VAE de video cuantizado en int8 (build ConvRot de Kijai) | https://huggingface.co/Comfy-Org/MiniMax-H3 (carpeta `vae/`) | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/` (requiere ComfyUI 0.31.0+) |
| Turbo versión lightx2v (archivos separados fl2v/ref2v) | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc (8 pasos, oficial) | Modelo: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>Nodo: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate (3 pasos) | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | renombrar el archivo distribuido `TaoMate-H3-3step-ComfyUI.safetensors` a **`minimax_h3_taomate_3step.safetensors`** y colocarlo en `models/loras/` (mejor para T2V/I2V; no recomendado para R2V, donde las imágenes de referencia tienden a no aplicarse bien) |
| Reescritor de prompts (LLM local, opcional — la pestaña "Integración LLM" de la pantalla de configuración ofrece una alternativa basada en clave de API) | Nodo: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>Adaptador LoRA: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | Consulta el propio README del nodo para los modelos base en GGUF (Qwen3.6-27B / Qwen3-VL-8B-Instruct / Qwen2.5-Omni-7B) |
| LoRA decorativa: personas realistas | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | renombrar el archivo distribuido `h3-realism-people-t2v-i2v-r2v.safetensors` a **`minimax_h3_realism_people_lora.safetensors`** y colocarlo en `models/loras/` |
| LoRA decorativa: física espacial | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | renombrar el archivo distribuido `wushu_spatial_physics_*_pruned.safetensors` (existen dos variantes; no está claro cuál se usó aquí) a **`minimax_h3_spatial_physics_lora.safetensors`** y colocarlo en `models/loras/` |
| LoRA decorativa: pixel art de 16 bits | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | renombrar el archivo distribuido `16bit-pixel.safetensors` a **`minimax_h3_16bit_pixel_lora.safetensors`** y colocarlo en `models/loras/` |

Para una lista más exhaustiva y actualizada continuamente, consulta la lista
mantenida por la comunidad
[wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
(sigue más de 150 variantes de cuantización, fine-tunes y LoRAs).

## Pantalla de generación — un único diseño sin complicaciones

![Pantalla de generación](screenshots/generation.jpg)

Basta con cambiar entre las pestañas T2V (texto), I2V (imagen) y R2V (varias
referencias). La cola de la derecha reproduce el video en línea en cuanto un
trabajo se completa, y también puedes ver el progreso de los trabajos en curso,
así que la espera nunca se siente como tiempo perdido. La duración, la
resolución y la proporción de aspecto se ajustan al instante con deslizadores y
valores preestablecidos, mostrando en tiempo real la resolución real que se
enviará (por ejemplo, 1024×576 px).

## Funciones adicionales — elige opciones de aceleración con confianza

![Pantalla de funciones adicionales](screenshots/extra-features.jpg)

Cuantización (int8/w4a8), LoRAs de aceleración de la familia Turbo, aceleración
de VAE... hay muchas opciones alrededor de H3, y elegir una combinación
equivocada puede arruinar la generación. Esta pantalla **explica el efecto y
las advertencias de cada opción** justo al lado de su botón de selección, para
que nunca tengas que adivinar. T2V/I2V y R2V tienen cada uno su propia opción
de aceleración óptima, ya que las LoRAs compatibles varían según el modo de
generación.

## Integración LLM — gestiona la asistencia de prompts en un solo lugar

![Pantalla de integración LLM](screenshots/llm-integration.jpg)

Registra claves de API para xAI, OpenAI, Anthropic y Google Gemini, y
gestiónalas todas desde una única pantalla. Se destaca claramente que
**Google Gemini se puede usar completamente dentro de su nivel gratuito, sin
necesidad de tarjeta de crédito**, lo que reduce la barrera para probarlo
primero. Las claves registradas se muestran enmascaradas, y puedes asignar
libremente el rol de cada una: reescribir prompts en el formulario de
generación, o generar escenas por lotes para R2V.

## A quién le viene bien

- A quienes quieran generar videos con H3 solo mediante controles de
  formulario, sin tocar el grafo de nodos de ComfyUI
- A quienes prefieran no usar cuantización, LoRAs de aceleración y
  aceleración de VAE sin entender qué hace cada una
- A quienes quieran probar la asistencia de prompts alternando entre
  varias claves de API de LLM

## Descarga

El instalador de Windows (autocontenido, sin necesidad de runtime adicional)
está disponible en Releases.

## Comentarios y preguntas

Utiliza [Issues](../../issues) para informes de errores, solicitudes de
funciones o cualquier otra pregunta.
