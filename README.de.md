# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | **Deutsch** | [Español](README.es.md) | [Bahasa Indonesia](README.id.md)

### ⬇️ [Installer herunterladen (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

Wir haben das auf ComfyUI laufende Videogenerierungsmodell **MiniMax H3** in eine
Windows-App verpackt, die sofort nach der Installation einsatzbereit ist. Kein
Bearbeiten von ComfyUI-Workflows, keine PHP-Kenntnisse nötig — einfach
doppelklicken zum Starten, danach läuft die gesamte T2V-, I2V- und
R2V-Videogenerierung über Formularsteuerung wie im Browser.

## Generierungsbildschirm — ein einziger, klarer Bildschirm

![Generierungsbildschirm](screenshots/generation.jpg)

Einfach zwischen T2V (Text), I2V (Bild) und R2V (mehrere Referenzen) per Tab
wechseln. In der Warteschlange rechts wird ein fertiges Video sofort inline
abgespielt, sobald der Job abgeschlossen ist — auch laufende Jobs zeigen ihren
Fortschritt, sodass sich die Wartezeit nie wie verlorene Zeit anfühlt. Dauer,
Auflösung und Seitenverhältnis lassen sich sofort über Schieberegler und Presets
anpassen, mit der tatsächlich übermittelten Auflösung (z. B. 1024×576 px) in
Echtzeit angezeigt.

## Zusatzfunktionen — Beschleunigungsoptionen ohne Rätselraten wählen

![Zusatzfunktionen-Bildschirm](screenshots/extra-features.jpg)

Quantisierung (int8/w4a8), Beschleunigungs-LoRAs der Turbo-Familie, VAE-
Beschleunigung — rund um H3 gibt es viele Optionen, und eine falsche Kombination
kann die Generierung zerstören. Dieser Bildschirm **erklärt direkt neben jeder
Option deren Wirkung und Fallstricke**, sodass du nie raten musst. T2V/I2V und
R2V haben jeweils ihre eigene optimale Beschleunigungswahl, da die
unterstützten LoRAs je nach Generierungsmodus unterschiedlich sind.

## LLM-Integration — Prompt-Unterstützung an einem Ort verwalten

![LLM-Integration-Bildschirm](screenshots/llm-integration.jpg)

Registriere API-Schlüssel für xAI, OpenAI, Anthropic und Google Gemini und
verwalte sie alle von einem einzigen Bildschirm aus. Deutlich hervorgehoben
wird, dass **Google Gemini vollständig im Rahmen seines kostenlosen Kontingents
genutzt werden kann, ohne Kreditkartenregistrierung** — eine niedrige
Einstiegshürde zum Ausprobieren. Registrierte Schlüssel werden maskiert
angezeigt, und du kannst jedem frei eine Rolle zuweisen: Umschreiben von
Prompts im Generierungsformular oder stapelweises Erzeugen von Szenen für R2V.

## Für wen das geeignet ist

- Für alle, die H3-Videos allein über Formularsteuerung generieren möchten,
  ohne den Node-Graph von ComfyUI anzufassen
- Für alle, die Quantisierung, Beschleunigungs-LoRAs und VAE-Beschleunigung
  nicht verwenden möchten, ohne zu verstehen, was jede einzelne bewirkt
- Für alle, die Prompt-Unterstützung ausprobieren möchten, während sie
  zwischen mehreren LLM-API-Schlüsseln wechseln

## Download

Der Windows-Installer (in sich geschlossen, keine zusätzliche Laufzeitumgebung
nötig) ist über Releases erhältlich.
