# MusifyRik — APK

Aplikasi Android resmi untuk **[musicfyrik.web.id](https://musicfyrik.web.id/)**.

Repo ini hanya berisi file APK yang siap dipasang. Kode web-nya tetap ada di repo
[`rikimusicfy`](https://github.com/aksariki541-sketch/rikimusicfy) dan **tidak diubah
sama sekali** oleh pembuatan APK ini.

## Unduh

| Versi | File | Ukuran | Catatan |
|---|---|---|---|
| **1.2** | [`MusifyRik-v1.2.apk`](./MusifyRik-v1.2.apk) | ±4,7 MB | ⭐ **Auto-update + mode offline** |
| 1.1 | [`MusifyRik-v1.1.apk`](./MusifyRik-v1.1.apk) | ±4,7 MB | mode offline |
| 1.0 | [`MusifyRik-v1.0.apk`](./MusifyRik-v1.0.apk) | ±406 KB | versi awal (selalu butuh internet) |

**Cara termudah: pakai [Releases](../../releases/latest)** — selalu berisi versi terbaru.

Klik file APK → tombol **Download** (ikon ⤓ di kanan atas) atau **Raw**.
Semua versi memakai kunci penandatangan yang sama, jadi bisa dipasang saling
**menimpa** tanpa kehilangan data.

> **Penting:** baru versi 1.2 yang punya auto-update. Jadi sekali ini kamu masih
> perlu memasang 1.2 secara manual. Setelah itu, versi 1.3 dan seterusnya bisa
> dipasang langsung dari dalam aplikasi (lihat bagian **Auto-Update**).

### Verifikasi (opsional)

```
MusifyRik-v1.2.apk
SHA-256 : 14994dd85c9994a11b4cecf9f0db69ae2bcb08bfa895f3721203b7618861fdd3
Versi   : 1.2 (versionCode 10200)

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

1. Unduh `MusifyRik-v1.2.apk` ke HP.
2. Buka **File Manager → Download → MusifyRik-v1.2.apk**.
3. Jika muncul peringatan, izinkan **"Install unknown apps"** untuk File Manager /
   browser yang dipakai.
4. Tap **Install** → **Open**.

## Auto-Update (baru di 1.2)

APK 1.2 memeriksa versi terbaru sendiri lewat GitHub Releases
(`api.github.com/repos/aksariki541-sketch/musifyrik-apk/releases/latest`),
maksimal **sekali per 24 jam**, tanpa memperlambat pembukaan aplikasi.

Alurnya:

1. Ada versi baru → dialog *"Versi 1.3 tersedia"* muncul.
2. Tap **Unduh & pasang** → APK diunduh langsung di dalam aplikasi
   (DownloadManager, ada progres di notifikasi).
3. Setelah selesai, Android menampilkan dialog **Update** → tap sekali → aplikasi
   diperbarui, data & koleksi tetap utuh.

Sekali saja, Android akan meminta izin **"Install unknown apps"** untuk MusifyRik —
aplikasi otomatis membuka halaman setelan yang tepat.

**Yang tidak bisa dilakukan:** memasang tanpa ketukan sama sekali. Android 8+
melarang pemasangan senyap oleh aplikasi biasa (hanya Play Store / app system /
device owner yang boleh). Jadi ini batas OS, bukan batas aplikasi.

Menerbitkan versi baru (untuk pemilik repo):

```bash
# 1. build dengan versionCode & versionName baru di build.sh
# 2. buat release + unggah APK-nya
gh release create v1.3 ../MusifyRik-v1.3.apk --title "MusifyRik 1.3" --notes "..."
```

Selama repo ini public dan asset release-nya berekstensi `.apk`, semua pengguna
1.2 akan mendapat tawaran pembaruan otomatis.

## Mode Offline (1.1)

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

### 1.2 — Auto-update
- Pemeriksa pembaruan harian ke GitHub Releases (tanpa login, tanpa Play Store).
- Unduh APK baru di dalam aplikasi, lanjut ke dialog pemasangan Android.
- Permintaan izin "Install unknown apps" dengan tautan langsung ke setelan.
- `ApkProvider` (pengganti FileProvider, tanpa AndroidX) untuk menyerahkan APK
  ke paket installer.
- Mode offline 1.1 tetap ada.
- versionCode pindah ke skema `major*10000 + minor*100 + patch` agar sinkron
  dengan tag rilis (`v1.2` → 10200).

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
