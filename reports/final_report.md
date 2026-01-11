# 🛡️ Final Report – Cyber Security Portfolio

## Disclaimer Etika & Legal
Seluruh aktivitas pengujian keamanan dan analisis yang didokumentasikan dalam laporan ini **hanya dilakukan pada lingkungan lab dan sandbox yang terizin**, seperti **Damn Vulnerable Web Application (DVWA)** serta sampel uji malware di **VirusTotal dan Any.Run Sandbox**.  
Tidak ada pengujian terhadap sistem produksi atau aset pihak ketiga tanpa izin tertulis. Seluruh bukti teknis telah **disanitasi** untuk mencegah penyalahgunaan.

---

## 1. Pendahuluan
Laporan ini disusun sebagai bagian dari portofolio Cyber Security untuk mendokumentasikan hasil **security assessment berbasis risiko** pada lingkungan lab terkontrol. Fokus utama laporan mencakup **Security Misconfiguration pada aplikasi web** serta **analisis malware berbasis sandbox** sebagai pendekatan defensive security.

Pendekatan yang digunakan menekankan pemahaman terhadap risiko, dampak, dan mitigasi yang realistis, selaras dengan praktik penetration testing dan security assessment profesional.

---

## 2. Executive Summary
Assessment menemukan beberapa temuan dengan tingkat risiko **Medium hingga Critical**. Risiko tertinggi berasal dari **penggunaan kredensial default**, yang memungkinkan pengambilalihan sistem secara penuh. Selain itu, kesalahan konfigurasi seperti **Cloudflare origin misconfiguration**, **directory listing aktif**, dan **verbose error message** memperluas permukaan serangan.

Sebagai pelengkap, analisis malware mengungkap indikasi **info-stealer malware** yang menargetkan data browser pengguna secara silent. Temuan ini menegaskan bahwa risiko keamanan tidak hanya berasal dari aplikasi web, tetapi juga dari sisi endpoint dan perilaku pengguna.

---

## 3. Metodologi
Pendekatan assessment mengacu pada praktik umum security testing:
1. Reconnaissance & Scanning  
2. Vulnerability Identification  
3. Validation (Lab Only)  
4. Post-Exploitation (Konseptual)  
5. Reporting & Mitigation  

Framework referensi yang digunakan meliputi **OWASP Top 10**, **OWASP Testing Guide**, dan **CVSS v3.1**.

---

## 4. Reconnaissance & Scanning – Security Misconfiguration  [`[See the Details]`](recon/Security_Misconfiguration.md)

### Tujuan
Tahap reconnaissance dilakukan untuk mengidentifikasi **indikasi awal security misconfiguration**, seperti konfigurasi layanan, mekanisme error handling, serta exposure direktori atau infrastruktur pendukung.

### Aktivitas Reconnaissance
- Observasi akses aplikasi DVWA melalui browser
- Identifikasi error infrastruktur (contoh: Cloudflare Error 523)
- Discovery directory listing menggunakan teknik pencarian terbuka (Google Dork)
- Observasi pesan error dan response aplikasi

Tahap ini difokuskan pada pemahaman **attack surface**, bukan eksploitasi agresif.

---

### 4.1 Reconnaissance – Malware Analysis (Pre-Execution) [`[See the Details]`](recon/Malware_Analysis.md)

Reconnaissance malware bertujuan untuk memperoleh **gambaran awal karakteristik ancaman** sebelum sampel dieksekusi di lingkungan sandbox. Pendekatan dilakukan secara **pasif dan non-intrusive**, dengan fokus pada klasifikasi ancaman dan penilaian risiko awal.

#### Aktivitas Reconnaissance
- Pengumpulan metadata dasar sampel malware
- Pengecekan reputasi awal menggunakan **VirusTotal**
- Observasi klasifikasi antivirus global
- Penilaian awal fokus ancaman (data harvesting vs destructive behavior)

Hasil reconnaissance menunjukkan indikasi malware tipe **Trojan / Info-Stealer**, sehingga analisis dilanjutkan ke tahap dynamic analysis menggunakan sandbox.

---

## 5. Technical Findings – Security Misconfiguration  [`[See the Details]`](findings/Security_Misconfiguration)

### 5.1 Penggunaan Kredensial Default pada Aplikasi Web [`[See the Details]`](findings/Security_Misconfiguration/01.Default_Credentials.md)
**Deskripsi:**  
Login DVWA berhasil menggunakan kredensial default (admin/password).

**Dampak:**  
Pengambilalihan akun administrator dan akses penuh ke aplikasi.

**CVSS:** 9.8 (Critical)
  `AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H`  

**Mitigasi:**  
Penggantian kredensial default, password policy kuat, rate limiting, account lockout, dan MFA.

#### “Bukti PoC dapat dilihat pada "[PoC Default Credentials](appendix/Screenshots_Sanitized/Security_Misconfiguration/01-poc_default-credentials.png)"

---

### 5.2 Improper Cloudflare Origin Server Configuration  [`[See the Details]`](findings/Security_Misconfiguration/02.Cloudflare_Origin_server_Configuration-Error_523.md)
**Deskripsi:**  
Cloudflare gagal menjangkau origin server akibat kesalahan konfigurasi backend.

**Dampak:**  
Gangguan availability layanan dan potensi downtime.

**CVSS:** 7.5 (High)
  `AV:N / AC:L / PR:N / UI:N / S:U / C:N / I:N / A:H`

**Mitigasi:**  
Hardening origin server, allowlist IP Cloudflare, dan verifikasi DNS.

#### “Bukti PoC dapat dilihat pada "[PoC Cloudflare-Error 523](appendix/Screenshots_Sanitized/Security_Misconfiguration/02-poc_cloudflare-error523.png)”

---

### 5.3 Directory Listing Aktif  [`[See the Details]`](findings/Security_Misconfiguration/03.Directory_Listing.md)
**Deskripsi:**  
Akses anonymous ke direktori internal dengan tampilan “Index of /”.

**Dampak:**  
Kebocoran struktur internal dan dukungan reconnaissance lanjutan.

**CVSS:** 5.3 (Medium)
  `AV:N /AC:L /PR:N /UI:N /S:U /C:L /I:N /A:N`

**Mitigasi:**  
Nonaktifkan directory listing dan hardening konfigurasi server.

#### “Bukti PoC dapat dilihat pada "[PoC Directory Listing](appendix/Screenshots_Sanitized/Security_Misconfiguration/03-poc_directory-listing.png)”

---

### 5.4 Verbose Error Message / Debug Mode Aktif [`[See the Details]`](findings/Security_Misconfiguration/04.Verbose_Error_Message.md)
**Deskripsi:**  
Aplikasi menampilkan pesan error detail ke pengguna umum.

**Dampak:**  
Kebocoran informasi internal (path file, struktur aplikasi).

**CVSS:** 5.3 (Medium)
  `AV:N / AC:L / PR:N / UI:N / S:U / C:L / I:N / A:N`

**Mitigasi:**  
Nonaktifkan display error di production dan gunakan error message generik.

#### “Bukti PoC dapat dilihat pada "[PoC Verbose Error Message](appendix/Screenshots_Sanitized/Security_Misconfiguration/04-poc_verbose-error-msg.png)”

---

## 6. Technical Findings – Malware Analysis  [`[See the Details]`](findings/Malware_Analysis/Malware_analysis.md)

### 6.1 Indikasi Info-Stealer Malware pada Data Browser
**Deskripsi:**  
Analisis sandbox menunjukkan malware berjalan secara silent dan mengakses direktori profil browser pengguna.

**Observasi Teknis (Disanitasi):**
- Akses ke direktori profil Mozilla Firefox
- Pembacaan database cookie dan metadata browsing
- Tidak ada indikasi visual pada sisi pengguna

**Dampak Risiko:**
- Pencurian cookie dan session
- Session hijacking
- Account takeover

**CVSS Estimasi:** 7.1 (High)

#### “Bukti PoC dapat dilihat pada "[PoC Malware Run](appendix/Screenshots_Sanitized/Malware_Analysis/poc_run-malware-anyrun.png)”

---

## 7. Dampak Risiko (Teknis & Bisnis)

Bagian ini menjelaskan dampak teknis dan dampak bisnis dari temuan utama yang diidentifikasi pada assessment, mencakup **Security Misconfiguration** pada aplikasi web dan **Info-Stealer Malware** pada endpoint pengguna.

---

### Dampak Teknis – Security Misconfiguration
- Penggunaan kredensial default memungkinkan **pengambilalihan akun administrator**, yang memberikan akses penuh terhadap seluruh fungsi aplikasi web.
- Directory listing aktif dan verbose error message mempercepat proses reconnaissance dengan mengungkap **struktur direktori, path file, dan informasi internal aplikasi**.
- Kesalahan konfigurasi Cloudflare origin server berpotensi menyebabkan **gangguan availability layanan** dan membuka peluang akses langsung ke origin server tanpa proteksi optimal.
- Kombinasi temuan dapat dimanfaatkan untuk **chaining attack**, yang meningkatkan risiko kompromi sistem secara menyeluruh.

---

### Dampak Bisnis – Security Misconfiguration
- **Downtime layanan** dapat mengganggu operasional bisnis dan menurunkan tingkat kepercayaan pengguna.
- Risiko **kebocoran data dan pengambilalihan sistem** dapat memicu konsekuensi hukum dan kepatuhan.
- Penurunan **reputasi organisasi** akibat insiden keamanan yang seharusnya dapat dicegah melalui konfigurasi dasar yang tepat.
- Peningkatan **biaya incident response dan remediasi** akibat lemahnya kontrol keamanan awal.

---

### Dampak Teknis – Malware Analysis (Info-Stealer)
- Pencurian cookie dan data sesi autentikasi memungkinkan terjadinya **session hijacking tanpa memerlukan kredensial pengguna**.
- Malware berjalan secara **silent di background**, sehingga sulit terdeteksi tanpa kontrol keamanan endpoint berbasis perilaku.
- Data browser yang dikumpulkan dapat digunakan untuk **akses tidak sah lanjutan** ke aplikasi internal maupun eksternal.
- Endpoint yang terinfeksi berpotensi menjadi **initial access vector** untuk serangan yang lebih kompleks di lingkungan organisasi.

---

### Dampak Bisnis – Malware Analysis (Info-Stealer)
- **Kompromi akun pengguna** meningkatkan risiko kebocoran data dan penyalahgunaan akses sistem.
- Potensi **fraud, impersonasi, dan penyalahgunaan akun** berdampak langsung pada kepercayaan pengguna dan mitra bisnis.
- Kerugian operasional akibat kebutuhan **forensik digital, pemulihan sistem, dan peningkatan kontrol keamanan endpoint** pasca insiden.
- Penurunan **kepercayaan pengguna** terhadap keamanan sistem dan layanan organisasi.

---

## 8. Rekomendasi & Prioritas Mitigasi

### Security Misconfiguration
- **Critical:** Penggantian kredensial default dan kontrol autentikasi
- **High:** Hardening Cloudflare dan origin server
- **Medium:** Penanganan directory listing dan verbose error message

### Malware Analysis
- **Critical:** Implementasi EDR / antivirus berbasis perilaku
- **High:** Pembatasan eksekusi file dari direktori user dan hardening browser
- **Medium:** Least privilege, patching berkala, dan security awareness

---

## 9. Kesimpulan
### 9.1 Kesimpulan Temuan – Security Misconfiguration

Berdasarkan hasil assessment, temuan kategori **Security Misconfiguration** menunjukkan bahwa kelemahan konfigurasi dasar dapat menimbulkan risiko keamanan yang signifikan, bahkan tanpa memerlukan teknik eksploitasi yang kompleks. Penggunaan kredensial default menjadi faktor risiko paling kritis karena memungkinkan pengambilalihan sistem secara penuh dalam waktu singkat.

Selain itu, keberadaan directory listing aktif, verbose error message, serta kesalahan konfigurasi infrastruktur memperluas permukaan serangan dan mempermudah proses reconnaissance oleh pihak yang tidak berwenang. Temuan ini menegaskan bahwa pengamanan konfigurasi dasar dan penerapan prinsip hardening merupakan fondasi utama dalam menjaga keamanan aplikasi web.

---

### 9.2 Kesimpulan Temuan – Malware Analysis (Info-Stealer)

Hasil analisis malware berbasis sandbox menunjukkan bahwa ancaman terhadap keamanan sistem tidak selalu bersifat destruktif atau terlihat secara langsung oleh pengguna. Sampel uji malware yang dianalisis memiliki karakteristik **info-stealer**, dengan fokus pada pengumpulan data browser dan sesi autentikasi secara silent.

Meskipun tidak ditemukan aktivitas perusakan sistem, potensi penyalahgunaan data yang dikumpulkan seperti session hijacking dan pengambilalihan akun menjadikan ancaman ini relevan dan berbahaya, khususnya pada endpoint dengan kontrol keamanan minimal. Temuan ini menekankan pentingnya pendekatan keamanan endpoint berbasis perilaku dan peningkatan kesadaran pengguna terhadap risiko eksekusi file yang tidak terverifikasi.

---

## Kesimpulan Akhir

Secara keseluruhan, assessment ini menunjukkan bahwa risiko keamanan sering kali muncul dari kombinasi kelemahan aplikasi, kesalahan konfigurasi infrastruktur, serta ancaman pada sisi endpoint. Temuan Security Misconfiguration dan Malware Analysis saling melengkapi dalam menggambarkan bagaimana celah sederhana dan perilaku pengguna dapat dimanfaatkan untuk menurunkan postur keamanan sistem secara signifikan.

Pendekatan berbasis risiko, disertai mitigasi yang realistis dan berorientasi pada praktik industri, menjadi kunci dalam meningkatkan ketahanan sistem terhadap ancaman keamanan. Laporan ini diharapkan dapat merepresentasikan pemahaman menyeluruh terhadap proses security assessment yang tidak hanya berfokus pada aspek teknis, tetapi juga mempertimbangkan dampak operasional dan bisnis secara holistik.

---

## 10. Catatan Etika
Seluruh analisis dilakukan secara legal, etis, dan terbatas pada lingkungan terizin. Dokumentasi ini disusun untuk tujuan pembelajaran dan peningkatan kesadaran keamanan.
