# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | **Bahasa Indonesia**

### ⬇️ [Unduh installer (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

Kami mengemas model generasi video **MiniMax H3**, yang berjalan di atas
ComfyUI, menjadi aplikasi Windows yang langsung siap dipakai begitu diinstal.
Tidak perlu mengedit workflow ComfyUI, tidak perlu paham PHP — cukup klik dua
kali untuk membuka, lalu generasi video T2V, I2V, dan R2V sepenuhnya lewat
kontrol formulir ala browser.

## Layar generasi — satu tampilan yang tidak membingungkan

![Layar generasi](screenshots/generation.jpg)

Tinggal beralih antar tab T2V (teks), I2V (gambar), dan R2V (banyak referensi).
Antrean di sebelah kanan langsung memutar video begitu tugas selesai — tugas
yang masih berjalan pun terlihat progresnya, jadi menunggu tidak terasa
membuang waktu. Durasi, resolusi, dan rasio aspek semuanya bisa disesuaikan
langsung lewat slider dan preset, dengan resolusi sebenarnya yang akan dikirim
(misalnya 1024×576 px) ditampilkan secara real-time.

## Fitur Tambahan — pilih opsi akselerasi tanpa ragu

![Layar fitur tambahan](screenshots/extra-features.jpg)

Kuantisasi (int8/w4a8), LoRA akselerasi keluarga Turbo, akselerasi VAE — ada
banyak pilihan seputar H3, dan salah memilih kombinasi bisa merusak hasil
generasi. Layar ini **menjelaskan efek dan hal yang perlu diperhatikan untuk
tiap opsi** tepat di sebelah tombol pilihannya, jadi kamu tidak perlu menebak-
nebak. T2V/I2V dan R2V masing-masing punya pilihan akselerasi optimalnya
sendiri, karena LoRA yang didukung berbeda-beda tergantung mode generasi.

## Integrasi LLM — kelola bantuan prompt dalam satu tempat

![Layar integrasi LLM](screenshots/llm-integration.jpg)

Daftarkan API key untuk xAI, OpenAI, Anthropic, dan Google Gemini, lalu kelola
semuanya dari satu layar. Ditonjolkan dengan jelas bahwa **Google Gemini bisa
dipakai sepenuhnya dalam batas gratisnya, tanpa perlu kartu kredit** — jadi
mudah untuk dicoba lebih dulu. Key yang terdaftar ditampilkan tersamar, dan
kamu bisa bebas menetapkan peran masing-masing: menulis ulang prompt di
formulir generasi, atau membuat adegan secara batch untuk R2V.

## Cocok untuk

- Orang yang ingin membuat video H3 hanya lewat kontrol formulir, tanpa
  menyentuh node graph ComfyUI
- Orang yang tidak ingin memakai kuantisasi, LoRA akselerasi, dan akselerasi
  VAE tanpa memahami fungsi masing-masing
- Orang yang ingin mencoba bantuan prompt sambil berpindah-pindah antar
  beberapa API key LLM

## Unduh

Installer Windows (mandiri sepenuhnya, tanpa perlu runtime tambahan) tersedia
di Releases.
