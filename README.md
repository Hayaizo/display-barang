# Display Barang

Aplikasi katalog barang berbasis HTML + Firebase SDK modular, siap dihubungkan ke Vercel dan Firebase. Tidak memerlukan build step atau package manager.

## Fitur
- Email/password registration dan login via Firebase Authentication
- Role pengguna: `viewer`, `editor`, `admin`
- CRUD katalog barang (nama, kategori, harga, deskripsi)
- Upload foto ke Firebase Storage
- Realtime catalog dari Cloud Firestore
- Search, filter kategori, export JSON
- UI responsif untuk HP dan desktop

## 1. Siapkan Firebase
1. Buat project di https://console.firebase.google.com/
2. Tambahkan Web App di Project settings > General > Your apps.
3. Salin konfigurasi web app ke `firebase-config.js`, ganti seluruh nilai `ISI_...`.
4. Authentication > Sign-in method > aktifkan **Email/Password**.
5. Authentication > Settings > Authorized domains: tambahkan domain Vercel kamu.
6. Firestore Database > Create database.
7. Storage > Get started.
8. Tempel isi `firestore.rules` ke Firestore Database > Rules lalu Publish.
9. Tempel isi `storage.rules` ke Storage > Rules lalu Publish.

> Jangan biarkan Firestore atau Storage dalam mode test/public. Gunakan rules yang disediakan dan uji melalui akun dengan role berbeda.

## 2. Deploy ke Vercel
1. Impor repository GitHub `Hayaizo/display-barang` di https://vercel.com/new.
2. Framework preset: **Other**.
3. Build command: kosongkan / None. Output directory: `.` (root).
4. Deploy. Setiap commit baru ke branch yang terhubung akan memicu deployment.
5. Tambahkan domain Vercel ke Firebase Authentication > Authorized domains.

## 3. Jadikan akun pertama sebagai admin
Demi keamanan, pendaftaran publik selalu menghasilkan role `viewer`. Setelah mendaftar dan login sekali:
1. Buka Firebase Console > Firestore Database > Data.
2. Buka koleksi `users`, pilih dokumen dengan ID UID akunmu (bisa dilihat di Authentication > Users).
3. Ubah field `role` dari `viewer` menjadi string `admin`.
4. Refresh aplikasi. Akun admin dapat mengelola barang dan mengubah role akun lain.

Jangan pernah memberikan role admin melalui form pendaftaran publik. Jika semua admin kehilangan akses, ubah role lewat Firebase Console sebagai pemilik project.

## 4. Role
- `viewer`: membaca katalog
- `editor`: membaca, menambah, mengedit, menghapus barang
- `admin`: semua hak editor + mengubah role dan mengelola profil pengguna

Pengguna bisa membuat akun sendiri. Admin mengubah role dari tab **Pengguna**. Penghapusan dokumen profil tidak menghapus akun Authentication; untuk menonaktifkan atau menghapus akun secara menyeluruh gunakan Firebase Console atau backend tepercaya dengan Firebase Admin SDK.

## 5. Catatan keamanan
- Firebase web config adalah konfigurasi client, bukan service-account secret. Jangan pernah commit service account JSON atau private key.
- Security Rules adalah lapisan keamanan sebenarnya; menyembunyikan tombol di UI bukan pengamanan.
- Aplikasi ini belum memakai backend Admin SDK, verifikasi email wajib, reset password khusus, atau audit log.
- Atur Firebase App Check dan batasan API key sesuai kebutuhan sebelum penggunaan produksi.
- Rules Storage menggunakan akses baca dokumen Firestore untuk mengecek role; pastikan Firestore dan Storage di project yang sama dan rules telah dipublikasikan.
- Untuk data inventaris sensitif, batasi pendaftaran akun dan akses baca sesuai kebutuhan bisnis.

## File
- `index.html`: seluruh UI dan logika client
- `firebase-config.js`: konfigurasi Firebase web app
- `firestore.rules`: aturan akses data
- `storage.rules`: aturan akses foto
- `vercel.json`: header keamanan dasar Vercel
