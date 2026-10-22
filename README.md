# MusifyRik — APK

Aplikasi Android resmi untuk **[musicfyrik.web.id](https://musicfyrik.web.id/)**.

Repo ini hanya berisi file APK yang siap dipasang. Kode web-nya tetap ada di repo
[`rikimusicfy`](https://github.com/aksariki541-sketch/rikimusicfy) dan **tidak diubah
sama sekali** oleh pembuatan APK ini.

## Unduh

| Versi | File | Ukuran |
|---|---|---|
| 1.0 | [`MusifyRik-v1.0.apk`](./MusifyRik-v1.0.apk) | ±406 KB |

Klik file APK di atas → tombol **Download** (ikon ⤓ di kanan atas) → atau
**Raw** / *View raw*.

### Verifikasi (opsional)

```
SHA-256 : 681002c186943b782b4206cc68f491411f77a96278bf3cb7867d71330a79e421
Package : id.web.musicfyrik
Versi   : 1.0 (versionCode 1)
Min SDK : Android 7.0 (API 24) · Target SDK 34
Signing : APK Signature Scheme v2 + v3
```

Cek checksum di Android/desktop:

```bash
sha256sum MusifyRik-v1.0.apk
```

## Cara Install

1. Unduh `MusifyRik-v1.0.apk` ke HP.
2. Buka **File Manager → Download → MusifyRik-v1.0.apk**.
3. Jika muncul peringatan, izinkan **"Install unknown apps"** untuk aplikasi
   File Manager / browser yang kamu pakai.
4. Tap **Install** → **Open**.

## Cara Kerja

Aplikasi ini adalah *WebView wrapper* yang memuat situs resmi
`https://musicfyrik.web.id/`. Pendekatan ini dipilih karena:

- Situs punya **domain-lock** (`guard-lock.js`). Kalau file web dibundel ke dalam
  APK lewat `file://` atau host lain, aplikasi akan menampilkan halaman
  *"Salinan tidak resmi"* dan tidak bisa dipakai.
- API-nya same-origin (`/api/...`), jadi harus tetap berjalan di domain resmi.
- **Setiap update website, aplikasi otomatis ikut versi terbaru** tanpa perlu
  build APK baru.

### Fitur Android

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

- **Butuh koneksi internet**, sama seperti versi web.
- Penyimpanan aplikasi **terpisah dari Chrome**. Koleksi/playlist yang sudah ada di
  browser tidak otomatis pindah — gunakan **backup & restore JSON** di menu Profil.
- Kontrol di layar kunci / notifikasi media bisa sedikit berbeda dibanding PWA di
  Chrome, karena tergantung WebView bawaan tiap perangkat.
- APK ini ditandatangani dengan kunci rilis. Update versi berikutnya **harus**
  memakai kunci yang sama agar bisa dipasang sebagai pembaruan (bukan aplikasi baru).
