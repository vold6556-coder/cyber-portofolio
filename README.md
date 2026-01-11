# 🛡️ Cyber Security Portfolio

## ⚠️ Disclaimer Etika & Legal
Seluruh aktivitas pengujian keamanan dan analisis yang didokumentasikan dalam portofolio ini **hanya dilakukan pada lingkungan lab dan sandbox yang terizin**, seperti **Damn Vulnerable Web Application (DVWA)** serta sampel uji malware di **VirusTotal** dan **Any.Run**.  

Tidak ada pengujian terhadap sistem produksi atau aset pihak ketiga tanpa izin tertulis. Seluruh bukti teknis telah **disanitasi** untuk mencegah penyalahgunaan.

---

## 👋 Pengantar Portofolio
Halo, saya **Bela Agustina** seorang **student Cyber Security** yang berfokus pada **Red Team**.  

Portofolio ini disusun untuk menunjukkan kemampuan saya dalam melakukan analisis keamanan secara **terstruktur, etis, dan profesional**. Fokus utama portofolio ini bukan hanya pada eksploitasi teknis, tetapi pada **pemahaman risiko, dampak, dan mitigasi yang realistis**, sebagaimana praktik dalam laporan penetration testing dan security assessment profesional.

---

## 📌 Executive Summary
Portofolio ini mendokumentasikan hasil **security assessment berbasis risiko** yang dilakukan pada lingkungan lab terizin, dengan fokus utama pada **Security Misconfiguration** dan **analisis malware berbasis sandbox**.  

Ditemukan beberapa temuan dengan tingkat risiko **Medium hingga Critical** yang berpotensi berdampak signifikan terhadap keamanan aplikasi dan endpoint pengguna. Risiko paling kritis berasal dari **penggunaan kredensial default**, yang memungkinkan pengambilalihan sistem secara penuh.

Selain itu, kesalahan konfigurasi infrastruktur dan aplikasi seperti **Cloudflare origin misconfiguration**, **directory listing aktif**, dan **verbose error message** memperluas permukaan serangan dan mempermudah eksploitasi lanjutan.

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

## 🔍 1. Reconnaissance & Scanning [`[See the Details]`](recon/Security_Misconfiguration.md)
### Tujuan
Tahap reconnaissance dilakukan untuk mengidentifikasi **indikasi awal security misconfiguration**, seperti konfigurasi layanan, mekanisme error handling, serta exposure direktori atau infrastruktur pendukung.

### Aktivitas Utama
- Observasi akses aplikasi DVWA melalui browser  
- Identifikasi error infrastruktur (misalnya **Cloudflare Error 523**)  
- Discovery directory listing menggunakan teknik pencarian terbuka (**Google Dork**)  

Tahap ini difokuskan pada **pemahaman permukaan serangan**, bukan eksploitasi agresif.

---

## 🧪 2. Technical Findings – Security Misconfiguration (Lab Terizin) [`[See the Details]`](findings/Security_Misconfiguration)

Bagian ini disesuaikan dengan **Laporan Security Misconfiguration**. Seluruh temuan dikategorikan sebagai kesalahan konfigurasi dengan tingkat risiko yang bervariasi.

### Temuan Utama

#### 1. Penggunaan Kredensial Default pada Aplikasi Web
- **Deskripsi**: Login DVWA berhasil menggunakan kredensial default (admin/password)  
- **Dampak**: Pengambilalihan akun administrator dan akses penuh ke aplikasi  
- **CVSS**: **9.8 (Critical)**  
  `AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H`  
- **Mitigasi**: Penggantian kredensial default, password policy kuat, rate limiting, account lockout, dan MFA
#### “Bukti PoC dapat dilihat pada "[PoC Default Credentials](appendix/Screenshots_Sanitized/Security_Misconfiguration/01-poc_default-credentials.png)"

#### 2. Improper Cloudflare Origin Server Configuration (Error 523)
- **Deskripsi**: Cloudflare gagal menjangkau origin server  
- **Dampak**: Gangguan availability dan potensi downtime  
- **CVSS**: **7.5 (High)**
  `AV:N / AC:L / PR:N / UI:N / S:U / C:N / I:N / A:H`
- **Mitigasi**: Hardening origin server, firewall allowlist IP Cloudflare, dan verifikasi DNS  
#### “Bukti PoC dapat dilihat pada "[PoC Cloudflare-Error 523](appendix/Screenshots_Sanitized/Security_Misconfiguration/02-poc_cloudflare-error523.png)”

#### 3. Directory Listing Aktif
- **Deskripsi**: Akses anonymous ke direktori internal  
- **Dampak**: Kebocoran struktur internal dan bantuan reconnaissance  
- **CVSS**: **5.3 (Medium)**
  `AV:N /AC:L /PR:N /UI:N /S:U /C:L /I:N /A:N`
- **Mitigasi**: Nonaktifkan directory listing, RBAC, audit repository publik  
#### “Bukti PoC dapat dilihat pada "[PoC Directory Listing](appendix/Screenshots_Sanitized/Security_Misconfiguration/03-poc_directory-listing.png)”


#### 4. Verbose Error Message / Debug Mode Aktif
- **Deskripsi**: Pesan error detail ditampilkan ke pengguna umum  
- **Dampak**: Kebocoran informasi internal  
- **CVSS**: **5.3 (Medium)**
  `AV:N / AC:L / PR:N / UI:N / S:U / C:L / I:N / A:N`
- **Mitigasi**: Gunakan error message generik di production  
#### “Bukti PoC dapat dilihat pada "[PoC Verbose Error Message](appendix/Screenshots_Sanitized/Security_Misconfiguration/04-poc_verbose-error-msg.png)”

---

## 🧠 3. Dampak Risiko (Konseptual)

### Dampak Teknis 
- Penggunaan kredensial default memungkinkan **pengambilalihan akun administrator**, yang memberikan akses penuh terhadap seluruh fungsi aplikasi web.
- Directory listing aktif dan verbose error message mempercepat proses reconnaissance dengan mengungkap **struktur direktori, path file, dan informasi internal aplikasi**.
- Kesalahan konfigurasi Cloudflare origin server berpotensi menyebabkan **gangguan availability layanan** dan membuka peluang akses langsung ke origin server tanpa proteksi optimal.
- Kombinasi temuan dapat dimanfaatkan untuk **chaining attack**, yang meningkatkan risiko kompromi sistem secara menyeluruh.

---

### Dampak Bisnis 
- **Downtime layanan** dapat mengganggu operasional bisnis dan menurunkan tingkat kepercayaan pengguna.
- Risiko **kebocoran data dan pengambilalihan sistem** dapat memicu konsekuensi hukum dan kepatuhan.
- Penurunan **reputasi organisasi** akibat insiden keamanan yang seharusnya dapat dicegah melalui konfigurasi dasar yang tepat.
- Peningkatan **biaya incident response dan remediasi** akibat lemahnya kontrol keamanan awal.
---

## 📊 4. Laporan Akhir & Rekomendasi (Security Misconfiguration)
## Kesimpulan Temuan – Security Misconfiguration

Berdasarkan hasil assessment, temuan kategori **Security Misconfiguration** menunjukkan bahwa kelemahan konfigurasi dasar dapat menimbulkan risiko keamanan yang signifikan, bahkan tanpa memerlukan teknik eksploitasi yang kompleks. Penggunaan kredensial default menjadi faktor risiko paling kritis karena memungkinkan pengambilalihan sistem secara penuh dalam waktu singkat.

Selain itu, keberadaan directory listing aktif, verbose error message, serta kesalahan konfigurasi infrastruktur memperluas permukaan serangan dan mempermudah proses reconnaissance oleh pihak yang tidak berwenang. Temuan ini menegaskan bahwa pengamanan konfigurasi dasar dan penerapan prinsip hardening merupakan fondasi utama dalam menjaga keamanan aplikasi web.

### Area Rekomendasi Prioritas
- **Critical**: Penggantian kredensial default dan kontrol autentikasi  
- **High**: Hardening konfigurasi Cloudflare dan origin server  
- **Medium**: Penanganan directory listing dan verbose error message  

---

## 🔬 Reconnaissance Malware Analysis (Sandbox-Based) [`[See the Details]`](recon/Malware_Analysis.md)

Selain pengujian keamanan aplikasi web, portofolio ini juga mencakup analisis malware berbasis sandbox sebagai bagian dari pendekatan defensive security. Fokus analisis ini bukan pada pengembangan atau penyebaran malware, melainkan pada pemahaman perilaku ancaman di sisi endpoint dan potensi dampaknya terhadap pengguna maupun organisasi.
Analisis dilakukan menggunakan VirusTotal dan Any.Run Sandbox terhadap sampel uji yang dieksekusi di lingkungan terisolasi.

Tools yang digunakan:
- **VirusTotal**
- **Any.Run Sandbox**

---

### 🧪 Technical Finding 02   [`[See the Details]`](findings/Malware_Analysis/Malware_analysis.md)
**Indikasi Info-Stealer Malware pada Data Browser**

Sampel uji malware menunjukkan perilaku **akses tidak sah terhadap data browser pengguna**. Malware berjalan secara **silent** tanpa notifikasi visual, dan terdeteksi mengakses direktori profil **Mozilla Firefox** yang menyimpan cookie dan data sesi autentikasi.

Perilaku ini konsisten dengan **Trojan / info-stealer ringan** yang berfokus pada pencurian data sensitif.

---

### 🧾 Observasi Teknis (Disanitasi)
- Malware dieksekusi di **Any.Run Sandbox**
- Proses berjalan tanpa interaksi user
- Akses ke direktori: "C:\Users\admin\AppData\Roaming\Mozilla\Firefox\Profiles\" 
- Tidak ada indikasi visual di sisi pengguna

#### “Bukti PoC dapat dilihat pada "[PoC Malware Run](appendix/Screenshots_Sanitized/Malware_Analysis/poc_run-malware-anyrun.png)”

---

### 💥 Dampak Risiko
### Dampak Teknis
- Pencurian cookie dan data sesi autentikasi memungkinkan terjadinya **session hijacking tanpa memerlukan kredensial pengguna**.
- Malware berjalan secara **silent di background**, sehingga sulit terdeteksi tanpa kontrol keamanan endpoint berbasis perilaku.
- Data browser yang dikumpulkan dapat digunakan untuk **akses tidak sah lanjutan** ke aplikasi internal maupun eksternal.
- Endpoint yang terinfeksi berpotensi menjadi **initial access vector** untuk serangan yang lebih kompleks di lingkungan organisasi.

---

### Dampak Bisnis
- **Kompromi akun pengguna** meningkatkan risiko kebocoran data dan penyalahgunaan akses sistem.
- Potensi **fraud, impersonasi, dan penyalahgunaan akun** berdampak langsung pada kepercayaan pengguna dan mitra bisnis.
- Kerugian operasional akibat kebutuhan **forensik digital, pemulihan sistem, dan peningkatan kontrol keamanan endpoint** pasca insiden.
- Penurunan **kepercayaan pengguna** terhadap keamanan sistem dan layanan organisasi.

---

### 📊 Severity Assessment (CVSS v3.1)
**Estimated Score: 7.1 (High)**  
Dampak utama pada **confidentiality**.

---

### 🛠️ Laporan Akhir & Rekomendasi – Malware Analysis

## Kesimpulan Temuan 

Hasil analisis malware berbasis sandbox menunjukkan bahwa ancaman terhadap keamanan sistem tidak selalu bersifat destruktif atau terlihat secara langsung oleh pengguna. Sampel uji malware yang dianalisis memiliki karakteristik **info stealer**, dengan fokus pada pengumpulan data browser dan sesi autentikasi secara silent.

Meskipun tidak ditemukan aktivitas perusakan sistem, potensi penyalahgunaan data yang dikumpulkan seperti session hijacking dan pengambilalihan akun—menjadikan ancaman ini relevan dan berbahaya, khususnya pada endpoint dengan kontrol keamanan minimal. Temuan ini menekankan pentingnya pendekatan keamanan endpoint berbasis perilaku dan peningkatan kesadaran pengguna terhadap risiko eksekusi file yang tidak terverifikasi.

#### Area Rekomendasi Prioritas
- **Critical**: Implementasi EDR / antivirus berbasis perilaku  
- **High**: Pembatasan eksekusi file & hardening browser  
- **Medium**: Least privilege, patching berkala, dan security awareness  

---

## ⚖️ Etika & Profesionalisme
Seluruh aktivitas dilakukan pada **lingkungan terisolasi dan terizin**. Dokumentasi ini disusun untuk tujuan pembelajaran dan peningkatan keamanan.

---

## Kesimpulan Akhir 

Secara keseluruhan, assessment ini menunjukkan bahwa risiko keamanan sering kali muncul dari kombinasi kelemahan aplikasi, kesalahan konfigurasi infrastruktur, serta ancaman pada sisi endpoint. Temuan Security Misconfiguration dan Malware Analysis saling melengkapi dalam menggambarkan bagaimana celah sederhana dan perilaku pengguna dapat dimanfaatkan untuk menurunkan postur keamanan sistem secara signifikan.

Pendekatan berbasis risiko, disertai mitigasi yang realistis dan berorientasi pada praktik industri, menjadi kunci dalam meningkatkan ketahanan sistem terhadap ancaman keamanan. Laporan ini diharapkan dapat merepresentasikan pemahaman menyeluruh terhadap proses security assessment yang tidak hanya berfokus pada aspek teknis, tetapi juga mempertimbangkan dampak operasional dan bisnis secara holistik.

## 🚀 Penutup
Portofolio ini mencerminkan pendekatan saya dalam Cyber Security: **memahami risiko, dampak, dan solusi**, bukan sekadar mencari celah teknis. Portofolio ini merepresentasikan kesiapan saya untuk terlibat dalam aktivitas security assessment secara profesional.



## [See the Final Report](reports/final_report.md)
---
