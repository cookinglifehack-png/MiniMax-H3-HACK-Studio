# MiniMax H3 HACK Studio

[English](README.md) | [日本語](README.ja.md) | [中文](README.zh.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Español](README.es.md) | **Bahasa Indonesia**

### ⬇️ [Unduh installer (Windows)](https://github.com/cookinglifehack-png/MiniMax-H3-HACK-Studio/releases/latest/download/MiniMax_H3_HACK_Studio_Setup.exe)

Kami mengemas model generasi video **MiniMax H3**, yang berjalan di atas
ComfyUI, menjadi aplikasi Windows yang langsung siap dipakai begitu diinstal.
Tidak perlu mengedit workflow ComfyUI, tidak perlu paham PHP — cukup klik dua
kali untuk membuka, lalu generasi video T2V, I2V, dan R2V sepenuhnya lewat
kontrol formulir ala browser.

## Pengaturan (ComfyUI + model MiniMax H3)

Installer menyertakan aplikasi itu sendiri, tetapi **tidak** menyertakan
ComfyUI atau bobot model MiniMax H3 — kamu tetap perlu instance ComfyUI
dengan model H3 terpasang, lalu mendaftarkannya sebagai backend dari layar
Pengaturan aplikasi. Unduh dari sumber-sumber di bawah ini dan tempatkan di
`ComfyUI/models/<folder>/` (nama file harus sama persis dengan yang diharapkan
aplikasi — jika menyiapkannya dengan agen/CLI AI, gunakan saja kolom "Nama
file" di bawah apa adanya).

### ComfyUI itu sendiri

- Panduan instalasi resmi (Windows portable): https://docs.comfy.org/installation/comfyui_portable_windows

### ① Minimum (sekadar agar H3 bisa jalan)

Semua berasal dari repositori resmi [`Comfy-Org/MiniMax-H3`](https://huggingface.co/Comfy-Org/MiniMax-H3).

| Peran | Nama file | Folder tujuan |
|---|---|---|
| Text encoder | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders/` |
| Audio VAE | `minimax_h3_audio_vae_fp32.safetensors` | `models/vae/` |
| Video VAE (fp16) | `minimax_h3_video_vae_fp16.safetensors` | `models/vae/` |
| UNet fl2va (kuantisasi int8) | `minimax_h3_fl2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |
| UNet ref2va (kuantisasi int8) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | `models/diffusion_models/` |

Dengan ini saja, T2V/I2V/R2V semuanya sudah berjalan (quant=int8, tanpa LoRA
akselerasi maupun TensorRT).

### ② Disarankan (konfigurasi tercepat hasil pengukuran)

Kombinasi ini paling cepat dalam pengukuran (baseline 442 detik → 182 detik,
pengurangan sekitar 59%). Tambahkan di atas ①.

| Peran | Sumber | Nama file / penempatan |
|---|---|---|
| LoRA Turbo versi pruned | https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI | `minimax_h3_turbo_v4_step600_ema_pruned_comfyui.safetensors` → `models/loras/` |
| Node Turbo | https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo | clone ke `custom_nodes/` (`MiniMaxH3TurboLoRA`/`MiniMaxH3TurboSampler`) |
| Node VAE TRT | https://github.com/lihaoyun6/ComfyUI-H3VAE_TRT | clone ke `custom_nodes/`, `pip install tensorrt` |
| Bobot ONNX untuk VAE TRT | https://huggingface.co/lihaoyun6/MiniMax-H3-VAE-ONNX | tempatkan di `models/vae/`, lalu kompilasi sekali ke `.engine` lewat node "MiniMax-H3 TRT VAE Compiler" (pilih decoder w4a16_awq VRAM rendah jika VRAM di bawah 12GB) |
| Node SageAttention | https://github.com/kijai/ComfyUI-KJNodes | clone ke `custom_nodes/` (`PathchSageAttentionKJ`) |
| SageAttention itu sendiri (wheel Windows) | https://github.com/woct0rdho/SageAttention/releases | `pip install triton-windows`, lalu pip install wheel yang sesuai versi PyTorch/CUDA kamu |

### ③ Semua (kuantisasi lain, metode akselerasi lain, dan LoRA dekoratif)

| Peran | Sumber | Nama file / penempatan |
|---|---|---|
| UNet kuantisasi w4a8 | https://huggingface.co/starsfriday/MiniMax-H3-w4a8 | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` / `minimax_h3_ref2va_pruned_w4a8_mixed.safetensors` → `models/diffusion_models/` |
| Video VAE kuantisasi int8 (build ConvRot dari Kijai) | https://huggingface.co/Comfy-Org/MiniMax-H3 (folder `vae/`) | `minimax_h3_video_vae_int8_convrot.safetensors` → `models/vae/` (butuh ComfyUI 0.31.0+) |
| Turbo versi lightx2v (file fl2v/ref2v terpisah) | https://huggingface.co/lightx2v/Minimax-h3-Turbo | `minimax_h3_fl2v_turbo_4step_v1.2_768p_comfyui_bf16.safetensors` / `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` → `models/loras/` |
| PDD-Acc (8 langkah, resmi) | Model: https://huggingface.co/alibaba-pai/MiniMax-H3-Acc-LoRAs<br>Node: https://github.com/Jalen-Brunson/ComfyUI-MiniMax-H3-PDD-Acc | `MiniMax-H3-FL2VA-Acc-8Step.safetensors` / `MiniMax-H3-Ref2VA-Acc-8Step.safetensors` → `models/pdd_acc/` |
| TaoMate (3 langkah) | https://huggingface.co/CZMartin22/TaoMate-H3-3step-ComfyUI | ganti nama file yang didistribusikan `TaoMate-H3-3step-ComfyUI.safetensors` menjadi **`minimax_h3_taomate_3step.safetensors`** lalu tempatkan di `models/loras/` (paling cocok untuk T2V/I2V — tidak disarankan untuk R2V, karena gambar referensi cenderung tidak terlalu berpengaruh) |
| Prompt rewriter (LLM lokal, opsional — tab "Integrasi LLM" di layar pengaturan menawarkan alternatif berbasis API key) | Node: https://github.com/pytraveler/MiniMax-H3-Prompt-Rewriter-ComfyUI<br>Adapter LoRA: https://huggingface.co/pytraveler/MiniMax-H3-Prompt-Rewriter-LoRA-GGUF | Lihat README node itu sendiri untuk model dasar GGUF (Qwen3.6-27B / Qwen3-VL-8B-Instruct / Qwen2.5-Omni-7B) |
| LoRA dekoratif: orang realistis | https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA | ganti nama file yang didistribusikan `h3-realism-people-t2v-i2v-r2v.safetensors` menjadi **`minimax_h3_realism_people_lora.safetensors`** lalu tempatkan di `models/loras/` |
| LoRA dekoratif: fisika spasial | https://huggingface.co/Jojocodex/minimax-h3-spatial-physics-lora | ganti nama file yang didistribusikan `wushu_spatial_physics_*_pruned.safetensors` (ada dua varian — belum jelas mana yang dipakai di sini) menjadi **`minimax_h3_spatial_physics_lora.safetensors`** lalu tempatkan di `models/loras/` |
| LoRA dekoratif: pixel art 16-bit | https://huggingface.co/KennethFal/16bit-pixel-lora-minimax-h3 | ganti nama file yang didistribusikan `16bit-pixel.safetensors` menjadi **`minimax_h3_16bit_pixel_lora.safetensors`** lalu tempatkan di `models/loras/` |

Untuk daftar yang lebih lengkap dan terus diperbarui, lihat daftar yang dikelola
komunitas [wildminder/awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)
(melacak 150+ varian kuantisasi, fine-tune, dan LoRA).

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
