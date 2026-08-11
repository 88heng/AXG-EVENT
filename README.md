[Uploading README (1).md…]()
# AXG EVENT

Website pendaftaran event AXG beserta Panel Admin. Halaman Public dan Admin memakai satu Google Apps Script API yang menyimpan data ke Google Sheets.

## Struktur repository

```text
index.html   # Website Public / pendaftaran peserta
admin.html   # Panel Admin
```

File backend `AXG-EVENT-APPS-SCRIPT.gs` **tidak** diunggah ke GitHub Pages. File tersebut ditempelkan ke proyek Google Apps Script yang terhubung ke Google Sheet database.

## Publikasi dengan GitHub Pages

1. Buat repository GitHub baru.
2. Unggah halaman Public dengan nama `index.html`.
3. Unggah Panel Admin dengan nama `admin.html`.
4. Buka **Settings → Pages** pada repository.
5. Pada **Build and deployment**, pilih **Deploy from a branch**.
6. Pilih branch `main` dan folder `/(root)`, lalu klik **Save**.
7. Setelah GitHub selesai membangun situs, buka alamat yang ditampilkan pada halaman tersebut.

Halaman yang tersedia:

- Public: `https://USERNAME.github.io/NAMA-REPOSITORY/`
- Admin: `https://USERNAME.github.io/NAMA-REPOSITORY/admin.html`

## Menyambungkan Google Apps Script

1. Buat Google Sheet baru, misalnya bernama **AXG EVENT Database**.
2. Dari Sheet, buka **Extensions → Apps Script**.
3. Hapus kode bawaan dan tempel isi `AXG-EVENT-APPS-SCRIPT.gs`.
4. Simpan proyek.
5. Jalankan fungsi `setupAxgEventSheets` satu kali dan berikan izin Google yang diminta. Sheet database akan dibuat otomatis.
6. Buka **Project Settings → Script properties**, lalu buat properti berikut:

| Property | Value |
| --- | --- |
| `ADMIN_API_TOKEN` | Token Admin yang panjang, unik, dan rahasia |

7. Pilih **Deploy → New deployment → Web app**.
8. Atur **Execute as: Me**. Untuk akses Public tanpa akun Google, pilih akses publik/anonymous jika tersedia pada akun Anda.
9. Klik **Deploy**, selesaikan izin, kemudian salin URL yang berakhir dengan `/exec`.

> Gunakan URL `/exec` untuk situs. URL `/dev` hanya untuk pengujian dari editor Apps Script.

## Konfigurasi API di HTML

Pada `index.html` dan `admin.html`, cari konfigurasi berikut:

```js
const AXG_CONFIG = Object.freeze({
  API_URL: 'PASTE_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE',
  ENABLE_MOCK_FALLBACK: true
});
```

Ganti dengan URL `/exec` hasil deployment. Setelah koneksi berhasil, ubah `ENABLE_MOCK_FALLBACK` menjadi `false` agar situs tidak kembali memakai data contoh saat API bermasalah.

```js
const AXG_CONFIG = Object.freeze({
  API_URL: 'https://script.google.com/macros/s/ID_DEPLOYMENT/exec',
  ENABLE_MOCK_FALLBACK: false
});
```

Commit dan push kedua file HTML lagi ke GitHub agar versi situs ikut diperbarui.

## Login Admin

Buka `admin.html`, lalu masukkan nilai `ADMIN_API_TOKEN` yang dibuat pada Script Properties. Token tidak boleh ditulis di `index.html`, `admin.html`, atau repository GitHub.

## Catatan keamanan

- Jangan pernah commit token Admin, password, atau URL rahasia.
- Data Telegram peserta hanya untuk Admin dan tidak ditampilkan di halaman Public.
- Untuk setiap perubahan Apps Script, pilih **Deploy → Manage deployments → Edit**, pilih versi baru, lalu **Deploy** kembali.

## Pengujian singkat

1. Buka halaman Public dan pastikan event tampil.
2. Buat satu pendaftaran uji.
3. Buka Admin, login dengan token, lalu periksa data peserta.
4. Ubah status/event dari Admin dan refresh halaman Public untuk memastikan data tersinkronisasi.
