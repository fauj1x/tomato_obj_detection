# tomato_obj_detection

DESKRIPSI
------------------
*ID

Proyek ini bertujuan untuk mendeteksi kondisi dan kematangan buah tomat 
menggunakan algoritma Deep Learning YOLOv8 (You Only Look Once version 8). 
Model dilatih untuk mengenali empat kategori spesifik dalam budidaya tomat.

*EN

This project aims to detect the condition and ripeness of tomatoes using the YOLOv8 (You Only Look Once version 8) deep learning algorithm.
The model was trained to recognize four specific categories in tomato cultivation.

STACK TEKNOLOGI
------------------
- Bahasa Pemrograman: Python
- Framework ML: Ultralytics YOLOv8, PyTorch
- Dataset Management: Roboflow
- Environment: Google Colab (dengan dukungan GPU Tesla T4)
- Penyimpanan: Google Drive (Integrasi Cloud)

STRUKTUR DATASET & LABEL
---------------------------
Dataset diunduh melalui Roboflow dengan format YOLOv8. 
Terdapat 4 kelas yang dideteksi:
1. Batang (Stem)
2. Busuk (Rotten)
3. Mentah (Unripe)
4. matang (Ripe)

Lokasi dataset: /content/drive/MyDrive/dataset/tomat-yoloo-1/

TAHAPAN EKSEKUSI
-------------------

A. Persiapan Lingkungan (Setup)
   - Melakukan mounting Google Drive untuk akses dataset dan penyimpanan model.
   - Instalasi library utama: `ultralytics`, `roboflow`, `torch`, dan `opencv-python`.
   - Melakukan verifikasi perangkat GPU menggunakan `nvidia-smi`.

B. Pelatihan Model (Training)
   - Arsitektur: YOLOv8 Nano (yolov8n.pt) - Dipilih karena ringan dan cepat.
   - Konfigurasi Training:
     * Epochs: 100
     * Image Size: 640x640 piksel
     * Optimizer: AdamW (Automatic detection)
     * Device: CUDA (GPU)
   - Hasil training (weights, plot, dan log) disimpan secara otomatis di:
     `/content/drive/MyDrive/dataset/runs/detect/train/`

C. Evaluasi Model
   - Model terbaik disimpan dengan nama `best.pt`.
   - Berdasarkan hasil validasi terakhir:
     * Mean Average Precision (mAP50): 0.825 (82.5%)
     * Performa terbaik ditemukan pada deteksi kelas 'Busuk' dan 'Batang'.

D. Prediksi (Inference)
   - Model yang telah dilatih (`best.pt`) digunakan untuk mendeteksi gambar baru.
   - Contoh input: `/content/drive/MyDrive/dataset/coba.jpg`
   - Output: Gambar hasil deteksi dengan bounding box dan label probabilitas, 
     disimpan di folder `runs/detect/predict/`.

STRUKTUR DIREKTORI OUTPUT
----------------------------
dataset/
├── tomat-yoloo-1/ (Dataset source)
├── runs/
│   ├── detect/
│   │   ├── train/ (Log pelatihan & weight best.pt)
│   │   └── predict/ (Hasil prediksi gambar test)
└── coba.jpg (Gambar uji coba)

CARA PENGGUNAAN SINGKAT
--------------------------
Untuk melakukan deteksi pada gambar baru menggunakan model ini:

```python
from ultralytics import YOLO

# 1. Load model yang sudah dilatih
model = YOLO('path/to/best.pt')

# 2. Jalankan prediksi
results = model.predict(source='gambar_tomat.jpg', save=True, conf=0.25)
