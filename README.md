# MusifyRik — APK

Aplikasi Android resmi untuk **[musicfyrik.web.id](https://musicfyrik.web.id/)**.

Repo ini hanya berisi file APK yang siap dipasang. Kode web-nya tetap ada di repo
[`rikimusicfy`](https://github.com/aksariki541-sketch/rikimusicfy) dan **tidak diubah
sama sekali** oleh pembuatan APK ini.

## Unduh

| Versi | File | Ukuran | Catatan |
|---|---|---|---|
| **1.1** | [`MusifyRik-v1.1.apk`](./MusifyRik-v1.1.apk) | ±4,7 MB | ⭐ **Mendukung mode offline** |
| 1.0 | [`MusifyRik-v1.0.apk`](./MusifyRik-v1.0.apk) | ±406 KB | versi awal (butuh internet selalu) |

Klik file APK → tombol **Download** (ikon ⤓ di kanan atas) atau **Raw**.
Versi 1.1 bisa dipasang **menimpa** 1.0 (kunci penandatangan sama), data tidak hilang.

### Verifikasi (opsional)

```
MusifyRik-v1.1.apk
SHA-256 : 351f24ab4892b1d797744156a44eadced17a6eb7a9a749b7c1f95a90f61c0137
Versi   : 1.1 (versionCode 2)

MusifyRik-v1.0.apk
SHA-256 : 681002c186943b782b4206cc68f491411f77a96278bf3cb7867d71330a79e421
Versi   : 1.0 (versionCode 1)

Package : id.web.musicfyrik
Min SDK : Android 7.0 (API 24) · Target SDK 34
Signing : APK Signature Scheme v2 + v3
```

Cek checksum:

```bash
sha256sum MusifyRik-v1.1.apk
```

## Cara Install

1. Unduh `MusifyRik-v1.1.apk` ke HP.
2. Buka **File Manager → Download → MusifyRik-v1.1.apk**.
3. Jika muncul peringatan, izinkan **"Install unknown apps"** untuk File Manager /
   browser yang dipakai.
4. Tap **Install** → **Open**.

## Mode Offline (baru di 1.1)

Seluruh "shell" aplikasi (HTML, JS, CSS, ikon, font — 41 berkas) dibundel ke dalam
APK, lalu **hanya dipakai kalau perangkat tidak punya internet**.

Cara kerjanya:

- Permintaan ke `https://musicfyrik.web.id/...` dicegat dan dilayani dari dalam APK,
  tetapi **origin di mata WebView tetap `musicfyrik.web.id`**. Konsekuensinya:
  - domain-lock (`guard-lock.js`) tetap lolos — tidak ada halaman "Salinan tidak resmi";
  - `localStorage` / IndexedDB / Cache Storage tetap sama dengan versi online, jadi
    **koleksi, unduhan offline, dan riwayat tetap terlihat**.
- `/api/...` **tidak** dicegat → data dinamis (pencarian, streaming, chart) tetap butuh
  internet, dan tampil dengan pesan offline bawaan situs.
- `sw.js` tidak dicegat agar service worker tidak dipasang dari cache lokal.
- Kalau jaringan mati tapi OS masih bilang "online" (Wi-Fi tanpa internet / captive
  portal), aplikasi otomatis jatuh ke shell offline setelah frame utama gagal dimuat.
- Begitu internet kembali, aplikasi otomatis memuat versi terbaru dari server.
- Unduhan lagu offline tetap memakai menu **Downloads** di situs (butuh internet saat
  mengunduh pertama kali).

Yang **tetap butuh internet**: pencarian lagu baru, streaming lagu yang belum
diunduh, lirik baru, chart, dan berita konser.

## Cara Kerja Umum

Aplikasi ini adalah *WebView wrapper* yang memuat situs resmi
`https://musicfyrik.web.id/`. Pendekatan ini dipilih karena:

- Situs punya **domain-lock** (`guard-lock.js`). Kalau file web dibundel dan dibuka
  lewat `file://` atau host lain, aplikasi akan menampilkan halaman *"Salinan tidak
  resmi"*.
- API-nya same-origin (`/api/...`), jadi harus tetap berjalan di domain resmi.
- **Setiap update website, aplikasi otomatis ikut versi terbaru** tanpa perlu build
  APK baru.

### Fitur Android

- **Mode offline** (v1.1): shell statis di dalam APK, lihat bagian di atas.
- Splash screen, progress bar, dan layar "tidak ada koneksi" + tombol coba lagi.
- Tema gelap `#09090e` pada status bar dan navigation bar.
- Deep link `https://musicfyrik.web.id/...` membuka aplikasi.
- Link ke situs lain / `intent://` / `tel:` dibuka di browser atau aplikasi terkait.
- Unduhan `http(s)` memakai DownloadManager; unduhan `blob:` (mis. backup JSON di
  menu Profil) disimpan ke folder **Downloads** lewat JS bridge.
- Pemilih file untuk restore backup.
- Tombol back mengikuti riwayat WebView, tekan dua kali untuk keluar.
- Foreground service `mediaPlayback` agar pemutaran musik tidak mudah dimatikan
  sistem saat aplikasi di-background.
- Izin kamera/mikrofon ditolak (sesuai header `Permissions-Policy` situs).

## Catatan

- Penyimpanan aplikasi **terpisah dari Chrome**. Koleksi/playlist yang sudah ada di
  browser tidak otomatis pindah — gunakan **backup & restore JSON** di menu Profil.
- Kontrol di layar kunci / notifikasi media bisa sedikit berbeda dibanding PWA di
  Chrome, karena tergantung WebView bawaan tiap perangkat.
- APK ditandatangani dengan kunci rilis. Update versi berikutnya **harus** memakai
  kunci yang sama agar bisa dipasang sebagai pembaruan (bukan aplikasi baru).

## Riwayat Versi

### 1.1 — Mode offline
- Shell aplikasi (41 berkas) dibundel ke APK: aplikasi tetap terbuka walau HP
  benar-benar tanpa internet.
- Penyajian shell tetap pada origin `musicfyrik.web.id` sehingga koleksi, unduhan
  offline, dan domain-lock tetap utuh.
- Deteksi offline native (`ConnectivityManager`) + fallback otomatis saat frame
  utama gagal dimuat.
- Auto-reload ke versi server begitu koneksi kembali.
- Ukuran APK naik dari 406 KB ke 4,7 MB karena bundel aset.

### 1.0 — Rilis awal
- WebView wrapper untuk `musicfyrik.web.id` + fitur Android dasar.
