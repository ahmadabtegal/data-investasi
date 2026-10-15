# Dashboard Data Proyek & KBLI — GitHub Pages + Apps Script

## Konsep
- **Frontend:** `index.html` di-host pada GitHub Pages.
- **Backend/API:** `Code.gs` pada Google Apps Script.
- **Penyimpanan:** Google Spreadsheet yang berisi data proyek.
- **Pembaruan:** tombol “Perbarui Data” membaca data terbaru dari spreadsheet.
- **KPI:** total proyek, jumlah KBLI unik (berdasarkan kode pada kolom `Kbli`), total investasi, total TKI, dan perusahaan unik.
- **Visualisasi:** tren tahun terbit OSS, kecamatan, risiko, skala usaha, sektor berdasarkan jumlah proyek/investasi, KBLI teratas, dan status penanaman modal.
- **Tabel:** pencarian dan filter kecamatan/risiko di browser. Versi awal ini bersifat baca-saja.

## A. Siapkan spreadsheet
1. Unggah `DP.Proyek (4).xlsx` ke Google Drive.
2. Buka sebagai Google Spreadsheet (konversi Excel ke format Google Sheets).
3. Pastikan baris pertama adalah header dan nama sheet sesuai `Sheet 1` (atau ubah `SHEET_NAME` pada `Code.gs`).
4. Salin ID spreadsheet dari URL. ID adalah bagian di antara `/d/` dan `/edit`.

Kolom yang dikenali antara lain `Id Proyek`, `Nama Perusahaan`, `Tanggal Terbit Oss`, `Uraian Status Penanaman Modal`, `Uraian Jenis Perusahaan`, `Uraian Risiko Proyek`, `Uraian Skala Usaha`, `kecamatan_usaha`, `Kbli`, `Judul Kbli`, `KL/Sektor Pembina`, `Jumlah Investasi`, dan `TKI`.

## B. Pasang backend Apps Script
1. Dari spreadsheet, pilih **Extensions > Apps Script**.
2. Tempel isi `Code.gs`.
3. Ganti `GANTI_DENGAN_ID_SPREADSHEET` dengan ID spreadsheet.
4. Periksa `SHEET_NAME = 'Sheet 1'`.
5. Klik **Deploy > New deployment**.
6. Pilih tipe **Web app**.
7. **Execute as:** Me.
8. **Who has access:** Anyone jika data memang dimaksudkan untuk dashboard publik. Jangan publikasikan data rahasia/pribadi.
9. Klik Deploy, izinkan akses, lalu salin URL yang berakhiran `/exec`.

## C. Hubungkan frontend
1. Buka `index.html`.
2. Cari `API_URL: "GANTI_DENGAN_URL_WEB_APP_APPS_SCRIPT"`.
3. Ganti dengan URL `/exec` hasil deployment.
4. Unggah `index.html` ke repository GitHub.
5. Pada repository, buka **Settings > Pages**, pilih branch dan folder tempat file berada, lalu simpan.
6. Buka URL GitHub Pages yang diberikan. Klik **Perbarui Data** untuk menguji.

## D. Catatan jumlah KBLI
KPI **Jumlah KBLI Unik** menghitung jumlah nilai berbeda yang terisi pada kolom `Kbli`. Baris dengan kode KBLI kosong tidak dihitung. Grafik “KBLI yang paling sering muncul” mengelompokkan berdasarkan `Judul Kbli`; jika judul kosong, akan masuk kategori “Tidak diisi”. Bila ingin daftar KBLI unik yang berbeda dari hitungan proyek, hitungan ini sudah disediakan secara terpisah.

## E. Batasan dan keamanan
- Ini adalah dashboard publik baca-saja; data diubah langsung di spreadsheet.
- Jika spreadsheet berisi data sensitif, jangan deploy API dengan akses publik. Batasi akses atau gunakan arsitektur autentikasi.
- API mengirim record ke browser untuk mendukung tabel pencarian; pertimbangkan endpoint ringkasan/paginasi jika dataset besar atau memuat informasi yang tidak boleh tampil publik.
- Setiap perubahan kode Apps Script setelah deployment perlu memperbarui deployment (Manage deployments > Edit > New version).
- Total investasi dan TKI adalah penjumlahan nilai baris. Pastikan tidak ada duplikasi dan definisi datanya sudah sesuai sebelum digunakan untuk pelaporan resmi.


PEMBARUAN LANJUTAN
- Label nilai pada grafik ditampilkan langsung tanpa hover (memerlukan koneksi CDN Chart.js dan plugin datalabels).
- KPI Usaha Unik menghitung NIB berbeda yang terisi, bukan nama perusahaan.
- Grafik baru: nilai investasi berdasarkan tahun pengajuan 2018–2027.
- Tabel Daftar Usaha merangkum KBLI/judul usaha, jumlah proyek, investasi, dan kecamatan. Kategori usaha seperti akomodasi, klinik, penyedia makanan, rumah sakit, apotek, dan lainnya diklasifikasikan berdasarkan kata kunci judul KBLI/nama proyek/sektor. Kategori ini untuk eksplorasi, bukan pengganti klasifikasi KBLI resmi.
- Pastikan CONFIG.API_URL di index.html tetap memakai URL Apps Script /exec yang sudah berjalan.
