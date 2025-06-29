# Optimasi Algoritma Deteksi Jatuh untuk Peningkatan Performa pada Kasus Olahraga Matras

### Skripsi Sarjana S1 | Universitas Multimedia Nusantara

Repositori ini berisi penelitian dan implementasi untuk skripsi sarjana yang berjudul "Optimasi Algoritma Deteksi Jatuh untuk Peningkatan Performa pada Kasus Olahraga Matras". Penelitian ini berfokus pada optimasi algoritma deteksi jatuh berbasis visi untuk mengurangi *false positive* yang disebabkan oleh aktivitas fisik yang kompleks.

---

## 📖 Abstrak

Penelitian ini menjawab limitasi utama dalam sistem deteksi jatuh berbasis visi modern: tingginya tingkat alarm palsu yang dipicu oleh aktivitas yang menyerupai jatuh, seperti olahraga matras. Berangkat dari replikasi algoritma performa tinggi oleh Tîrziu, dkk. (2024) yang memanfaatkan YOLOv7-W6-Pose, studi ini mengidentifikasi kerentanannya dalam membedakan antara insiden jatuh yang sebenarnya dan gerakan olahraga yang terstruktur. Untuk mengatasi hal ini, serangkaian optimasi diimplementasikan, meliputi penyesuaian parameter, pengenalan **Logika Temporal dan Pemulihan (*Recovery Logic*)**, serta modul kesadaran kontekstual untuk deteksi lingkungan. Algoritma final yang telah dioptimasi menunjukkan penurunan *false positive* yang drastis pada Dataset Olahraga Matras yang dibangun khusus, dan secara bersamaan, mencapai performa keseluruhan yang lebih unggul pada Dataset Le2i Fall standar, dengan **akurasi 96.15%** dan **presisi 100%**.

## 🎯 Latar Belakang Masalah dan Tujuan

Referensi utama dari penelitian ini adalah jurnal **"Enhanced Fall Detection Using YOLOv7-W6-Pose for Real-Time Elderly Monitoring"** oleh Tîrziu, dkk.[cite: 1345]. Meskipun algoritma aslinya sangat efektif untuk deteksi jatuh secara umum, penelitian tersebut mengakui adanya limitasi pada skenario yang melibatkan gerakan-gerakan kompleks.

Pengujian awal kami mengonfirmasi limitasi ini, di mana algoritma hasil replikasi hanya mencapai **akurasi 6.92%** pada dataset olahraga matras, karena salah mengklasifikasikan sebagian besar aktivitas sebagai jatuh.

Tujuan utama dari skripsi ini adalah:
1.  Mengimplementasikan optimasi yang terarah pada algoritma acuan untuk secara signifikan mengurangi tingkat *false positive* pada skenario olahraga matras.
2.  Mengukur peningkatan performa secara kuantitatif, baik pada kasus limitasi spesifik (olahraga matras) maupun pada dataset validasi umum (Le2i).

## 🛠️ Pendekatan Optimasi

Alih-alih melakukan pelatihan ulang (*retraining*) model *deep learning*, penelitian ini berfokus pada penyempurnaan logika pengambilan keputusan yang memproses *output* dari model YOLOv7-W6-Pose. Optimasi utama yang dilakukan meliputi:

* **Penyesuaian Parameter:** Nilai ambang batas (*threshold*) utama seperti kecepatan, rasio aspek, dan sudut tubuh dibuat lebih ketat untuk mengurangi sensitivitas terhadap gerakan yang bukan jatuh.
* **Logika Temporal dan Pemulihan (*Temporal & Recovery Logic*):** Sebuah mekanisme *stateful* baru diimplementasikan. Sistem kini mampu melacak urutan gerakan dan mengidentifikasi "event pemulihan" ketika pengguna kembali ke posisi normal setelah melakukan postur yang menyerupai jatuh. Jika beberapa *event* pemulihan terdeteksi, sistem secara cerdas mengklasifikasikan aktivitas tersebut sebagai olahraga, bukan insiden jatuh.
* **Deteksi Lingkungan Kontekstual:** Sebuah model YOLOv7 standar diimplementasikan secara terpisah untuk mendeteksi objek yang umum ditemukan di area gym (misalnya, 'sports ball', 'bench'). Fitur ini memungkinkan logika deteksi jatuh untuk menjadi lebih ketat secara adaptif ketika sistem mengenali bahwa ia berada di lingkungan olahraga.

## 🗂️ Dataset

Dua dataset utama digunakan untuk pengujian dan validasi dalam penelitian ini.

* **Le2i Fall Dataset:** Dataset publik standar untuk *benchmarking* sistem deteksi jatuh, berisi 130 video yang terdiri dari insiden jatuh dan aktivitas normal sehari-hari.
    * **[Unduh Dataset Le2i di Sini](https://drive.google.com/drive/folders/1NcEK32uX96QW_x_YPyBfRCOF33ZTxj8s?usp=drive_link)**

* **Dataset Olahraga Matras:** Dataset kustom yang terdiri dari 130 video yang dikumpulkan untuk penelitian ini. Dataset ini secara spesifik berisi 9 jenis aktivitas olahraga matras (seperti *push-up*, *abdominal curl*, *bird dog*) yang rentan memicu alarm palsu. Seluruh video dalam dataset ini dikategorikan sebagai "Non-Fall".
    * **[Unduh Dataset Olahraga Matras di Sini](https://drive.google.com/drive/folders/1zrMhEF3K0pOYBkKSMJCYBwKY6lhnPkMK?usp=drive_link)**

## 📊 Ringkasan Hasil Utama

Optimasi yang diimplementasikan menghasilkan peningkatan performa yang signifikan pada kedua dataset.

| Dataset | Metrik | Sebelum Optimasi (Replikasi) | Setelah Optimasi (Final) | Hasil |
| :--- | :--- | :--- | :--- | :--- |
| **Olahraga Matras** | Akurasi | 6.92% | **38.46%** | ✅ Penurunan *False Positive* secara drastis. |
| **Le2i Fall Dataset** | Akurasi | 92.31% | **96.15%** | ✅ Akurasi keseluruhan lebih tinggi. |
| | Presisi | 98.90% | **100.00%** | 🎉 **Nol alarm palsu** pada dataset umum. |
| | Recall | 90.91% | **94.95%** | ✅ Peningkatan sensitivitas, lebih sedikit kasus jatuh terlewat. |
| | Skor F1 | 94.74% | **97.41%** | ✅ Keseimbangan dan performa model lebih baik. |

## 📁 Isi Repositori

Repositori ini mencakup:

* `📄 Skripsi_Optimasi_..._FINAL.pdf`: Dokumen lengkap skripsi yang merinci penelitian dari awal hingga akhir.
* `💻 Notebooks_Jupyter/`: Folder yang berisi dua *notebook* utama:
    * `yolov7-w6pose-replicate-v13.ipynb`: Kode untuk replikasi awal dari algoritma referensi.
    * `yolov7-w6-pose-exercise-v7-refactored.ipynb`: Kode yang berisi algoritma final yang telah dioptimasi dengan semua logika baru.
* `🎥 Video_Hasil_Proses/`:
    * **[Tautan akan segera ditambahkan - video sedang dalam proses unggah.]**

## CARA MENGUTIP

Jika Anda menggunakan penelitian ini untuk pekerjaan Anda, silakan mengutip skripsi berikut:
