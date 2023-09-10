# Analisa-Data-E-Commerce-Olist
Data Wrangling &amp; SQL Project Pacmann
## 1.	Objektif Analisis
Pada project Analisa Data E-Commerce ini dibatasi pada 3 objektif yaitu:
1. Mengetahui produk yang paling diminati pelanggan/paling banyak jumlah order nya.
2. Mengetahui total penjualan dari tiap kategori produk.
3. Mengetahui pertumbuhan pemesanan produk dari tiap 10 produk yang paling diminati.
## 2.	Data Overview
Pada Analisa data ini akan dataset olist, didalamnya  terdapat 9 tabel yaitu:  
![gbr 01 list tabel dataset](https://github.com/indra2878/Analisa-Data-E-Commerce-Olist/assets/129472057/b35ca799-2984-4758-a979-7acab0ae1e2e)   

untuk mengetahui isi masing-masing tabel maka dibuat dataframenya,dari dataframe itu bisa dilihat juga kolom-kolomnya masing-masing.

![gbr 02 list kolom tabel dataset](https://github.com/indra2878/Analisa-Data-E-Commerce-Olist/assets/129472057/1dfa0867-47f1-4cde-8d39-ba273cb98375)  

dengan langkah yang sama maka untuk setiap tabel maka didapat data kolom masing-masing tabel sebagai berikut:  
  
1)	Tabel olist_order_customers_dataset 
	Tabel ini berisi informasi mengenai customer (pelanggan), berisi 7 kolom terdiri dari: 
	['index', 'customer_id', 'customer_unique_id', 'customer_zip_code_prefix', 'customer_city', 'customer_state']
2)	Tabel olist_order_dataset  
   Tabel ini berisi informasi mengenai order, berisi 9 kolom terdiri dari:
	['index', 'order_id', 'customer_id', 'order_status', 'order_purchase_timestamp', 'order_approved_at', 'order_delivered_carrier_date', 'order_delivered_customer_date', 'order_estimated_delivery_date']
3)	Tabel olist_order_reviews_dataset  
   Tabel ini berisi informasi mengenai order review, berisi 8 kolom terdiri dari:
['index', 'review_id', 'order_id', 'review_score', 'review_comment_title', 'review_comment_message', 'review_creation_date', 'review_answer_timestamp']
4)	Tabel olist_order_payments_dataset  
   Tabel ini berisi informasi mengenai payment, berisi 6 kolom terdiri dari:
['index', 'order_id', 'payment_sequential', 'payment_type', 'payment_installments', 'payment_value']
5)	Tabel olist_order_items_dataset
   Tabel ini berisi informasi mengenai order items, berisi 8 kolom terdiri dari:
['index', 'order_id', 'order_item_id', 'product_id', 'seller_id', 'shipping_limit_date', 'price', 'freight_value']
6)	Tabel olist_products_dataset
   Tabel ini berisi informasi mengenai product, berisi 10 kolom terdiri dari:
['index', 'product_id', 'product_category_name', 'product_name_lenght', 'product_description_lenght', 'product_photos_qty', 'product_weight_g', 'product_length_cm', 'product_height_cm', 'product_width_cm']
7)	Tabel olist_sellers_dataset
    Tabel ini berisi informasi mengenai seller, berisi 5 kolom terdiri dari:
['index', 'seller_id', 'seller_zip_code_prefix', 'seller_city', 'seller_state']
8)	Tabel olist_geolocation_dataset
   Tabel ini berisi informasi mengenai geolocation, berisi 5 kolom terdiri dari:
['index', 'geolocation_zip_code_prefix', 'geolocation_lat', 'geolocation_lng', 'geolocation_city', 'geolocation_state']
9)	Tabel product_category_name_translation
 Tabel ini berisi informasi mengenai tranlasi category name ke english, berisi 3 kolom terdiri dari: ['index', 'product_category_name', 'product_category_name_english']  

Adapun Diagram Hubungan Entiti (ERD) dari Dataset olist.db dapat dilihat pada gambar dibawah:
![juknis project 04](https://github.com/indra2878/Analisa-Data-E-Commerce-Olist/assets/129472057/f6d04155-2eb3-4ad5-805c-d457b4c4d613)  

Dari isi table dan gambaran ERD diatas dapat dilihat bahwa :  
1)	Tabel (entitas) olist_order_dataset terhadap tabel: olist_order_payments_dataset, olist_order_reviews_dataset, olist_order_customer_dataset dan olist_order_items_dataset, berhubungan langsung melalui kolom (atribut) order_id (5 tabel tersebut sama-sama mempunyai kolom order_id).
2)	Tabel olist_order_items_dataset mempunyai hubungan langsung dengan 
tabel olist_products_dataset melalui kolom product_id, serta berhubungan langsung dengan 
tabel olist_sellers_dataset melalui kolom seller_id.
3)	selain dengan tabel olist_order_items_dataset, Tabel olist_sellers_dataset mempunyai hubungan langsung dengan tabel olist_geolocation_dataset dan olist_order_customer_dataset  melalui kolom zip_code_prefix serta berhubungan tidak langsung dengan olist_products_dataset melalui kolom product_id.
4)	selain itu dapat dilihat juga tabel olist_order_dataset mempunyai hubungan tidak langsung dengan 3 tabel yaitu: tabel olist_products_dataset  (melaui kolom product_id), tabel olist_sellers_dataset (melalui kolom seller_id)  dan abel olist_geolocation_dataset melalui kolom zip_code_prefix.
   
Untuk menjawab objektif analisa data, diperlukan beberapa entitas dan atribut sebagai berikut:  
1.	Entitas olist_order_dataset,  dengan atribut: ‘order id’ dan  'order_purchase_timestamp’.
2.	Entitas olist_products_dataset   dengan atribut: 'product_id' dan 'product_category_name'
3.	Entitas terjemahan nama kategori ke english, dengan atribut : 'product_category_name' dan 'product_category_name_english'
4.	Karena entitas order dan produk berhubungan secara tidak langsung maka dibutuhkan entitas 
olist_order_items_datase, dengan atribut: 'order_id', 'product_id', 'price'.

Sebelum melakukan analisa data untuk menjawab objektif harus melalui tahapan eksplorasi/ pengecekkan dan pemrosesan data terlebih dahulu. 
sehingga untuk memudahkan proses ekplorasi dan pemrosesan data, entitas entitas akan digabungkan menjadi entitas baru dengan nama olist_analisa_dataset.  

## 3.	Ekplorasi dan Pemrosesan Data
Proses eksplorasi data yang dilakukan meliputi: Identifikasi missing value, duplicate data, outlier, dan inconsistent format.
(lihat file jupiter notebook untuk melihat proses pengolahan data dan hasilny secara lengkap). 

**3.1	Identifikasi missing value**
Berdasarkan info dataset jumlah non-null value sama dengan jumlah data,  serta visualisasi heatmap tidak menunjukkan adanya missing value maka dipastikan tidak ada null value pada olist_dataset_use.  

**3.2	identfikasi duplicate data**
Dari hasil pemeriksaan duplikasi data terlihat ada 10080 baris data yang sama dengan baris data sebelumnya. Data duplikat ini selanjutnya akan ditangani dengan cara dihilangkan menggunakan fungsi drop_duplicates().

**3.3	 Identifikasi outlier**
Dari histogram terlihat grafiknya distibusinya skew (mengekor) ke kanan dan cukup jauh dari puncaknya, menandakan adanya outlier. Untuk menanganinya digunakan metode IQR.  
Dari histogram hasil penanganan outlier terlihat grafiknya distibusinya sudah mendekati normal, menandakan outlier sudah ditangani dengan baik.  

**3.4.	 Identifikasi inconsistent format**
Dari info dataset dan tampilan isi tabel, kolom order_purchase_timestamp bertipe datetime namun masih tertulis object.
Untuk menanganinya dengan mengubah type datanya dengan fungsi to_datetime() pandas.  
Dari info diatas type data order_purchase_timestamp sudah berubah menjadi datetime64[ns], maka inconsistent format sudah tertangani. Untuk selanjutnya dataset olist_dataset_use4 bisa digunakan untuk menjawab objektif.  

## 4.	Analisa Data
1. Mengetahui produk yang paling diminati pelanggan/paling banyak jumlah order-nya.
2. Mengetahui total omzet penjualan dari tiap kategori produk.
3. Mengetahui pertumbuhan pemesanan produk dari tiap 10 produk yang paling diminati.

## 5. Kesimpulan
5.	Kesimpulan
Dari hasil analisa data dapat diambil beberapa kesimpulan utama sebagai berikut
1.	Ada 10 kategori produk yang paling diminati pelanggan yaitu: bed_bath_table, health_beauty, sports_leisure, computers_accessories, furniture_decor, housewares, watches_gifts, telephony, toys dan auto.

2.	10 urutan kategori produk dengan oomzet terbesar yaitu: bed_bath_table, sports_leisure, health_beauty, computers_accessories, watches_gifts, furniture_decor, housewares,  cool_stuff, auto dan  toys.

3.	Dari kedua urutan diatas bed_bath_table adalah kategori produk yang paling diminati dan juga penyumbang omzet terbesar di olist e-commerce ini sehingga disarankan kepada pengelola e-commerce ini agar lebih mengembangkan produk kategori bed_bath_table, bisa dari segi variasi produknya, service after salesnya, promo-promo dan sebagainya.

## 6.	Link easy report

https://medium.com/@i.wahyudin2878/analisa-data-e-commerce-olist-7bb135d227ba

