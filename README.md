# Project Data Analysis for Retail: Sales Performance Report
## Dataset Brief
Dataset yang digunakan berisi transaksi dari tahun 2009 sampai dengan tahun 2012 dengan jumlah raw data sebanyak 5500, termasuk di dalamnya order status yang terbagi menjadi order finished, order returned dan order cancelled
Nama tabel yang akan digunakan pada project ini adalah dqlab_sales_store
Adapun dataset yang sudah diberikan dan akan digunakan pada project ini berisi data sebagai berikut.

- OrderID
- Order Status
- Customer
- Order Date
- Order Quantity
- Sales
- Discount %
- Discount
- Product Category
- Product Sub-Category
## Petunjuk Penyelesaian Project
Untuk menyelesaikan project, maka kita akan mengetikkan code yang perlu disubmit untuk dicek jawabannya benar atau salah.
Dari data yang sudah diberikan, dari pihak manajemen DQLab store ingin mengetahui:
- 1A. Overall perofrmance DQLab Store dari tahun 2009 - 2012 untuk jumlah order dan total sales order finished
- 1B. Overall performance DQLab by subcategory product yang akan dibandingkan antara tahun 2011 dan tahun 2012
- 2A. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan tahun
- 2B. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan sub-category

Setelah melihat hasil analisa di Sub Bab 1 dan 2, selanjutnya dilakukan analisa terhadap customer DQLab. Analisa dari sisi customer dengan menggunakan metrics:
- 3A. Analisa terhadap customer setiap tahunnya
- 3B. Analisa terhadap jumlah customer baru setiap tahunnya
- 3C. Cohort untuk mengetahui angka retention customer tahun 2009

## Overall Performance by Year
Buatlah Query dengan menggunakan SQL untuk mendapatkan total penjualan (sales) dan jumlah order (number_of_order) dari tahun 2009 sampai 2012 (years). 

SELECT year(order_date) AS years, SUM(sales) AS sales, COUNT(order_id) AS number_of_order

FROM dqlab_sales_store

WHERE order_status = 'order finished'

GROUP BY years

ORDER BY years ASC;

## Overall Performance by Product Sub Category
Buatlah Query dengan menggunakan SQL untuk mendapatkan total penjualan (sales) berdasarkan sub category dari produk (product_sub_category) pada tahun 2011 dan 2012 saja (years) 

SELECT year(order_date) AS years, product_sub_category, SUM(sales) AS sales

FROM dqlab_sales_store

WHERE order_status = 'order finished' AND year(order_date) in ('2011', '2012')

GROUP BY years, product_sub_category

ORDER BY years, sales DESC;

## Promotion Effectiveness and Efficiency by Years
Buatkan Derived Tables untuk menghitung total sales (sales) dan total discount (promotion_value) berdasarkan tahun(years) dan formulasikan persentase burn rate nya (burn_rate_percentage).
SELECT 

year(order_date) as years,

sum(sales) as sales,

sum(discount_value) as promotion_value,

round(sum(discount_value)/sum(sales)*100,2) as burn_rate_percentage

FROM dqlab_sales_store

WHERE order_status = 'order finished'

GROUP BY years

ORDER BY years ASC;

## Promotion Effectiveness and Efficiency by Product Sub Category
SELECT 

year(order_date) as years,

product_sub_category,

product_category,

sum(sales) as sales,

sum(discount_value) as promotion_value,

round(sum(discount_value)/sum(sales)*100,2) as burn_rate_percentage

FROM dqlab_sales_store

WHERE year(order_date) = '2012' AND order_status = 'order finished'

GROUP BY year(order_date),product_sub_category,product_category

ORDER BY sales DESC;

## Customers Transactions per Year
SELECT 

year(order_date) as years,

COUNT(DISTINCT customer) as number_of_customer

FROM dqlab_sales_store

WHERE order_status = 'order finished'

GROUP BY years

ORDER BY years ASC;

