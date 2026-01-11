# 🛡️ Cyber Security Portfolio

## ⚠️ Disclaimer Etika & Legal
Seluruh aktivitas pengujian keamanan dan analisis yang didokumentasikan dalam portofolio ini **hanya dilakukan pada lingkungan lab dan sandbox yang terizin**, seperti **Damn Vulnerable Web Application (DVWA)** serta sampel uji malware di **VirusTotal** dan **Any.Run**.  

Tidak ada pengujian terhadap sistem produksi atau aset pihak ketiga tanpa izin tertulis. Seluruh bukti teknis telah **disanitasi** untuk mencegah penyalahgunaan.

---

## 👋 Pengantar Portofolio
Halo, saya seorang **student Cyber Security** yang berfokus pada **analisis risiko keamanan**, **security misconfiguration**, serta **analisis malware berbasis sandbox**.  

Portofolio ini disusun untuk menunjukkan kemampuan saya dalam melakukan analisis keamanan secara **terstruktur, etis, dan profesional**. Fokus utama portofolio ini bukan hanya pada eksploitasi teknis, tetapi pada **pemahaman risiko, dampak, dan mitigasi yang realistis**, sebagaimana praktik dalam laporan penetration testing dan security assessment profesional.

---

## 📌 Executive Summary
Portofolio ini mendokumentasikan hasil **security assessment berbasis risiko** yang dilakukan pada lingkungan lab terizin, dengan fokus utama pada **Security Misconfiguration** dan **analisis malware berbasis sandbox**.  

Ditemukan beberapa temuan dengan tingkat risiko **Medium hingga Critical** yang berpotensi berdampak signifikan terhadap keamanan aplikasi dan endpoint pengguna. Risiko paling kritis berasal dari **penggunaan kredensial default**, yang memungkinkan pengambilalihan sistem secara penuh.

Selain itu, kesalahan konfigurasi infrastruktur dan aplikasi—seperti **Cloudflare origin misconfiguration**, **directory listing aktif**, dan **verbose error message**—memperluas permukaan serangan dan mempermudah eksploitasi lanjutan.

Sebagai pelengkap, analisis malware menunjukkan adanya indikasi **info-stealer** yang menargetkan data browser pengguna secara **silent**. Temuan ini menegaskan bahwa kontrol keamanan harus mencakup **aplikasi, endpoint, dan perilaku pengguna**. Seluruh rekomendasi difokuskan pada mitigasi yang **praktis, realistis, dan selaras dengan praktik industri**.

---

## 🧭 Metodologi Umum
Pendekatan yang digunakan mengacu pada praktik umum security assessment dan penetration testing:

1. **Reconnaissance & Scanning** – Identifikasi exposure awal dan potensi misconfiguration  
2. **Vulnerability Identification** – Analisis kerentanan berdasarkan observasi teknis  
3. **Validation (Lab Only)** – Pembuktian risiko secara terbatas dan terkontrol  
4. **Post-Exploitation (Konseptual)** – Analisis dampak lanjutan tanpa eksploitasi aktif  
5. **Reporting & Mitigation** – Penyusunan laporan, penilaian risiko, dan rekomendasi  

Framework referensi: **OWASP Top 10**, **OWASP Testing Guide**, dan **CVSS v3.1**

---

## 🔍 1. Reconnaissance & Scanning

### Tujuan
Tahap reconnaissance dilakukan untuk mengidentifikasi **indikasi awal security misconfiguration**, seperti konfigurasi layanan, mekanisme error handling, serta exposure direktori atau infrastruktur pendukung.

### Aktivitas Utama
- Observasi akses aplikasi DVWA melalui browser  
- Identifikasi error infrastruktur (misalnya **Cloudflare Error 523**)  
- Discovery directory listing menggunakan teknik pencarian terbuka (**Google Dork**)  

Tahap ini difokuskan pada **pemahaman permukaan serangan**, bukan eksploitasi agresif.

---

## 🧪 2. Technical Findings – Security Misconfiguration (Lab Terizin)

Bagian ini disesuaikan dengan **Laporan Security Misconfiguration**. Seluruh temuan dikategorikan sebagai kesalahan konfigurasi dengan tingkat risiko yang bervariasi.

### Temuan Utama

#### 1. Penggunaan Kredensial Default pada Aplikasi Web
- **Deskripsi**: Login DVWA berhasil menggunakan kredensial default (admin/password)  
- **Dampak**: Pengambilalihan akun administrator dan akses penuh ke aplikasi  
- **CVSS**: **9.8 (Critical)**  
  `AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H`  
- **Mitigasi**: Penggantian kredensial default, password policy kuat, rate limiting, account lockout, dan MFA  

#### 2. Improper Cloudflare Origin Server Configuration (Error 523)
- **Deskripsi**: Cloudflare gagal menjangkau origin server  
- **Dampak**: Gangguan availability dan potensi downtime  
- **CVSS**: **7.5 (High)**  
- **Mitigasi**: Hardening origin server, firewall allowlist IP Cloudflare, dan verifikasi DNS  

#### 3. Directory Listing Aktif
- **Deskripsi**: Akses anonymous ke direktori internal  
- **Dampak**: Kebocoran struktur internal dan bantuan reconnaissance  
- **CVSS**: **5.3 (Medium)**  
- **Mitigasi**: Nonaktifkan directory listing, RBAC, audit repository publik  

#### 4. Verbose Error Message / Debug Mode Aktif
- **Deskripsi**: Pesan error detail ditampilkan ke pengguna umum  
- **Dampak**: Kebocoran informasi internal  
- **CVSS**: **5.3 (Medium)**  
- **Mitigasi**: Gunakan error message generik di production  

---

## 🧠 3. Post-Exploitation & Dampak Risiko (Konseptual)

Bagian ini bersifat **analisis konseptual** dan tidak memuat eksploitasi lanjutan.

- Kredensial default berpotensi menyebabkan **kompromi total aplikasi**
- Directory listing dan verbose error memungkinkan **chaining attack**
- Misconfiguration infrastruktur berdampak pada **availability, reputasi, dan operasional bisnis**

---

## 📊 4. Laporan Akhir & Rekomendasi (Security Misconfiguration)

### Area Rekomendasi Prioritas
- **Critical**: Penggantian kredensial default dan kontrol autentikasi  
- **High**: Hardening konfigurasi Cloudflare dan origin server  
- **Medium**: Penanganan directory listing dan verbose error message  

---

## 🔬 5. Malware Analysis (Sandbox-Based)

Analisis malware dilakukan untuk memahami **perilaku ancaman di sisi endpoint**, bukan untuk pengembangan atau penyebaran malware.

Tools yang digunakan:
- **VirusTotal**
- **Any.Run Sandbox**

---

### 🧪 Technical Finding 02  
**Indikasi Info-Stealer Malware pada Data Browser**

Sampel uji malware menunjukkan perilaku **akses tidak sah terhadap data browser pengguna**. Malware berjalan secara **silent** tanpa notifikasi visual, dan terdeteksi mengakses direktori profil **Mozilla Firefox** yang menyimpan cookie dan data sesi autentikasi.

Perilaku ini konsisten dengan **Trojan / info-stealer ringan** yang berfokus pada pencurian data sensitif.

---

### 🧾 Observasi Teknis (Disanitasi)
- Malware dieksekusi di **Any.Run Sandbox**
- Proses berjalan tanpa interaksi user
- Akses ke direktori: "C:\Users\admin\AppData\Roaming\Mozilla\Firefox\Profiles\" 

- Tidak ada indikasi visual di sisi pengguna

---

### 💥 Dampak Risiko
- Pencurian cookie dan data sesi
- Session hijacking dan account takeover
- Kebocoran aktivitas browsing
- Risiko akses tidak sah lanjutan

---

### 📊 Severity Assessment (CVSS v3.1)
**Estimated Score: 7.1 (High)**  
Dampak utama pada **confidentiality**.

---

### 🛠️ Laporan Akhir & Rekomendasi – Malware Analysis

#### Area Rekomendasi Prioritas
- **Critical**: Implementasi EDR / antivirus berbasis perilaku  
- **High**: Pembatasan eksekusi file & hardening browser  
- **Medium**: Least privilege, patching berkala, dan security awareness  

---

## ⚖️ Etika & Profesionalisme
Seluruh aktivitas dilakukan pada **lingkungan terisolasi dan terizin**. Dokumentasi ini disusun untuk tujuan pembelajaran dan peningkatan keamanan.

---

## 🚀 Penutup
Portofolio ini mencerminkan pendekatan saya dalam Cyber Security: **memahami risiko, dampak, dan solusi**, bukan sekadar mencari celah teknis. Portofolio ini merepresentasikan kesiapan saya untuk terlibat dalam aktivitas security assessment secara profesional.
