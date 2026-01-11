# cyber-portofolio
🛡️ Cyber Security Portfolio
Disclaimer Etika & Legal
Seluruh aktivitas pengujian keamanan dan analisis yang didokumentasikan dalam portofolio ini hanya dilakukan pada lingkungan lab dan sandbox yang terizin, seperti Damn Vulnerable Web Application (DVWA) serta sampel uji malware di VirusTotal dan Any.Run. Tidak ada pengujian terhadap sistem produksi atau aset pihak ketiga tanpa izin tertulis. Seluruh bukti teknis telah disanitasi untuk mencegah penyalahgunaan.

👋 Pengantar Portofolio
Halo, saya seorang student Cyber Security yang berfokus pada analisis risiko keamanan, security misconfiguration, serta analisis malware berbasis sandbox. Portofolio ini disusun untuk menunjukkan kemampuan saya dalam melakukan analisis keamanan secara terstruktur, etis, dan profesional.
Fokus utama portofolio ini bukan hanya pada eksploitasi teknis, tetapi pada pemahaman menyeluruh terhadap risiko, dampak, dan mitigasi yang realistis, sebagaimana praktik yang diterapkan dalam laporan penetration testing dan security assessment di dunia profesional.

📌 Executive Summary
Portofolio ini mendokumentasikan hasil security assessment berbasis risiko yang dilakukan pada lingkungan lab terizin, dengan fokus utama pada Security Misconfiguration dan analisis malware berbasis sandbox. Melalui pendekatan terstruktur, ditemukan beberapa temuan dengan tingkat risiko Medium hingga Critical yang berpotensi berdampak signifikan terhadap keamanan aplikasi dan endpoint pengguna.
Risiko paling kritis berasal dari penggunaan kredensial default, yang memungkinkan pengambilalihan sistem secara penuh. Selain itu, kesalahan konfigurasi infrastruktur dan aplikasi (seperti Cloudflare origin misconfiguration, directory listing aktif, dan verbose error message) memperluas permukaan serangan dan mempermudah eksploitasi lanjutan.
Sebagai pelengkap, analisis malware menunjukkan adanya indikasi infostealer yang menargetkan data browser pengguna secara silent. Temuan ini menegaskan pentingnya kontrol keamanan tidak hanya pada sisi aplikasi, tetapi juga pada endpoint dan perilaku pengguna. Seluruh rekomendasi difokuskan pada mitigasi yang praktis, realistis, dan selaras dengan praktik industri.

🧭 Metodologi Umum
Pendekatan yang digunakan mengacu pada praktik umum security assessment dan penetration testing: 
1.Reconnaissance & Scanning – Identifikasi exposure awal dan potensi misconfiguration 
2.Vulnerability Identification – Analisis kerentanan berdasarkan observasi teknis 
3.Validation (Lab Only) – Pembuktian risiko secara terbatas dan terkontrol 
4.Post‑Exploitation (Konseptual) – Analisis dampak lanjutan tanpa eksploitasi aktif 
5. Reporting & Mitigation – Penyusunan laporan, penilaian risiko, dan rekomendasi
Framework referensi: OWASP Top 10, OWASP Testing Guide, dan CVSS v3.1.

🔍 1. Reconnaissance & Scanning
Tujuan
Tahap reconnaissance dilakukan untuk mengidentifikasi indikasi awal security misconfiguration, seperti konfigurasi layanan, mekanisme error handling, serta exposure direktori atau infrastruktur pendukung.
Aktivitas Utama
Observasi akses aplikasi DVWA melalui browser
Identifikasi error infrastruktur (misalnya Cloudflare Error 523)
Discovery directory listing menggunakan teknik pencarian terbuka (Google Dork)
Tahap ini difokuskan pada pemahaman permukaan serangan, bukan eksploitasi agresif.

🧪 2. Technical Findings – Security Misconfiguration (Lab Terizin)
Bagian ini disesuaikan langsung dengan Laporan Security Misconfiguration yang telah disusun. Seluruh temuan dikategorikan sebagai kesalahan konfigurasi, dengan tingkat risiko yang bervariasi.
Temuan Utama
1. Penggunaan Kredensial Default pada Aplikasi Web
Deskripsi: Login DVWA berhasil dilakukan menggunakan kredensial default (admin/password).
Dampak: Pengambilalihan akun administrator dan akses penuh ke seluruh fungsi aplikasi.
CVSS: 9.8 (Critical)
Vector: AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H
Mitigasi: Penggantian kredensial default, penerapan password policy yang kuat, rate limiting, account lockout, dan MFA.
2. Improper Cloudflare Origin Server Configuration (Error 523)
Deskripsi: Cloudflare tidak dapat menjangkau origin server akibat kesalahan konfigurasi backend.
Dampak: Gangguan availability layanan dan potensi downtime berkepanjangan.
CVSS: 7.5 (High)
Vector: AV:N / AC:L / PR:N / UI:N / S:U / C:N / I:N / A:H
Mitigasi: Hardening konfigurasi origin server, firewall allowlist IP Cloudflare, dan verifikasi DNS.
3. Directory Listing Aktif
Deskripsi: Akses anonymous ke direktori internal dengan tampilan “Index of /”.
Dampak: Kebocoran struktur internal dan bantuan reconnaissance untuk serangan lanjutan.
CVSS: 5.3 (Medium)
Mitigasi: Nonaktifkan directory listing, terapkan RBAC, audit repository publik, dan hardening artifactory.
4. Verbose Error Message / Debug Mode Aktif
Deskripsi: Aplikasi menampilkan pesan error detail kepada pengguna umum.
Dampak: Kebocoran informasi internal seperti path file dan struktur aplikasi.
CVSS: 5.3 (Medium)
Mitigasi: Nonaktifkan display error di production dan gunakan pesan error generik.

🧠 3. Post‑Exploitation & Dampak Risiko (Konseptual)
Bagian ini bersifat analisis konseptual dan tidak memuat eksploitasi lanjutan.
Kredensial default pada sistem produksi dapat menyebabkan kompromi total aplikasi.
Informasi dari directory listing dan verbose error message dapat digunakan untuk chaining attack.
Kesalahan konfigurasi infrastruktur berdampak langsung pada availability, reputasi, dan operasional bisnis.
Tujuan bagian ini adalah menunjukkan pemahaman bahwa risiko keamanan tidak berhenti pada satu temuan teknis.
📊 5. Laporan Akhir & Rekomendasi
Ditemukan beberapa kerentanan kategori Security Misconfiguration dengan tingkat risiko Medium hingga Critical. Risiko tertinggi berasal dari penggunaan kredensial default, diikuti oleh kesalahan konfigurasi infrastruktur.
Area Rekomendasi Prioritas
Critical: Penggantian kredensial default dan kontrol autentikasi
High: Hardening konfigurasi Cloudflare dan origin server
Medium: Penanganan directory listing dan verbose error message
Rekomendasi difokuskan pada langkah yang realistis dan dapat diterapkan di lingkungan organisasi.

⚖️ Etika & Profesionalisme
Seluruh aktivitas dalam portofolio ini dilakukan dengan menjunjung tinggi etika, legalitas, dan tanggung jawab profesional. Analisis difokuskan pada pembelajaran, peningkatan keamanan, dan pendekatan defensif.

Malware Analysis (Sandbox-Based)
Selain pengujian keamanan aplikasi web, portofolio ini juga mencakup analisis malware berbasis sandbox sebagai bagian dari pendekatan defensive security. Fokus analisis ini bukan pada pengembangan atau penyebaran malware, melainkan pada pemahaman perilaku ancaman di sisi endpoint dan potensi dampaknya terhadap pengguna maupun organisasi.
Analisis dilakukan menggunakan VirusTotal dan Any.Run Sandbox terhadap sampel uji yang dieksekusi di lingkungan terisolasi.

🧪 Technical Finding 02
Indikasi Info-Stealer Malware pada Data Browser
Berdasarkan hasil analisis statis dan dinamis, sampel uji malware menunjukkan perilaku yang mengarah pada akses tidak sah terhadap data browser pengguna. Saat dijalankan di lingkungan sandbox, malware berjalan secara silent di background tanpa menampilkan antarmuka atau notifikasi apa pun kepada pengguna.
Observasi aktivitas proses menunjukkan adanya akses langsung ke direktori profil Mozilla Firefox, termasuk file-file yang umumnya menyimpan cookie, histori browsing, dan data sesi autentikasi. Dari sudut pandang pengguna awam, aktivitas ini hampir tidak terdeteksi. Namun dari perspektif keamanan, perilaku tersebut berpotensi dimanfaatkan untuk session hijacking atau pengambilalihan akun aplikasi web tanpa perlu mengetahui kredensial pengguna.
Pola ini konsisten dengan karakteristik Trojan atau info-stealer ringan, yang lebih berfokus pada pengumpulan data sensitif secara tersembunyi dibandingkan perusakan sistem secara langsung.

🧾 Observasi Teknis (Disanitasi)
Bukti teknis disajikan dalam bentuk observasi sandbox, tanpa menyertakan payload aktif atau instruksi eksploitasi.
Malware dieksekusi di Any.Run Sandbox
Proses berjalan tanpa interaksi user
Terdeteksi akses ke direktori “ C:\Users\admin\AppData\Roaming\Mozilla\Firefox\Profiles\”
File yang diakses mencakup database cookie dan metadata browsing
Tidak ada indikasi visual atau error yang muncul di sisi pengguna
Observasi ini menunjukkan bahwa ancaman utama bersifat silent data harvesting, yang sulit dideteksi tanpa kontrol keamanan endpoint.

💥 Dampak Risiko
Jika perilaku serupa terjadi pada endpoint pengguna di lingkungan produksi, potensi dampak yang dapat timbul antara lain:
Pencurian cookie dan data sesi autentikasi
Pengambilalihan akun aplikasi web tanpa username dan password
Kebocoran aktivitas browsing dan informasi personal
Risiko akses tidak sah lanjutan ke sistem internal melalui reuse session
Dampak utama berada pada aspek confidentiality, sementara integritas dan ketersediaan sistem tidak terdampak secara langsung.

📊 Penilaian Severity (CVSS v3.1 – Estimasi)
Parameter	Nilai
Attack Vector	Local
Attack Complexity	Low
Privileges Required	None
User Interaction	Required
Scope	Unchanged
Confidentiality Impact	High
Integrity Impact	Low
Availability Impact	None
Estimated CVSS Score: 7.1 (High)
Penilaian ini mencerminkan bahwa meskipun eksekusi malware membutuhkan interaksi pengguna, dampak kebocoran data yang dihasilkan tergolong signifikan.

🛠️ Laporan Akhir & Rekomendasi
Berdasarkan hasil analisis malware berbasis sandbox, dapat disimpulkan bahwa sampel uji yang dianalisis menunjukkan karakteristik malware tipe Trojan / info-stealer dengan fokus utama pada pengumpulan data browser pengguna. Malware berjalan secara silent tanpa indikasi visual, sehingga berpotensi lolos dari perhatian pengguna awam.
Perilaku akses ke direktori profil browser dan file penyimpanan cookie serta data sesi mengindikasikan risiko serius terhadap kerahasiaan data dan keamanan akun pengguna. Meskipun tidak ditemukan aktivitas destruktif atau gangguan langsung terhadap sistem, potensi penyalahgunaan data yang dikumpulkan (seperti session hijacking atau account takeover) menjadikan ancaman ini relevan dan berbahaya, terutama pada lingkungan endpoint dengan kontrol keamanan minimal.
Temuan ini menegaskan bahwa ancaman keamanan tidak hanya berasal dari celah aplikasi web, tetapi juga dari sisi endpoint dan perilaku pengguna dalam mengeksekusi file yang tidak terverifikasi.
Area Rekomendasi Prioritas
Beberapa langkah mitigasi yang direkomendasikan untuk mengurangi risiko serupa:
·  Critical: Implementasi EDR / antivirus berbasis perilaku serta monitoring proses mencurigakan di endpoint pengguna
·  High: Pembatasan eksekusi file dari direktori user (AppData, Downloads) dan hardening konfigurasi browser
·  Medium: Penerapan least privilege, patching berkala, dan program security awareness pengguna

⚖️ Catatan Etika
Seluruh analisis malware dilakukan hanya pada sampel uji dan lingkungan sandbox terisolasi. Tidak ada pengujian terhadap sistem produksi atau data pengguna nyata. Dokumentasi ini disusun untuk tujuan pembelajaran dan peningkatan kesadaran keamanan.

🚀 Penutup
Portofolio ini mencerminkan pendekatan saya dalam mempelajari Cyber Security: memahami risiko, dampaknya, dan solusi yang tepat, bukan sekadar mencari celah teknis. Portofolio ini diharapkan dapat merepresentasikan kesiapan saya untuk terlibat dalam aktivitas security assessment secara profesional.
