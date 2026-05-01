# Soal-lks-2-2025

Username: 165111-7
Password: EVSnoujuDb

Username: 165111-6
Password: UNfBbwh6pN

Username: 165111-13
Password: 43lk8Y0qTi
# Description of project and tasks
Modul ini berdurasi 5 jam - **Hari 1 – Proyek 1: Sistem Respons Insiden**.<br>
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
Siap, aku lanjutkan FULL tanpa diringkas dan tetap mempertahankan struktur aslinya.

### PoC 1: Incident Detection Workflow
Pada proof of concept pertama (PoC 1), alur penanganan insiden dimulai dengan menginstal **CloudWatch Agent** pada instance EC2 target. Agent ini mengumpulkan metrik resource sistem dan log aplikasi, serta membuat **CloudWatch log group** baru. Log filter dikonfigurasi untuk mendeteksi kata kunci tertentu seperti "**APP_CRASH**," "**APP_ERROR**," dan "**SHUTDOWN**," yang membantu mengidentifikasi masalah pada level aplikasi. Selain itu, metric filter digunakan untuk memantau penggunaan CPU di atas 70% dan penggunaan memori di atas 80%. Ketika ambang batas ini terlampaui atau entri log yang sesuai ditemukan, CloudWatch Alarm akan terpicu dan memanggil **AWS Step Function**. Step Function ini mengorkestrasi proses penanganan insiden dengan membuat record insiden baru di DynamoDB, menghasilkan ringkasan laporan dan saran tindakan menggunakan large language model (LLM), dan mengirimkan notifikasi. Notifikasi tersebut mencakup opsi konfirmasi untuk penanganan manual atau otomatis terhadap insiden, tergantung pada jenis insiden dan kebijakan yang telah ditentukan sebelumnya.


### PoC 2: Incident Handling and Action
Untuk memungkinkan mekanisme respons insiden yang mulus dan fleksibel yang mendukung baik tindakan otomatis maupun manual berdasarkan jenis insiden dan keputusan responden, yang diorkestrasi melalui layanan AWS.

**Workflow Summary:**
- **Trigger via Action Button (Manual/Auto):**
Alur respons dimulai ketika pengguna mengklik tombol aksi (di UI atau notifikasi) atau ketika aturan otomatis terpicu. Hal ini mengirimkan incident_id dan mode yang dipilih (manual atau auto) melalui API Gateway ke Lambda function khusus.

- **Action Routing:**
Lambda function tersebut memicu **Incident Action Step Function** yang mengarahkan logika berdasarkan jenis insiden. Desain ini mendukung percabangan keputusan yang dapat diskalakan dan memungkinkan fleksibilitas dalam mendefinisikan aturan aksi baru seiring waktu. Dalam mode otomatis, sistem melakukan langkah mitigasi yang telah ditentukan berdasarkan jenis insiden:<br>
**HIGH_CPU:** Meluncurkan instance EC2 pengganti dengan vCPU yang ditingkatkan.<br>
**HIGH_MEM**: Meluncurkan instance EC2 pengganti dengan memori yang lebih besar.<br>
**APP_CRASH:** Me-restart service yang relevan.<br>
**APP_SHUTDOWN**: Mencoba pemulihan penuh atau memulai strategi fallback.<br>
Namun, beberapa jenis insiden tidak didukung untuk penanganan otomatis. Dalam kasus tersebut, sistem harus memberikan notifikasi bahwa tindakan tidak dapat dilakukan secara otomatis, dan data insiden harus diperbarui secara manual. Lambda function telah disiapkan untuk menangani hal ini. Untuk informasi lebih lanjut, silakan merujuk ke dokumentasi AWS Lambda dan bagian AWS Step Function.

- **Success and Error:**
Jika mode diatur ke manual atau jenis insiden tidak dapat diselesaikan secara otomatis, insiden akan ditandai sebagai **pending_action**. **Notifikasi konfirmasi** dikirim ke pengguna atau grup yang bertanggung jawab melalui SNS.

- **State Update:**
Setelah tindakan—baik manual maupun otomatis—selesai, status insiden diperbarui di DynamoDB. Notifikasi akhir dikirim dengan hasil (success, failed, needs follow-up) untuk memberikan transparansi dan kemampuan audit.

### PoC 3: Incident Vectorization for AI Integration
Untuk membangun pipeline setelah penyelesaian yang mengubah insiden yang telah diselesaikan menjadi representasi vektor untuk memori generative AI dan fungsi asisten DevOps.

**Workflow Summary**:
1. **Stream Activation from DynamoDB**:
    - DynamoDB stream diaktifkan untuk menangkap perubahan **secara real-time**, terutama ketika insiden ditandai sebagai resolved.

2. **Data Stream Processing**:
    - Data dialirkan melalui **Kinesis (atau stream serupa)** ke komponen filtering yang memastikan hanya insiden yang telah selesai (solved) yang diproses. Nama atribut adalah `status` dan nilainya harus salah satu dari `solved` atau `done`. Nilai selain `solved` dan `done` tidak dapat diproses.

3. **Filtering & Transformation**:
    - Sebuah mekanisme filter memvalidasi jenis insiden, status penyelesaian, dan mengekstrak informasi yang bermakna sebelum dilakukan vectorization.
    - Gunakan event bridge pipeline.

4. **Vectorization Lambda**:
    - Sebuah Lambda function khusus mengubah data ini menjadi vector embeddings (misalnya, menggunakan Sentence Transformers atau model serupa).

5. **Vector Storage**:
    - Embedding kemudian disimpan dalam **Vector Database (misalnya, pgvector atau yang serupa)** untuk memungkinkan pencarian semantik dan bantuan AI chat yang sadar konteks dalam penyelesaian insiden di masa depan.

### Step Function
Sistem manajemen insiden menggunakan AWS Step Functions untuk mengorkestrasi workflow otomatis untuk pembuatan dan penyelesaian insiden. Ini dipisahkan menjadi dua state utama: incident-creation dan incident-handling. Berikut adalah rincian setiap langkah:

1. Lks-incident-creation-state:

• Memicu Lambda function untuk membuat insiden dan menyimpan datanya di DynamoDB.
• Menghasilkan laporan insiden dengan dukungan dari model AI (berdasarkan log dan metrik).
• Mengirimkan notifikasi konfirmasi kepada pengguna sebelum melanjutkan ke fase berikutnya.


---

2. Lks-incident-handling-state:

• Ketika dipicu, Step Function membaca incident_type untuk menentukan apakah auto self-healing dapat dilakukan. Nilai incident_type yang didukung:

HIGH_CPU – Meluncurkan instance EC2 pengganti dengan vCPU yang ditingkatkan.

HIGH_MEM – Meluncurkan instance EC2 pengganti dengan memori yang lebih besar.

APP_CRASH – Me-restart service yang relevan.

APP_SHUTDOWN – Mencoba pemulihan penuh atau memulai strategi fallback.


Jika incident_type tidak didukung, Step Function memperbarui status action insiden di DynamoDB menjadi manual.


---

• Memanggil Lambda yang relevan, yang melakukan remediasi otomatis dan mengembalikan status: success atau failed.

Jika success → Secara otomatis memperbarui status insiden menjadi solved atau done, kemudian menandai eksekusi Step Function sebagai success.

Jika error → Membaca output error, membuat file log di bucket S3 yang sudah ada pada folder logs/incident_handle_error.log dan isi file tersebut berisi detail output error.



---

Kirim aja lanjutannya, nanti aku teruskan sampai selesai dengan format yang sama 👍
Siap, ini aku lanjutkan FULL translation tanpa diringkas sama sekali, mengikuti struktur aslinya:


---

Simple Notification Service

Sistem menggunakan satu topik SNS terpusat bernama lks-incident-topic untuk menyiarkan notifikasi terkait insiden. Topik ini bertanggung jawab untuk mengirimkan pesan pada setiap tahap penting dalam siklus hidup insiden. Topik ini harus memiliki subscription email yang dikonfigurasi untuk betuah@seamolec.org dan semua email peserta, sehingga memungkinkan pembaruan tepat waktu kepada penyelenggara dan peserta.


---

Simple Storage Service

Amazon S3 digunakan sebagai penyimpanan terpusat untuk data log dan workspace peserta yang terkait dengan Infrastructure as Code (IaC) dan Kubernetes. Buat bucket dengan format nama lksn2025-<your_name>-<your_province>.

Struktur Folder & Penggunaan:

• infrastructure/ – Upload semua file IaC ke folder ini.
• kubernetes/ – Upload semua file terkait Kubernetes ke folder ini.
• logs/ – Simpan semua log sistem yang dihasilkan oleh sistem atau Step Functions di folder ini.

Struktur ini memastikan penyimpanan yang terorganisir dan kemudahan dalam mengambil kembali workspace peserta, manifest Kubernetes, dan log sistem.


---

Relational Database

Vector Database memainkan peran penting dalam memungkinkan similarity search dan pemahaman kontekstual terhadap insiden berdasarkan pola historis. Hal ini memungkinkan sistem untuk mengambil insiden terkait di masa lalu atau menghasilkan saran yang sadar konteks menggunakan semantic embeddings. Dalam proyek ini, pipeline vectorization bertanggung jawab untuk mengubah data insiden terstruktur (title, description, report, suggestions, dll.) menjadi representasi vektor menggunakan model embedding. Embedding ini disimpan dalam database yang kompatibel dengan PostgreSQL dengan extension pgvector yang diaktifkan. Melakukan provisioning relational database menggunakan Infrastructure as Code (IaC) akan menambah nilai poin, cukup tambahkan stack baru di dalam direktori infrastructure untuk service Postgres.


---

Relational Database (DynamoDB)

Bagian ini menjelaskan requirement untuk mengonfigurasi Amazon DynamoDB sebagai data store utama untuk record insiden dalam Incident Response System. Peserta bertanggung jawab untuk memastikan tabel DynamoDB diprovision dan dikonfigurasi dengan benar untuk mendukung siklus hidup insiden dan integrasi AI selanjutnya.


---

Requirements:

• Incident Data Storage: Tabel DynamoDB harus diprovision untuk menyimpan semua data terkait insiden. Tabel ini akan berfungsi sebagai repositori pusat untuk melacak insiden dari deteksi hingga penyelesaian.

• Table Naming Convention: Secara default, aplikasi monitoring mengharapkan tabel insiden bernama incident. Namun, peserta memiliki fleksibilitas untuk memilih nama tabel yang berbeda jika diinginkan, dengan syarat konfigurasi aplikasi diperbarui agar sesuai dengan nama yang dipilih.

• Primary Key Configuration: Primary key tabel harus dikonfigurasi dengan id sebagai Partition Key. Hal ini memastikan pengambilan dan pengelolaan record insiden secara efisien.


---

ECR & EKS

Proyek ini menggunakan Amazon ECR (Elastic Container Registry) untuk menyimpan container image untuk IRS-FE (Frontend) dan IRS-BE (Backend) dengan namespace lks/irs atau lks/irsbe, memastikan semua image memiliki version control dan dikelola secara aman. Image ini di-deploy ke Amazon EKS (Elastic Kubernetes Service), yang berfungsi sebagai environment Kubernetes terkelola untuk mengorkestrasi komponen monitoring. Manifest Kubernetes yang diperlukan untuk deployment sistem ke EKS sudah tersedia di repository, namun manifest tersebut mungkin belum dikonfigurasi untuk ingress. Baik ECR maupun EKS harus dikonfigurasi untuk private access guna memastikan komunikasi yang aman dan membatasi eksposur ke jaringan publik.


---

Lambda Function

AWS Lambda memainkan peran utama dalam mengorkestrasi baik langkah otomatis maupun manual dalam workflow respons insiden. Dalam Proof of Concept (PoC) ini, berbagai Lambda function bertanggung jawab untuk menangani ingestion data insiden, routing keputusan berdasarkan jenis insiden dan mode eksekusi, memicu workflow, serta melakukan mitigasi di level AWS seperti me-restart instance atau melakukan scaling resource. Semua kode Lambda dan konfigurasi yang digunakan dalam proyek ini tersedia secara publik di repository berikut:
https://github.com/betuah/lks-insident-respons.


---

API Gateway

API Gateway memainkan peran penting dalam memungkinkan feedback pengguna yang aman dan terstruktur dalam workflow respons insiden. API Gateway bertindak sebagai interface di mana pengguna (biasanya DevOps engineer atau system administrator) dapat merespons notifikasi insiden yang dikirim melalui email, yang dipicu oleh sistem orkestrasi insiden.

API Gateway menyediakan endpoint /action untuk menangani feedback pengguna dari email notifikasi insiden. Endpoint ini menerima dua parameter:

• id: ID unik insiden (dari DynamoDB).
• action: bernilai "auto" atau "manual".

Ketika dipicu, endpoint ini akan memanggil Lambda function yang:

• Memvalidasi input.
• Mengambil record insiden dari DynamoDB.
• Jika action = "auto": memulai Step Function untuk melakukan self-healing berdasarkan jenis insiden.
• Jika action = "manual": memperbarui status insiden dan field action untuk mencerminkan penanganan manual.


---

CloudWatch Monitoring & Alarm

CloudWatch digunakan untuk memonitor performa sistem dan kesehatan aplikasi secara real-time. Dalam proyek ini, beberapa alarm dikonfigurasi untuk mendeteksi event kritis atau kegagalan sistem, dan memberi notifikasi ke handler respons insiden melalui AWS Lambda function.

CloudWatch Alarms berikut harus dibuat:

CPU_HIGH – Sumber: EC2 Metrics – Terpicu ketika penggunaan CPU EC2 melebihi 70%

MEM_HIGH – Sumber: CW Agent Metrics – Terpicu ketika penggunaan memori melebihi 80%

APP_CRASH – Sumber: Log Filter – Terpicu ketika pola crash terdeteksi dalam log

APP_ERROR – Sumber: Log Filter – Terpicu ketika log dengan level "error" terdeteksi

APP_SHUTDOWN – Sumber: Log Filter – Terpicu ketika log terkait shutdown aplikasi terdeteksi (misalnya, "graceful shutdown")


CloudWatch Agent diperlukan untuk mengumpulkan metrik tambahan seperti penggunaan memori, penggunaan CPU, dan semua log aplikasi. Untuk mempermudah deployment, script user-data.sh yang disediakan dalam repository proyek sudah mencakup perintah untuk menginstal, mengonfigurasi, dan menjalankan CloudWatch Agent pada instance EC2 target.


---

Kalau masih ada bagian lain (misalnya diagram, tambahan PoC, atau instruksi deployment), kirim saja—aku lanjutkan dengan gaya full translation tanpa diringkas 👍

Siap, ini aku lanjutkan FULL translation tanpa diringkas sama sekali, tetap mengikuti struktur asli:


---

Infrastructure as Code (IaC) Challenge

Bagian ini menantang peserta untuk menunjukkan kemampuan mereka dalam Infrastructure as Code (IaC) dengan menganalisis, melakukan debugging, dan memperluas codebase Terraform yang diberikan untuk melakukan provisioning dan konfigurasi infrastruktur respons insiden yang kuat di AWS. Meskipun setup IaC awal sudah menyediakan fondasi, peserta harus mengantisipasi bahwa sistem tersebut mungkin tidak langsung berjalan sempurna dan akan membutuhkan kemampuan analisis serta pemecahan masalah untuk mengidentifikasi dan memperbaiki isu yang ada.


---

Context:

Sebuah codebase Terraform yang sudah ada telah disediakan, yang sudah menangani provisioning komponen infrastruktur inti. Ini mencakup setup network (VPC, subnet, NAT instance), sebuah bastion host, dan sebuah instance EC2 untuk LLM, beserta security group masing-masing.

Di dalam repository IaC, Anda akan menemukan direktori modules dan stacks:

modules/ berisi template infrastruktur yang dapat digunakan kembali (reusable) yang dapat digunakan di berbagai stack dan berfokus pada satu resource atau solusi spesifik. Module yang sudah ada tidak boleh dimodifikasi, cukup tambahkan module baru jika Anda membutuhkan service yang belum tersedia.

stacks/ berisi konfigurasi spesifik untuk environment atau service yang memanggil module. Setiap service atau environment baru ditambahkan di sini untuk menjaga agar service tetap terorganisir dan terisolasi.


Peserta diharapkan bekerja dalam struktur yang sudah ada ini dan memperluasnya.


---

Tasks:

1. IaC Analysis and Debugging (Existing Infrastructure):

• Analisis kode Terraform yang disediakan untuk komponen infrastruktur yang sudah ada (network, bastion, LLM EC2, dan security group). Diharapkan bahwa IaC awal mungkin tidak berjalan sesuai yang diharapkan atau memerlukan penyesuaian kecil agar dapat berfungsi sepenuhnya.

• Pastikan LLM telah mengunduh model phi4-mini, silakan lihat dokumentasi ollama berikut (https://github.com/ollama/ollama/blob/main/docs/api.md) untuk menguji dan melakukan pull model dari endpoint API.

• Identifikasi dan perbaiki setiap masalah yang mencegah provisioning infrastruktur yang ada berjalan dengan sukses.

• Pastikan network diprovision dengan benar dengan:
o 2 public subnet.
o 2 private subnet.
o Sebuah NAT instance untuk memungkinkan akses internet keluar bagi resource di private subnet.

• Verifikasi bahwa bastion host dikonfigurasi untuk bertindak sebagai VPN server menggunakan WireGuard dan berfungsi sebagai NAT instance untuk private subnet.

• Pastikan wg-easy telah terinstal pada bastion host, dapat diakses melalui UI pada port yang telah ditentukan, dan password untuk UI wg-easy adalah lkspass.


---

• Participant Instructions for WireGuard Client:

o Peserta diwajibkan untuk mengunduh dan menginstal WireGuard client pada mesin lokal mereka dari:
https://www.wireguard.com/install/

o Setelah bastion host diprovision, peserta harus mengakses UI wg-easy (menggunakan public IP dari bastion host dan port yang telah dikonfigurasi, dengan password lkspass) untuk mengunduh file konfigurasi tunnel WireGuard.

o Import file konfigurasi yang telah diunduh tersebut ke dalam WireGuard client mereka untuk membangun koneksi yang aman ke private network.


---

2. Target EC2 Instance Provisioning (NEW STACK):

• Buat stack Terraform baru di dalam direktori stacks yang telah ditentukan untuk instance EC2 target. Beri nama stack ini dengan sesuai (misalnya, target-ec2).

• Instance EC2 ini harus memenuhi spesifikasi berikut:

• Instance Type: t3.micro
• Operating System: Ubuntu 24.04
• Application: Install aplikasi loadsim. Source code untuk loadsim tersedia di repository yang disediakan pada folder apps/loadsim. Ikuti instruksi build dalam direktori tersebut.

• Go Language: Pastikan bahasa Go (minimal versi 1.21) terinstal pada instance, karena loadsim memerlukannya.

• CloudWatch Agent Installation and Configuration: Script user_data.sh (yang disebutkan di bawah) akan menangani instalasi dan konfigurasi CloudWatch Agent, termasuk setup pengumpulan log untuk loadsim.

• User Data Script: Script user_data.sh yang diperlukan untuk instance ini mencakup perintah untuk build loadsim, instalasi Go, setup service systemd, dan konfigurasi CloudWatch Agent. Peserta harus mengintegrasikan script ini ke dalam stack Terraform baru untuk instance EC2 target. Anda mungkin perlu memodifikasi script ini sesuai dengan kebutuhan konfigurasi Anda.


---

3. Database Stack Provisioning (NEW STACK):

• Buat stack Terraform baru lainnya di dalam direktori stacks yang telah ditentukan untuk database. Beri nama stack ini dengan sesuai (misalnya, postgres-db).

• Refer ke bagian Relational Database dalam dokumentasi proyek untuk requirement detail terkait setup database, termasuk penggunaan PostgreSQL dengan extension pgvector. Melakukan provisioning database ini menggunakan IaC akan meningkatkan skor Anda.


---

Infrastruktur Anda dan project manifest Kubernetes harus di-upload ke S3 menggunakan bucket yang sudah ada pada folder infrastructure.


---

Kalau masih ada bagian lain (biasanya ada penilaian / scoring / diagram), kirim aja—aku lanjutkan tetap full tanpa diringkas 👍
