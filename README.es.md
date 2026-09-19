# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | **Español** | [Bahasa Indonesia](README.id.md)

Hemos convertido el modelo de generación de video **MiniMax H3**, que se ejecuta
sobre ComfyUI, en una aplicación de Windows lista para usar en cuanto la
instalas. No hace falta editar workflows de ComfyUI ni tener conocimientos de
PHP: solo haz doble clic para iniciarla, y luego genera videos T2V, I2V y R2V
completamente mediante controles de formulario, como en un navegador.

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
