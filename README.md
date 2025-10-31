# Insight-IPSD-Kelompok-4

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk menampilkan dan menganalisis insight dari database CoffeeShop_Dataset.db dengan memanfaatkan pustaka Pandas pada Python.
Notebook ini berisi serangkaian program eksplorasi data, mulai dari pembacaan database, penggabungan tabel, pengelompokan (grouping), hingga analisis transaksi.
Seluruh proses dilakukan untuk memperlihatkan bagaimana Pandas dapat digunakan sebagai alat bantu dalam data processing dan data analytics secara efisien.

## 2. Tujuan

Mengimplementasikan penggunaan Pandas dalam membaca dan mengolah data dari database SQLite.

Menunjukkan tahapan pengolahan data mulai dari penggabungan tabel, agregasi, dan transformasi data.

Menyediakan insight awal terhadap data penjualan kopi berdasarkan outlet, transaksi, dan kategori.

## 3. Manfaat

Memberikan pemahaman tentang penggunaan Pandas untuk mengelola data relasional tanpa harus berpindah ke SQL langsung.

Menjadi referensi praktikum untuk memahami konsep data wrangling dan data aggregation di Python.

Menunjukkan efisiensi Pandas dalam menganalisis dataset berukuran menengah seperti CoffeeShop_Dataset.db.

## 4. Alur Kerja (Workflow)

Membaca database CoffeeShop menggunakan sqlite3 dan menampilkan tabel yang tersedia.

Mengimpor data dari tabel utama ke dalam DataFrame Pandas.

Melakukan eksplorasi awal dengan fungsi head(), info(), dan describe().

Melakukan penggabungan data antar tabel untuk memperkaya informasi (misalnya antara sales_report dan sales_outlet).

Melakukan analisis statistik sederhana melalui groupby() dan agg().

Menampilkan hasil akhir dalam bentuk DataFrame insight seperti total transaksi per outlet.

##Program 1 — Analisis Penjualan per Kategori Produk
```python
merged = sr.merge(p[["product_id","product_category"]], on="product_id", how="left")
category = merged.groupby("product_category")["line_item_amount"].sum().reset_index()
category
```

### Tujuan

Mengetahui total nilai penjualan berdasarkan kategori produk.

### Manfaat

* Mengidentifikasi kategori dengan penjualan tertinggi/rendah.

* Mendukung strategi stok dan promosi.

### Langkah

* Gabungkan data penjualan (sr) dan produk (p) lewat product_id.

* Kelompokkan berdasarkan product_category.

* Jumlahkan kolom line_item_amount.

* Tampilkan hasil total penjualan tiap kategori.

## Program 2 — Menampilkan 10 Produk dengan Penjualan Tertinggi

```python
top_p = (
    merged.groupby("product_id")["line_item_amount"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
    .reset_index()
)
top_p
```

### Tujuan

Menampilkan 10 produk dengan total nilai penjualan tertinggi.

### Manfaat

* Mengetahui produk paling laku dan berkontribusi besar terhadap pendapatan.

* Menjadi acuan strategi stok dan pemasaran.

### Langkah

* Kelompokkan data berdasarkan product_id.

* Jumlahkan nilai transaksi pada line_item_amount.

* Urutkan hasil secara menurun (descending).

* Ambil 10 produk teratas dengan head(10).

## Program 3 — Menghitung Total Nilai Penjualan

```python
total = merged["line_item_amount"].sum()
float(total)
```
### Tujuan

Menghitung total keseluruhan nilai transaksi penjualan.

### Manfaat

* Memberikan gambaran total pendapatan dari seluruh transaksi.

* Menjadi dasar perbandingan dengan analisis lain seperti kategori atau outlet.

### Langkah

* Akses kolom line_item_amount dari DataFrame merged.

* Gunakan .sum() untuk menjumlahkan seluruh nilai penjualan.

* Ubah hasilnya ke tipe float agar mudah dibaca dan diolah lebih lanjut.

## Program 4 — Analisis Jumlah Transaksi per Outlet

```python
merged = pd.merge(sr, so, on='sales_outlet_id')
nt = merged.groupby('sales_outlet_id').agg(nilai_transaksi=('transaction_id', 'nunique')).reset_index()
display(nt)
```

### Tujuan
Menghitung jumlah transaksi unik pada setiap outlet penjualan untuk mengetahui tingkat aktivitas masing-masing outlet.

### Manfaat

* Menilai performa outlet berdasarkan frekuensi transaksi.

* Membantu manajemen mengidentifikasi outlet paling aktif/pasif.

* Menjadi dasar strategi promosi dan evaluasi kinerja outlet.

### Langkah

* Gabungkan data sr dan so dengan pd.merge() menggunakan sales_outlet_id.

* Kelompokkan berdasarkan sales_outlet_id dengan groupby().

* Hitung jumlah transaksi unik menggunakan .nunique().

* Simpan hasil ke DataFrame baru (nt) dan tampilkan dengan display().

## Program 5 — Menghitung Jumlah Produk dan Pelanggan Unik

```python
n_product = merged["product_id"].nunique()
n_customer = merged["customer_id"].nunique()
n_product, n_customer
```

### Tujuan
Mengetahui jumlah produk dan pelanggan unik yang tercatat dalam dataset penjualan.

### Manfaat

* Memberikan gambaran keragaman produk yang dijual dan pelanggan yang dilayani.

* Menjadi indikator jangkauan pasar dan variasi portofolio produk.

### Langkah

* Gunakan .nunique() pada kolom product_id untuk menghitung jumlah produk unik.

* Gunakan .nunique() pada kolom customer_id untuk menghitung jumlah pelanggan unik.

* Tampilkan kedua hasil tersebut (n_product, n_customer).
