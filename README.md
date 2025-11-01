# Insight-IPSD-Kelompok-4

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk menampilkan dan menganalisis insight dari database CoffeeShop_Dataset.db dengan memanfaatkan pustaka Pandas pada Python.
Tujuan kita kali ini adalah menentukan penjualan produk tertinggi hingga terendah dan mencari outlet dengan transaksi terbanyak hingga tersedikt untuk menganalisis dalam pembuatan event diskon atau promo dalam 2 kategori yaitu :
1. Memberikan promo untuk produk dengan penjualan terendah pada outlet dengan jumlah transaksi terbanyak
2. Memberikan promo untuk produk dengan penjualan tertinggi pada outlet dengan jumlah transaksi tersedikit

## 2. Tujuan dan Manfaat

#### 1. Memberikan promo untuk produk dengan penjualan terendah pada outlet dengan jumlah transaksi terbanyak

Menjual produk yang kurang terjual pada outlet yang ramai akan memberikan peluang penjualan pada produk yang kurang terjual, karena kebanyakan orang sering mengambil promo dari produk yang ditawarkan biarpun itu bukan produk favorit pelanggan apalagi promo tersebut dilakukan di outlet yang ramai. Ini akan memberikan peluang untuk mendapat keuntungan.

#### 2. Memberikan promo untuk produk dengan penjualan tertinggi pada outlet dengan jumlah transaksi tersedikit

Menjual produk yang banyak terjual pada outlet yang sepi akan memberikan peluang pada kunjungan pelanggan ke outlet, karena produk dengan penjualan tinggi tentunya memiliki banyak peminat, ini akan menarik pelanggan untuk melakukan transaksi di outlet tersebut. 

## 3. Alur Kerja (Workflow)

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

## Program 6 - Statistik Penjualan per Produk
```python
stats = (
    merged.groupby(['product_id']).agg(
        total_sales=('line_item_amount', 'sum'),
        avg_sales=('line_item_amount', 'mean'),
        total_qty=('quantity', 'sum'),
        num_transactions=('transaction_id', 'nunique')
    ).reset_index()
)
stats = stats.sort_values(by='total_sales', ascending=False)
stats.head()
```


### Tujuan
Menganalisis performa penjualan tiap produk berdasarkan total penjualan, rata-rata transaksi, jumlah barang terjual, dan banyaknya transaksi.

### Manfaat

* Mengetahui produk dengan penjualan tertinggi dan performa terbaik.

* Menjadi dasar pengambilan keputusan stok, harga, atau promosi produk.

* Memberikan insight mendalam mengenai kontribusi tiap produk terhadap total pendapatan.

### Langkah

Kelompokkan data berdasarkan product_id menggunakan groupby().

Hitung total, rata-rata penjualan, total kuantitas, dan jumlah transaksi unik.

Urutkan hasil berdasarkan total_sales dari tertinggi ke terendah.

Tampilkan 5 produk teratas dengan head().

## Program 7 - Analisis Kontribusi Penjualan Tiap Produk terhadap Kategorinya

```python
category_summary = (
    merged.groupby("product_category")["line_item_amount"]
    .sum()
    .reset_index()
    .rename(columns={'line_item_amount': 'total_sales', 'product_category': 'category'})
)

category_summary = category_summary.rename(columns={'total_sales': 'total_sales_category'})
merged_stats = merged.merge(
    category_summary[['category', 'total_sales_category']],
    left_on='product_category',
    right_on='category',
    how='left'
)
merged_stats['kontribusi%'] = (
    merged_stats['line_item_amount'] / merged_stats['total_sales_category']
) * 100
merged_stats.head()
```

### Tujuan

Mengukur kontribusi tiap produk terhadap total penjualan kategori produknya masing-masing.

### Manfaat

* Mengetahui produk mana yang paling berpengaruh dalam kategori tertentu.

* Membantu menentukan fokus promosi pada produk dengan kontribusi tinggi.

* Memberikan insight proporsi penjualan tiap produk dalam satu kategori.

### Langkah:

* Hitung total penjualan per kategori dengan groupby() dan sum().

* Ganti nama kolom agar lebih deskriptif (total_sales_category).

* Gabungkan hasil tersebut ke dataset utama (merged).

* Hitung persentase kontribusi tiap produk dengan membagi nilai produk terhadap total kategori.

* Tampilkan hasil dalam kolom baru kontribusi%.

## Program 8- outlet dengan penjualan terendah

```python
rekap = (
    sr.groupby("sales_outlet_id")
    .agg(
        penjualan=("quantity", "sum"),
        transaksi=("transaction_id", "count")
    )
    .reset_index()
)
rekap
```

### Tujuan

Membuat rekapitulasi jumlah barang terjual dan total transaksi untuk setiap outlet penjualan.

### Manfaat

* Mengetahui performa setiap outlet dalam hal jumlah transaksi dan volume penjualan.

* Menjadi dasar evaluasi cabang yang paling produktif.

### Langkah

* Kelompokkan data berdasarkan sales_outlet_id.

* Hitung total quantity (barang terjual) dan jumlah transaction_id (transaksi).

* Kembalikan hasil ke bentuk tabel dengan reset_index().

## Program 9 - Produk dengan penjualan terendah

```python
rekap_product = (
    sr.groupby('product_id')
    .agg(
        total_penjualan=('quantity', 'sum'),
        total_transaksi=('transaction_id', 'count')
    )
    .reset_index()
    .sort_values(by='total_penjualan')
)
rekap_product.head(10)
```

### Tujuan
Membuat rekap jumlah barang terjual dan banyaknya transaksi untuk setiap produk.

### Manfaat

* Mengidentifikasi produk dengan volume penjualan tinggi atau rendah.

* Menjadi dasar pengambilan keputusan pengelolaan stok dan promosi produk.

### Langkah

* Kelompokkan data berdasarkan product_id.

* Hitung total quantity (barang terjual) dan jumlah transaction_id (transaksi).

* Urutkan hasil berdasarkan total_penjualan.

* Tampilkan 10 produk dengan penjualan terendah menggunakan head(10).

## Program 10 - Ringkasan Total Penjualan per Outlet

```python
outlet_sales_summary = (
    merged.groupby('sales_outlet_id')['line_item_amount']
    .sum()
    .reset_index()
)
outlet_sales_summary = pd.merge(
    outlet_sales_summary,
    so[["sales_outlet_id", "store_city"]],
    on="sales_outlet_id",
    how="left"
)
outlet_sales_summary = outlet_sales_summary.sort_values("line_item_amount", ascending=False)
display(outlet_sales_summary.head())
```

### Tujuan
Menampilkan total nilai penjualan tiap outlet dan mengaitkannya dengan kota tempat outlet berada.

### Manfaat

* Mengetahui outlet dengan performa penjualan tertinggi.

* Memberikan gambaran persebaran kontribusi penjualan per kota.

* Mendukung analisis geografis dalam strategi penjualan.

### Langkah

* Hitung total penjualan per outlet dengan groupby() dan sum().

* Gabungkan hasil dengan tabel outlet (so) untuk mendapatkan nama kota.

* Urutkan hasil berdasarkan line_item_amount secara menurun.

* Tampilkan beberapa outlet dengan nilai penjualan tertinggi menggunakan head().

## 4. Kesimpulan

#### 1. Memberikan promo untuk produk dengan penjualan terendah pada outlet dengan jumlah transaksi terbanyak

Dari hasil analisis yang sudah dilakukan produk yang akan diberikan promo adalah produk dengan product_id : 19, 18, 12, 4, dan 14 pada outlet dengan sales_oulet_id : 8

#### 2. Memberikan promo untuk produk dengan penjualan tertinggi pada outlet dengan jumlah transaksi tersedikit

Dari hasil analisis yang sudah dilakukan produk yang akan diberikan promo adalah produk dengan product_id : 50, 59, 38, 54, dan 32 pada outlet dengan sales_outlet_id : 3
