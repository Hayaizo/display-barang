# Display Barang

Katalog publik sederhana berbasis HTML + Firebase SDK modular, siap untuk hosting statis di Vercel. Tidak perlu build step.

## Fitur
- Katalog publik: pengunjung tidak perlu login.
- Tombol Beli membuka WhatsApp moderator dengan template pesan.
- Member Share membuat materi promosi berisi foto, deskripsi, harga, dan kode posting unik.
- Moderator/admin mengelola barang dan besaran insentif.
- Moderator/admin mencatat pesanan dari WhatsApp, mengidentifikasi kode posting, memverifikasi pesanan, dan mencatat pembayaran insentif.
- Admin mengelola peran member, moderator, dan admin.
- Foto disimpan di Firebase Storage; katalog, member, materi share, dan pesanan di Firestore.

## Setup Firebase
1. Buat project Firebase.
2. Aktifkan Authentication > Email/Password.
3. Buat Firestore Database dan aktifkan Storage.
4. Isi konfigurasi web app pada \`firebase-config.js\`.
5. Salin isi \`firestore.rules\` ke Firestore Rules lalu Publish.
6. Salin isi \`storage.rules\` ke Storage Rules lalu Publish.
7. Pada Firebase Authentication > Settings > Authorized domains, tambahkan domain Vercel.
8. Deploy repo ini ke Vercel sebagai static site (Framework Preset: Other, Build Command kosong, Output Directory \`.\`).
9. Isi konstanta \`MODERATOR_WA\` pada \`index.html\` dengan nomor WhatsApp moderator format internasional tanpa tanda +, misalnya \`6281234567890\`.
10. Daftarkan akun admin pertama seperti biasa. Di Firestore, buka dokumen \`users/{UID}\` untuk akun tersebut lalu ubah \`role\` dari \`member\` menjadi \`admin\`. UID ada di Firebase Authentication.
11. Login kembali. Admin bisa mengubah peran member menjadi \`moderator\` atau \`admin\`.

## Cara kerja
- Pengunjung buka halaman utama dan melihat barang tanpa login.
- Beli membuka chat WhatsApp moderator. Pesanan dicatat moderator dari dashboard.
- Member login, pilih Share pada barang, lalu salin materi dan kode posting unik untuk dibagikan ke grup.
- Ketika pembeli menghubungi moderator, moderator memasukkan kode pada form Catat Pesanan. Sistem mencari pemilik kode dan menghubungkan pesanan ke member tersebut.
- Kode posting membantu pencatatan, tetapi tidak membuktikan transaksi selesai. Moderator harus memeriksa bukti percakapan dan status pesanan sebelum memverifikasi insentif.

## Catatan
- Insentif hanyalah catatan internal, bukan transfer otomatis. Moderator/admin menandai status pembayaran setelah pembayaran benar-benar dilakukan.
- Kode posting dapat diteruskan atau disalin orang lain; gunakan bukti posting/chat jika terjadi sengketa.
- Konfigurasi Firebase web berisi identifier client publik; keamanan tetap harus ditegakkan lewat Firebase Security Rules.
