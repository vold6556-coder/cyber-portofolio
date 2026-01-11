# Reconnaissance & Scanning

Bagian ini mendokumentasikan aktivitas reconnaissance awal yang dilakukan pada lingkungan lab terizin sebagai tahap awal security assessment.

## Tujuan
Reconnaissance bertujuan untuk:
- Mengidentifikasi permukaan serangan (attack surface)
- Menemukan indikasi awal security misconfiguration
- Mengumpulkan informasi tanpa eksploitasi agresif

## Aktivitas Reconnaissance
- Observasi akses aplikasi DVWA melalui browser
- Identifikasi error infrastruktur (Cloudflare Error 523)
- Discovery directory listing menggunakan teknik pencarian terbuka (Google Dork)

Seluruh aktivitas dilakukan secara pasif dan hanya pada sistem uji, tanpa melakukan eksploitasi atau bypass kontrol keamanan.

## Output
Hasil reconnaissance menjadi dasar untuk tahap technical findings, khususnya pada kategori:
- Security Misconfiguration
- Availability & infrastructure exposure


