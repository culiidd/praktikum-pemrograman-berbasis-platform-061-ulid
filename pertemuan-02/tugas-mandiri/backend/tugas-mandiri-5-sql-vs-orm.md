# Tugas Mandiri 5 — Membandingkan SQL Mentah dan ORM

**Nama:** Ulid 
**NIM:** 061
**Pertemuan:** 02  
**Topik:** Perbandingan SQL Mentah dan ORM

## Operasi Database yang Dipilih

Pada tugas ini, operasi database yang dipilih adalah **mengambil satu data jadwal berdasarkan ID**. Tabel yang digunakan sebagai contoh adalah tabel `jadwal`.

Operasi ini bertujuan mencari satu data tertentu berdasarkan nilai `id` yang diberikan oleh pengguna atau aplikasi.

---

## A. SQL Mentah

Pada pendekatan SQL mentah, perintah SQL ditulis secara langsung untuk berkomunikasi dengan database.

Contoh query:

```sql
SELECT *
FROM jadwal
WHERE id = ?;
```

Tanda `?` merupakan parameter yang nilainya akan diberikan secara terpisah ketika query dijalankan.

Apabila menggunakan Node.js dengan library `mysql2`, contoh implementasinya dapat ditulis sebagai berikut:

```javascript
const [rows] = await db.execute(
  'SELECT * FROM jadwal WHERE id = ?',
  [1]
);

console.log(rows);
```

Pada contoh tersebut, nilai `1` akan dimasukkan sebagai parameter untuk menggantikan tanda `?`. Penggunaan parameter lebih aman dibandingkan menggabungkan nilai input pengguna secara langsung ke dalam string SQL.

---

## B. ORM

Operasi yang sama dapat dilakukan menggunakan Object Relational Mapping (ORM), misalnya Prisma.

Contoh:

```javascript
const jadwal = await prisma.jadwal.findUnique({
  where: {
    id: 1
  }
});

console.log(jadwal);
```

Pada pendekatan ORM, programmer tidak perlu menuliskan perintah SQL secara langsung. Prisma menerjemahkan perintah yang ditulis menggunakan objek JavaScript menjadi query yang sesuai dengan database.

---

## C. Perbandingan SQL Mentah dan ORM

| Aspek | SQL Mentah | ORM |
|---|---|---|
| Cara akses database | Menulis query SQL secara langsung | Menggunakan method dan object pada bahasa pemrograman |
| Tingkat kontrol | Sangat tinggi | Sebagian dikelola oleh ORM |
| Kemudahan penulisan | Membutuhkan pemahaman SQL | Lebih mudah dibaca oleh programmer aplikasi |
| Query kompleks | Lebih fleksibel | Bergantung pada kemampuan ORM |
| Keamanan | Harus mengelola parameter dengan benar | Umumnya menyediakan mekanisme query yang lebih aman |
| Pemeliharaan kode | Dapat lebih sulit pada aplikasi besar | Lebih terstruktur dan mudah dipelihara |

---

## D. Jawaban Pertanyaan

### 1. Apa perbedaan SQL mentah dan ORM?

SQL mentah merupakan pendekatan akses database dengan menuliskan perintah SQL secara langsung, seperti `SELECT`, `INSERT`, `UPDATE`, dan `DELETE`. Programmer memiliki kontrol yang lebih besar terhadap query yang dijalankan.

ORM merupakan pendekatan yang menghubungkan tabel pada database dengan objek atau model dalam bahasa pemrograman. Programmer dapat melakukan operasi database melalui method seperti `findUnique()`, `findMany()`, `create()`, `update()`, atau `delete()` tanpa harus menulis seluruh query SQL secara manual.

### 2. Apa kelebihan SQL mentah?

Kelebihan SQL mentah adalah programmer memperoleh kontrol yang lebih besar terhadap query database. SQL mentah sangat berguna ketika membutuhkan query yang kompleks, optimasi tertentu, penggunaan join yang rumit, agregasi, atau fitur khusus yang tersedia pada database.

Selain itu, programmer dapat mengetahui secara langsung query apa yang dijalankan sehingga proses analisis dan optimasi performa database dapat dilakukan dengan lebih terperinci.

### 3. Apa kelebihan ORM?

ORM membuat kode akses database menjadi lebih mudah dibaca, terstruktur, dan dipelihara. Programmer dapat bekerja menggunakan model dan objek yang lebih dekat dengan bahasa pemrograman yang digunakan.

ORM juga mengurangi penulisan query SQL yang berulang. Pada aplikasi yang memiliki banyak tabel dan operasi CRUD, ORM dapat meningkatkan produktivitas programmer karena banyak proses akses database dapat dilakukan menggunakan method yang telah disediakan.

### 4. Apa risiko SQL Injection?

SQL injection merupakan serangan yang terjadi ketika input pengguna dimasukkan secara langsung ke dalam query SQL tanpa proses pengamanan yang tepat.

Sebagai contoh, penggunaan query seperti:

```javascript
const query = "SELECT * FROM jadwal WHERE id = " + req.params.id;
```

dapat berbahaya karena input pengguna menjadi bagian langsung dari perintah SQL. Penyerang dapat memasukkan karakter atau perintah SQL tertentu sehingga struktur query berubah dan database menjalankan perintah yang tidak diinginkan.

Dampaknya dapat berupa pembacaan data tanpa izin, perubahan data, penghapusan data, maupun akses yang seharusnya tidak diberikan.

### 5. Mengapa penggunaan parameter query dapat mengurangi risiko SQL Injection?

Parameter query memisahkan antara struktur perintah SQL dan nilai data yang diberikan oleh pengguna.

Contohnya:

```javascript
const [rows] = await db.execute(
  'SELECT * FROM jadwal WHERE id = ?',
  [id]
);
```

Pada contoh tersebut, `id` diperlakukan sebagai nilai parameter dan bukan sebagai bagian dari sintaks SQL. Dengan demikian, input pengguna tidak dapat dengan mudah mengubah struktur query yang telah ditentukan.

Penggunaan parameter query merupakan salah satu cara penting untuk mengurangi risiko SQL injection.

### 6. Bagaimana ORM membantu programmer dalam mengakses database?

ORM menyediakan lapisan abstraksi antara program dengan database. Programmer dapat mengakses database melalui model, object, dan method yang disediakan ORM tanpa harus menulis semua perintah SQL secara langsung.

Sebagai contoh, untuk mengambil jadwal berdasarkan ID menggunakan Prisma dapat digunakan:

```javascript
const jadwal = await prisma.jadwal.findUnique({
  where: {
    id: 1
  }
});
```

Kode tersebut lebih mudah dibaca karena menunjukkan secara langsung bahwa program ingin mencari satu data `jadwal` berdasarkan ID.

ORM juga membantu dalam pengelolaan model database, operasi CRUD, relasi antar tabel, migrasi schema, serta penggunaan parameter query secara lebih terstruktur.

---

## Kesimpulan

SQL mentah dan ORM merupakan dua pendekatan yang dapat digunakan untuk mengakses database. SQL mentah memberikan kontrol yang lebih tinggi terhadap query sehingga cocok digunakan ketika diperlukan query yang kompleks atau optimasi database secara khusus.

ORM memberikan abstraksi yang membuat proses pengembangan aplikasi menjadi lebih sederhana dan terstruktur. Pada operasi database yang umum, ORM dapat mengurangi jumlah kode SQL yang harus ditulis secara manual dan meningkatkan kemudahan pemeliharaan aplikasi.

Pemilihan antara SQL mentah dan ORM sebaiknya disesuaikan dengan kebutuhan aplikasi. ORM dapat digunakan untuk operasi umum dan pengembangan yang lebih cepat, sedangkan SQL mentah tetap berguna ketika programmer membutuhkan kontrol yang lebih detail terhadap query database.