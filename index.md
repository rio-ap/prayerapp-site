# Kebijakan Privasi — Prayer Alarm

**Terakhir diperbarui:** 22 November 2026
**Aplikasi:** Prayer Alarm (`com.trazire.prayerapp`), versi 1.0
**Pengembang:** Trazire Gamedev Team

## 1. Ringkasan

Prayer Alarm adalah aplikasi pengingat waktu shalat. Aplikasi ini:

- **tidak** membuat akun dan **tidak** meminta login;
- **tidak** menampilkan iklan;
- **tidak** memakai analitik, pelacak, atau pelaporan crash pihak ketiga;
- **tidak** menjual atau menyewakan data Anda kepada siapa pun;
- menyimpan jadwal, pengaturan, dan lokasi pilihan Anda **di perangkat Anda sendiri**.

Data hanya meninggalkan perangkat dalam tiga keadaan: (a) meminta jadwal shalat ke sumber
jadwal (Kemenag, dan sebagai cadangan Aladhan), (b) layanan lokasi sistem operasi yang
menyediakan posisi perangkat saat Anda meminta deteksi, dan (c) layanan geocoding sistem
operasi, yang menerjemahkan koordinat menjadi nama kota **dan** menerjemahkan nama kota yang
Anda pilih menjadi koordinat — yang terakhir ini juga terjadi bila Anda tidak pernah memberi
izin lokasi (lihat bagian 4c).

## 2. Data yang diproses

| Data | Kapan | Disimpan di perangkat? |
|---|---|---|
| **Lokasi perkiraan dan presisi** (garis lintang/bujur dari GPS) | Dari GPS hanya setelah Anda menekan "Deteksi Lokasi Otomatis" di Beranda atau "Perbarui Lokasi" di layar Jadwal, dan hanya dengan izin lokasi. Untuk kota yang Anda pilih manual, koordinatnya berasal dari geocoding nama kota (bagian 4c) — tanpa membaca posisi perangkat | Ya — posisi yang dipakai untuk kota tersebut disimpan agar jadwal dan arah kiblat tetap bekerja tanpa internet: koordinat hasil geocoding kota, atau posisi perangkat itu sendiri bila hanya provinsinya yang dikenali (aplikasi lalu memakai ibu kota provinsi) |
| **Kota/provinsi pilihan Anda** (nama, atau ID provinsi/kota Kemenag bila dipilih dari daftar) | Saat deteksi otomatis atau pencarian kota manual | Ya |
| **Jadwal shalat bulanan** (waktu shalat, tanggal, sumber) | Setelah jadwal berhasil diambil | Ya — berkas cache di penyimpanan internal aplikasi, kedaluwarsa setelah 7 hari |
| **Pengaturan aplikasi** (waktu shalat yang aktif, pengingat, nada alarm, tema) | Saat Anda mengubahnya | Ya |

Aplikasi tidak meminta nama, alamat email, nomor telepon, kontak, foto, berkas, mikrofon,
kamera, riwayat penelusuran, atau data kesehatan. Aplikasi tidak membaca ID iklan
(`AD_ID`) dan tidak menyertakan SDK periklanan.

## 3. Izin dan pilihan Anda

- **Lokasi (`ACCESS_COARSE_LOCATION`, `ACCESS_FINE_LOCATION`)** diminta lewat dialog sistem
  hanya ketika Anda memilih deteksi otomatis (Beranda) atau "Perbarui Lokasi" (Jadwal).
  Menolak izin ini tidak mematikan aplikasi: Anda dapat memilih provinsi dan kota/kabupaten
  secara manual, dan dalam jalur manual itu tidak ada posisi perangkat yang dibaca. Yang tetap
  terjadi hanyalah nama kota yang Anda pilih dikirim ke layanan geocoding sistem untuk
  diterjemahkan menjadi koordinat (bagian 4c); tanpa hasil geocoding, aplikasi memakai titik
  pusat ibu kota provinsi yang sudah ada di dalam aplikasi.
- **Notifikasi (`POST_NOTIFICATIONS`)** dipakai untuk menampilkan alarm dan pengingat waktu
  shalat. Penolakannya terlihat jelas di dalam aplikasi.
- **Alarm presisi (`SCHEDULE_EXACT_ALARM`)** dipakai agar alarm berbunyi tepat waktu. Tanpa
  izin ini alarm tetap berbunyi, tetapi dapat terlambat hingga 10 menit; aplikasi menjelaskan
  hal ini sebelum membuka layar pengaturan sistem.
- **`INTERNET`, `ACCESS_NETWORK_STATE`, `RECEIVE_BOOT_COMPLETED`, `VIBRATE`, `WAKE_LOCK`**
  dipakai untuk mengambil jadwal, memulihkan alarm setelah perangkat dinyalakan ulang,
  bergetar, dan membunyikan alarm saat layar mati. Tidak satu pun dari izin ini mengumpulkan
  data pribadi.

## 4. Ke mana data dikirim

Kami tidak mengoperasikan server. Data hanya dikirim ke pihak berikut, dan hanya seperti
yang dijelaskan di bawah.

**a. Kementerian Agama RI — Bimas Islam (`bimasislam.kemenag.go.id`).**
Sumber utama jadwal shalat. Saat meminta jadwal bulanan, aplikasi mengirim **ID provinsi,
ID kota/kabupaten, bulan, dan tahun**. Aplikasi **tidak** mengirim koordinat GPS ke Kemenag.
Nama kota yang Anda pilih (atau yang terdeteksi dari GPS) dicocokkan **di dalam perangkat**
dengan daftar kota Kemenag; yang dikirim ke server hanya ID wilayah.

**b. Layanan Aladhan (`api.aladhan.com`) — hanya sebagai cadangan.**
Jika jadwal Kemenag gagal diambil (misalnya server tidak dapat dihubungi), aplikasi meminta
jadwal ke Aladhan dan mengirim **garis lintang, garis bujur, bulan, tahun, dan metode
perhitungan**. Ini satu-satunya layanan di luar platform perangkat yang menerima koordinat
(layanan lokasi dan geocoding platform dibahas di (c)). Jika Kemenag berhasil, Aladhan tidak
dihubungi sama sekali. Bila kedua sumber gagal, aplikasi menghitung jadwal **di dalam
perangkat** tanpa mengirim koordinat ke mana pun.

**c. Layanan lokasi dan geocoding sistem operasi (Android, termasuk Google Play services).**
Aplikasi memakai dua API platform, dan keduanya dapat ditangani oleh penyedia layanan perangkat
(pada perangkat bersertifikasi Google: Google) sebagai bagian dari sistem operasi.

- **Penyedia lokasi.** Posisi perangkat diambil lewat Fused Location Provider dari Google Play
  services — bukan dihitung sendiri oleh aplikasi. Panggilan ini hanya terjadi setelah Anda
  meminta deteksi (Beranda) atau "Perbarui Lokasi" (Jadwal) dan memberi izin lokasi; tanpa izin,
  panggilan itu tidak pernah dibuat.
- **Geocoding dua arah.** API `Geocoder` milik Android dipakai untuk (i) menerjemahkan koordinat
  menjadi nama kota setelah deteksi, dan (ii) menerjemahkan **nama kota yang Anda pilih menjadi
  koordinat** saat Anda memilih kota secara manual — permintaan ini diulang setiap kali layar
  jadwal memuat kota tersebut, bukan hanya sekali saat kota dipilih. Permintaan (ii) berisi teks
  seperti "Kota Bandung, Jawa Barat, Indonesia" dan **tetap dikirim meskipun Anda tidak pernah
  memberi izin lokasi** — teks itu adalah nama tempat, bukan posisi perangkat. Tanpa hasil dari
  layanan ini, aplikasi memakai titik pusat ibu kota provinsi yang tersimpan di dalam aplikasi.

Aplikasi tidak menyimpan hasil geocoding di server mana pun dan tidak mengirimkannya ke pihak
lain selain sebagai koordinat pada permintaan jadwal ke (a)/(b) di atas. Kami tidak
mengendalikan kebijakan penyedia layanan platform tersebut; permintaan ini tunduk pada
kebijakan privasi sistem operasi perangkat Anda.

**d. Tidak ada pihak lain.** Selain sumber jadwal di (a) dan (b) serta layanan platform di (c),
tidak ada penerima data: tidak ada jaringan iklan, broker data, atau penyedia analitik yang
menerima data dari aplikasi ini.

Seperti setiap permintaan web, server tujuan di atas dapat mencatat alamat IP dan waktu
permintaan sebagai bagian dari log operasional mereka. Aplikasi tidak mengirim pengenal
pengguna, akun, atau ID perangkat apa pun bersama permintaan tersebut. Semua koneksi memakai
HTTPS; aplikasi menolak lalu lintas HTTP biasa (cleartext).

## 5. Yang tidak kami lakukan

- Tidak ada akun, pendaftaran, atau profil pengguna.
- Tidak ada iklan dan tidak ada pembagian data untuk iklan atau pemasaran.
- Tidak ada analitik pemakaian, tidak ada pelaporan crash, tidak ada SDK Firebase.
- Tidak ada penjualan, penyewaan, atau pertukaran data pribadi.
- Tidak ada akses ke kontak, foto, berkas, mikrofon, kamera, atau lokasi latar belakang.

Aplikasi tidak meminta lokasi di latar belakang. Posisi perangkat hanya dibaca saat Anda
menekan "Deteksi Lokasi Otomatis" di Beranda atau "Perbarui Lokasi" di layar Jadwal, yaitu
saat aplikasi sedang terbuka di layar itu; setelah tersimpan, alarm dan arah kiblat memakai
posisi tersebut tanpa membaca GPS lagi.

## 6. Penyimpanan dan penghapusan

Semua data tersimpan di penyimpanan internal aplikasi pada perangkat Anda (tidak dapat
dibaca aplikasi lain di perangkat yang tidak di-root), dan **tidak** disertakan dalam
pencadangan cloud Android — aplikasi menetapkan `android:allowBackup="false"`.

Anda dapat menghapus semuanya kapan saja:

- hapus data aplikasi lewat **Setelan → Aplikasi → Prayer Alarm → Penyimpanan → Hapus data**;
  atau
- hapus pemasangan aplikasi.

Setelah itu tidak ada salinan data yang tersisa di perangkat. Karena kami tidak memiliki
server atau akun pengguna, tidak ada data di sisi kami yang perlu dihapus.

## 7. Keamanan

Semua komunikasi jaringan memakai HTTPS dengan sertifikat sistem, dan konfigurasi keamanan
jaringan aplikasi memblokir lalu lintas cleartext. Cookie sesi Kemenag hanya hidup di memori
selama aplikasi berjalan dan tidak pernah ditulis ke disk.

## 8. Anak-anak

Aplikasi ini adalah utilitas umum untuk pengguna dewasa dan remaja; aplikasi **tidak**
ditujukan kepada anak-anak, dan tidak mengumpulkan data pribadi apa pun dari siapa pun,
termasuk anak-anak. Aplikasi tidak memiliki iklan, pembelian, atau konten yang dibuat
pengguna, sehingga tidak ada jalur bagi anak untuk membagikan informasi.

## 9. Hak Anda

Karena aplikasi tidak memiliki akun dan tidak menyimpan data di server kami, kendali penuh
ada di perangkat Anda: Anda dapat melihat data apa yang tersimpan (jadwal, kota, pengaturan),
menghapusnya (bagian 6), dan menolak atau mencabut izin lokasi serta notifikasi kapan saja
lewat Setelan Android. Mencabut izin lokasi tidak menghapus kota yang sudah tersimpan; pilih
kota lain atau hapus data aplikasi untuk menggantinya.

## 10. Perubahan kebijakan

Jika kebijakan ini berubah, tanggal "Terakhir diperbarui" di atas akan diubah dan versi baru
dipublikasikan di URL yang sama. Perubahan yang memengaruhi cara data diproses akan
dijelaskan di catatan rilis aplikasi.

## 11. Kontak

Pertanyaan tentang kebijakan ini atau tentang data di aplikasi:

**support@trazire.com**
Trazire Gamedev Team

---

## English summary (non-binding)

Prayer Alarm is a prayer-time reminder utility. It creates no account, shows no ads, and
contains no analytics, tracking, or crash-reporting SDKs. It keeps its schedule cache,
settings, and the coordinates of the city you selected in the app's private storage on your
device, and it sets `android:allowBackup="false"` so that data is not uploaded to cloud
backup. Location (approximate and precise) is read only after you tap automatic detection and
grant the permission; choosing a city manually needs no location permission. Coordinates are sent
to **api.aladhan.com** only when the primary source (Kemenag, `bimasislam.kemenag.go.id`,
which receives province/city IDs, month and year but never coordinates) fails; if both fail,
prayer times are computed on the device. The device position itself comes from the platform
Fused Location Provider (Google Play services), and geocoding runs both ways through the
Android `Geocoder` API: coordinates into a city name after detection, and — even if you never
grant location permission — the city name you pick into coordinates when you select a city
manually. Both may be served by the device's platform provider (Google on certified devices)
and are subject to that provider's privacy policy. Deleting the app or clearing its data
erases everything the app stored.
