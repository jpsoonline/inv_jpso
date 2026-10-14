# Cetak Rekap & Invoice JPSO - PWA + GitHub Pages + Firebase (Firestore)

## Isi paket
| File | Fungsi | Ke GitHub? |
|---|---|---|
| `index.html` | **Dibuat sendiri**: hasil bungkus `Cetak_Rekap_Invoice_JPSO_sumber.html` dengan `Alat_Lisensi_JPSO.html` (lihat bagian B), lalu ganti nama jadi `index.html` | Ya |
| `manifest.json`, `sw.js`, `icon-*.png` | PWA (bisa di-install, jalan offline) | Ya |
| `firebase-config.js` | Konfigurasi Firebase (isi sendiri) | Ya |
| `admin.html` | Halaman admin untuk mendaftarkan/mencabut akun Google pengguna | Ya |
| `.nojekyll` | Agar GitHub Pages tidak memproses file | Ya |
| `firestore.rules` | Security Rules (ditempel di Firebase Console) | Tidak perlu |
| `_RAHASIA_jangan_diunggah/` (aplikasi sumber, alat lisensi, kunci master) | File asli/rahasia | **JANGAN diunggah** |

## A. Siapkan Firebase (sekali)
Jika Anda sudah punya proyek Firebase untuk Rekap OS, **pakai proyek yang sama**: langkah 1-5 tidak perlu diulang, akun yang sudah terdaftar di `licenses` otomatis berlaku juga untuk aplikasi ini (hanya langkah 6 dan 7 untuk alamat baru).
1. https://console.firebase.google.com > **Add project** (paket gratis Spark cukup).
2. **Build > Authentication > Get started > Sign-in method > Google > Enable**.
3. **Authentication > Settings > Authorized domains > Add domain**: `USERNAME.github.io`.
4. **Build > Firestore Database > Create database** (mode production, lokasi `asia-southeast2` Jakarta).
5. Tab **Rules**: tempel isi `firestore.rules`, **ganti `ADMIN_EMAIL_DISINI@gmail.com` dengan email Google admin (huruf kecil)**, lalu **Publish**.
6. **Project settings (roda) > General > Your apps > Web (`</>`)** > daftarkan app > salin objek `firebaseConfig` ke `firebase-config.js` (nama variabelnya `window.JPSO_FB`).
7. (Disarankan) Google Cloud Console > APIs & Services > Credentials > API key proyek > *Website restrictions* > tambahkan `https://USERNAME.github.io/*`.

## B. Bungkus aplikasi + tanam lisensi (aktivasi tidak hilang)
1. Buka `_RAHASIA_jangan_diunggah/Alat_Lisensi_JPSO.html`. Gunakan **kunci master yang sama** dengan Rekap OS: **Pulihkan dari cadangan (.json)** atau **Impor master** (file `master_key_....json`). Lisensi lama dari master yang sama tetap berlaku.
2. **2. Bungkus aplikasi** > pilih `Cetak_Rekap_Invoice_JPSO_sumber.html`. Hasilnya `..._berlisensi.html`.
3. Buka hasil bungkus itu di alamat web (setelah langkah C, atau untuk uji coba lewat alamat GitHub Pages) untuk melihat **Kode Perangkat**.
4. **3. Buat lisensi**: tempel Kode Perangkat, isi nama pemegang, tanggal berlaku > **Buat lisensi**.
5. Klik **Tanam lisensi ke file aplikasi...** > pilih file hasil langkah 2. Hasilnya file baru yang **sudah membawa lisensi**: aplikasi tidak meminta aktivasi lagi walau data browser/cache dihapus. Ganti namanya jadi **`index.html`** lalu unggah ke GitHub.

**Catatan penting tentang lisensi tertanam di GitHub Pages**
- Satu `index.html` hanya bisa membawa **satu** lisensi (terikat ke satu Kode Perangkat). Perangkat lain yang membuka alamat yang sama tetap melihat layar aktivasi dan menempel lisensinya sendiri (cukup sekali per perangkat, tetapi **harus diulang jika data situs dihapus**).
- File di GitHub Pages bersifat publik. Lisensi yang tertanam ditandatangani dan terikat ke ciri perangkat pemiliknya, jadi tidak berlaku di perangkat lain. Kalau ingin tiap pengguna tahan hapus data, buat satu repo/alamat per pengguna dengan `index.html` miliknya masing-masing.
- Login Google dan pengaturan **tidak** termasuk yang ditanam: setelah data browser dihapus, pengguna cukup **Masuk dengan Google** lagi dan pengaturan kembali otomatis dari Firestore.

## C. GitHub Pages
1. Buat repo baru (mis. `jpso`), **Public**.
2. Unggah (Add file > Upload files): `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `firebase-config.js`, `admin.html`, `.nojekyll`.
3. **Settings > Pages > Deploy from a branch > main / (root) > Save**. Alamat: `https://USERNAME.github.io/jpso/`.

## D. Daftarkan pengguna (admin)
1. Buka `https://USERNAME.github.io/jpso/admin.html`, masuk dengan Google admin.
2. Isi email pengguna, status `active`, tanggal berlaku (opsional), **Simpan**. Untuk mencabut: tombol **Cabut**.

## E. Pemakaian
1. Buka alamat aplikasi di Chrome/Edge. Layar aktivasi hanya muncul jika lisensi belum tertanam/belum ditempel.
2. **Masuk dengan Google**. Jika akun terdaftar dan aktif, aplikasi terbuka; nama akun dan status lisensi tampil di bawah judul.
3. Klik ikon install di address bar (atau menu > Install) agar berjalan seperti aplikasi.
4. Yang tersinkron antar perangkat: Fee, PPN, Dasar PPN, PPH, nama & jabatan penanda tangan, periode yang sudah ditutup, lebar panel, dan tanda tangan yang diunggah sendiri. Tombol **Sinkron sekarang** dan **Keluar akun** ada di bawah judul.
5. Tidak pernah diunggah: data hasil baca Rekap_JPSO, PDF, dan folder penyimpanan (izin folder tersimpan per perangkat; pilih folder sekali di tiap perangkat, lalu **Simpan sebagai default**).

## F. Memperbarui aplikasi
Bungkus ulang `Cetak_Rekap_Invoice_JPSO_sumber.html` > tanam lisensi > ganti nama `index.html` > unggah menimpa file lama. Jika perubahan tidak muncul, naikkan nomor `jpso-v1` di `sw.js` (mis. `jpso-v2`).

## Hal penting
- **Masa tenggang offline 7 hari**: setelah verifikasi akun online terakhir, aplikasi tetap bisa dibuka offline sampai 7 hari. Pencabutan/kedaluwarsa berlaku saat verifikasi online berikutnya.
- Kunci di `firebase-config.js` memang publik. Yang melindungi data adalah login + Security Rules. Pengecekan lisensi di sisi browser adalah penghalang tingkat menengah, bukan perlindungan mutlak; pengecekan sungguhan di server memerlukan Cloud Functions (paket Blaze).
- Jika `firebase-config.js` belum diisi, aplikasi berjalan lokal tanpa login dan tanpa sinkronisasi.
- Memilih folder (File System Access API) hanya didukung di Chrome/Edge desktop.
- Jika aplikasi lain (mis. Rekap OS) memakai alamat `USERNAME.github.io` yang sama, data aktivasi dan pengaturannya tidak bentrok: aplikasi ini memakai nama penyimpanan `jpso_*` sendiri.
