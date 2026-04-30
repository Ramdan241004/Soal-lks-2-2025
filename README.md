# Soal-lks-2-2025

# Description of project and tasks
Modul ini berdurasi 5 jam - **Hari 1 – Proyek 1: Sistem Respons Insiden**.
Dalam proyek ini, Anda akan mengambil peran sebagai Cloud Operations Engineer yang bertanggung jawab untuk merancang dan mengorkestrasi Sistem Respons Insiden yang terintegrasi dengan merancang sistem yang aman, dapat diskalakan, dan akurat, yang didukung oleh Generative AI. Sistem ini harus mampu secara proaktif mendeteksi dan melacak insiden infrastruktur, terutama yang dapat menyebabkan error atau gangguan kritis. Sistem ini juga harus melacak siklus hidup setiap insiden dari deteksi hingga penyelesaian, menyediakan visibilitas dan akuntabilitas penuh. Dengan dukungan AI, sistem harus dapat merangkum insiden, menghasilkan laporan yang informatif, dan membantu dalam analisis akar penyebab, sehingga mempercepat respons operasional dan mengurangi upaya manual dalam situasi bertekanan tinggi.

# Task
1. Baca dokumentasi dengan saksama (tercantum di bawah).
2. Harap baca dengan cermat bagian **detail teknis.**
3. Harap baca dengan cermat **detail proyek.**
4. Login ke AWS Console.
5. Harap baca dengan cermat PoC untuk memahami langkah demi langkah.
6. Lakukan pengujian menyeluruh terhadap seluruh infrastruktur dan pastikan semuanya beroperasi sesuai yang diinginkan.
7. Konfigurasikan monitoring aplikasi yang diperlukan dan metrik di CloudWatch.

# Technical Details
1. Region yang tersedia untuk proyek ini adalah **us-east-1 (N. Virginia).**
2. Gunakan **LabRole** untuk semua kebutuhan IAM Role di setiap service, termasuk instance EC2 di mana Anda dapat menggunakan **LabInstanceProfile.**
3. Semua source code resource yang diperlukan tersedia di GitHub Repository di https://github.com/betuah/lks-insident-response.
4. Sistem operasi yang diperbolehkan untuk proyek ini adalah Ubuntu, dengan versi minimum 24.04.
5. Setiap service yang Anda buat harus mengikuti format penamaan dengan prefix **"lks-"**, sebagai contoh, "lks-vpc-zone-a", "lks-api-gateway", dan seterusnya. Juri hanya akan meninjau service yang menggunakan konvensi penamaan ini.
6. Saat menjalankan LLM, Anda mungkin akan mengalami respons yang lebih lambat karena instance maksimum yang tersedia adalah tipe **Large,** sedangkan komputasi LLM akan bekerja lebih baik dengan GPU atau vCPU yang lebih besar. Ini bukan masalah; cukup pastikan bahwa LLM beroperasi dengan benar dan tetap dapat diakses, meskipun lambat.
7. Pastikan Anda memberikan label pada setiap service AWS yang Anda buat, kecuali untuk service yang dibuat secara otomatis. Memperhatikan detail ini akan berkontribusi dalam mendapatkan poin lebih.
8. Ingat untuk selalu memberikan deskripsi yang jelas agar pekerjaan Anda dapat dengan mudah dipahami. Hal ini mungkin akan memberikan Anda poin tambahan.
9. Sebelum proyek berakhir, tinjau kembali pekerjaan Anda dan hapus semua service yang tidak diperlukan untuk menghindari kebingungan terhadap hasil pekerjaan Anda. Hal ini akan membantu Anda menghindari kehilangan banyak poin.
10. Bahasa pemrograman yang digunakan dalam proyek ini adalah golang, javascript dan python.

# Project Overview
Dalam proyek studi kasus ini, peserta akan mengambil peran sebagai Cloud Operations Engineer yang bertugas membangun Sistem Respons Insiden yang aman, dapat diskalakan, dan terintegrasi AI di AWS. Tujuan utama adalah untuk secara proaktif mendeteksi, menganalisis, dan merespons insiden infrastruktur—baik melalui tindakan manual maupun mekanisme pemulihan otomatis. Dengan memanfaatkan tools seperti Amazon CloudWatch, Step Functions, Lambda, DynamoDB, dan Generative AI (LLM), peserta akan membangun sistem yang tidak hanya mendeteksi dan merespons insiden tetapi juga belajar dari insiden tersebut dengan menghasilkan data yang telah di-vektorisasi untuk analisis di masa depan dan kecerdasan operasional.

# Project Details
Untuk menyediakan jalur implementasi yang jelas dan terorganisir, proyek ini dibagi menjadi beberapa Proof of Concept dan kasus teknis yang harus diselesaikan oleh setiap peserta. Anda diharapkan untuk mengikuti struktur tersebut dan menyelesaikan setiap fase sesuai dengan ketentuan. Semua resource yang dibutuhkan untuk proyek ini tersedia di https://github.com/betuah/lks-insident-response. Repository tersebut memiliki 4 direktori yaitu:
- **Apps** directory Berisi aplikasi inti yang dibutuhkan untuk proyek ini:
  - **loadsim**: Aplikasi simulasi berbasis Go yang harus dikompilasi menjadi binary dan di-deploy sebagai **service systemd** pada instance EC2 target. Aplikasi ini mampu mensimulasikan CPU Spike, Memory Spike dan error berbasis log (contohnya, CRASH, SHUTDOWN, ERROR).
  - **irs-fe**: Dashboard frontend untuk memonitor dan memvisualisasikan insiden.
  - **irs-be**: Service backend untuk mendukung dashboard insiden dan integrasi dengan resource AWS.Kedua `irs-fe` dan `irs-be` sudah menyertakan **Dockerfile** dan dimaksudkan untuk **di-build menjadi image di ECR** dan **di-deploy ke Amazon EKS.**
- **Function** directory Berisi semua AWS Lambda function yang digunakan di seluruh pipeline deteksi dan penanganan insiden. Setiap Lambda memiliki tujuan masing-masing dan akan dijelaskan secara rinci pada bagian Lambda.
- **Kubernetes** directory Berisi **manifest Kubernetes** untuk melakukan deployment komponen monitoring (irs-fe dan irs-be) ke EKS. Peserta mungkin perlu menyesuaikan manifest ini berdasarkan path image ECR dan konfigurasi environment.
- **Infrastructure** dtermasuk Berisi script **Infrastructure-as-Code (IaC)** untuk melakukan provisioning infrastruktur dasar, termasuk:
  - Sebuah bastion host (juga berfungsi sebagai VPN menggunakan WireGuard dan NAT instance).
  - Sebuah instance EC2 private untuk hosting LLM, digunakan untuk menghasilkan laporan insiden berbasis AI.
  - Sebuah network dengan 2 subnet public dan 2 subnet private dengan NAT instance.

> **Catatan** : Instance EC2 LLM membutuhkan akses internet keluar untuk provisioning dan konfigurasi awal.
Setiap folder menyertakan file `README.md` dengan instruksi setup dan penggunaan secara rinci.
