#Tugas Pendahuluan Pertemuan 2
TUGAS PENDAHULUAN JSONPLACEHOLDER
Analisis Endpoint /posts dan /users serta HTTP Method
Nama : ulid
NIM   : 2024520061
Kelas : Pemrograman Berorientasi Objek 12.00

SOAL NOMOR 1
Bandingkan struktur data pada /posts/1 dan /users/1 di JSONPlaceholder. Jelaskan perbedaan field dan fungsinya.
A. Data Post/Artikel
•	userId = ID pengguna yang membuat post.
•	id = ID unik post.
•	title = judul post.
•	body = isi post.
B. Data User/Pengguna
•	id = ID unik pengguna.
•	name, username, email = identitas pengguna.
•	address = informasi alamat pengguna.
•	phone, website = informasi kontak pengguna.
•	company = informasi perusahaan pengguna.
Hubungan kedua data terdapat pada field userId di post yang merujuk ke id pada user:
post.userId = user.id

Contoh:
post.userId = 1
user.id     = 1
Artinya, post tersebut dibuat oleh user dengan ID 1, yaitu Leanne Graham. Jadi, data post menyimpan informasi konten yang dibuat pengguna, sedangkan data user menyimpan informasi lengkap tentang pengguna. Field userId pada post berfungsi sebagai penghubung ke id pada data user.
SOAL NOMOR 2
Analisis struktur tabel yang ada pada /posts dan /users di JSONPlaceholder. Buat diagram relasi sederhana yang menjelaskan hubungan antar tabel.
A. Struktur Data /posts
Data pada /posts memiliki empat field utama yang menggambarkan sebuah post dan pemiliknya.
Field	Fungsi
id	ID unik setiap post.
userId	ID pengguna yang membuat post.
title	Judul post.
body	Isi atau konten post.

B. Struktur Data /users
Data pada /users memiliki struktur yang lebih kompleks karena menyimpan informasi identitas, alamat, kontak, dan perusahaan pengguna.
Field	Fungsi
id	ID unik pengguna.
name	Nama lengkap pengguna.
username	Username pengguna.
email	Email pengguna.
address	Informasi alamat pengguna, termasuk street, suite, city, zipcode, dan geo.
phone	Nomor telepon pengguna.
website	Website pengguna.
company	Informasi perusahaan pengguna, termasuk name, catchPhrase, dan bs.

C. Diagram Relasi Sederhana
USERS
+----------------+
| id        | PK |
| name           |
| username       |
| email          |
| address        |
| phone          |
| website        |
| company        |
+----------------+
        |
        | 1
        |
        | memiliki
        |
        | N
        v
POSTS
+----------------+
| id        | PK |
| userId    | FK |
| title          |
| body           |
+----------------+

USERS.id  1 -------- N  POSTS.userId
Relasi tersebut bersifat one-to-many (1:N). Satu user dapat memiliki banyak post, sedangkan setiap post terhubung ke satu user melalui field userId.
SOAL NOMOR 3
Analisis hubungan antara URL, method, dan data yang dikembalikan pada contoh /posts di JSONPlaceholder.
Dalam REST API, URL menunjukkan resource yang ingin diakses, sedangkan HTTP method menentukan tindakan yang dilakukan terhadap resource tersebut. Karena itu, kombinasi URL dan method menentukan bentuk data atau respons yang dikembalikan.
URL	Method	Fungsi	Data yang Dikembalikan
/posts	GET	Mengambil seluruh post	Array berisi banyak post.
/posts/1	GET	Mengambil satu post tertentu	Satu objek post dengan id = 1.
/posts	POST	Membuat post baru	Objek post baru.
/posts/1	PUT	Mengganti seluruh data post	Objek post yang telah diperbarui.
/posts/1	PATCH	Mengubah sebagian data post	Objek post dengan field tertentu yang diperbarui.
/posts/1	DELETE	Menghapus post	Objek kosong {}.

Jadi, URL menentukan resource atau data yang dituju, sedangkan method menentukan operasi yang dilakukan terhadap resource tersebut. Contohnya, GET /posts berarti mengambil seluruh post, sedangkan DELETE /posts/1 berarti meminta penghapusan post dengan ID 1.
SOAL NOMOR 4
Bedakan penggunaan /posts/1 dan ?userId=1 berdasarkan hasil yang dikembalikan JSONPlaceholder.
A. /posts/1
Endpoint GET /posts/1 digunakan untuk mengambil satu post berdasarkan ID post. Sistem mencari post dengan id = 1 dan hasilnya berupa satu objek JSON.
GET /posts/1

{
  "userId": 1,
  "id": 1,
  "title": "...",
  "body": "..."
}
B. /posts?userId=1
Endpoint GET /posts?userId=1 digunakan untuk memfilter post berdasarkan userId. Hasilnya berupa array yang berisi semua post milik user dengan ID 1.
GET /posts?userId=1

[
  { "userId": 1, "id": 1, "title": "...", "body": "..." },
  { "userId": 1, "id": 2, "title": "...", "body": "..." }
]
Endpoint	Pencarian Berdasarkan	Hasil
/posts/1	id post	Satu objek post.
/posts?userId=1	userId	Banyak post dalam bentuk array.

Kesimpulannya, /posts/1 digunakan untuk mengambil satu post tertentu berdasarkan id, sedangkan /posts?userId=1 digunakan untuk mencari semua post yang dimiliki oleh user tertentu.
SOAL NOMOR 5
Bandingkan hasil GET, POST, PUT, PATCH, dan DELETE pada JSONPlaceholder. Kaitkan setiap method dengan status code dan perubahan datanya.
Method	Fungsi	Contoh	Status Code	Perubahan Data
GET	Mengambil data	GET /posts/1	200 OK	Tidak mengubah data.
POST	Membuat data baru	POST /posts	201 Created	Membuat data baru secara simulasi.
PUT	Mengganti seluruh data	PUT /posts/1	200 OK	Seluruh resource dianggap diperbarui.
PATCH	Mengubah sebagian data	PATCH /posts/1	200 OK	Hanya field tertentu yang diperbarui.
DELETE	Menghapus data	DELETE /posts/1	200 OK	Data dianggap dihapus.

GET
GET digunakan untuk membaca atau mengambil data. Contoh GET /posts/1 biasanya menghasilkan status 200 OK. Method ini tidak mengubah data yang ada.
POST
POST digunakan untuk membuat resource baru. Pada JSONPlaceholder, POST /posts akan mengembalikan data yang dikirim beserta id baru secara simulasi, umumnya dengan status 201 Created.
POST /posts

{
  "title": "Belajar API",
  "body": "Praktikum JSONPlaceholder",
  "userId": 1
}
Respons contoh:
{
  "title": "Belajar API",
  "body": "Praktikum JSONPlaceholder",
  "userId": 1,
  "id": 101
}
PUT
PUT digunakan untuk mengganti atau memperbarui keseluruhan resource. Contoh PUT /posts/1 mengirimkan representasi lengkap data post dan biasanya menghasilkan status 200 OK.
PUT /posts/1

{
  "id": 1,
  "title": "Judul Baru",
  "body": "Isi baru",
  "userId": 1
}
PATCH
PATCH digunakan untuk memperbarui sebagian field saja. Jika hanya title yang dikirim, maka field lain dianggap tetap. JSONPlaceholder biasanya memberikan status 200 OK.
PATCH /posts/1

{
  "title": "Judul Diubah"
}
Perbedaan utama: PUT digunakan untuk mengganti keseluruhan representasi resource, sedangkan PATCH digunakan untuk mengubah bagian tertentu dari resource.
DELETE
DELETE digunakan untuk meminta penghapusan resource. Contoh DELETE /posts/1 biasanya menghasilkan status 200 OK dan respons objek kosong {} pada JSONPlaceholder.
Kesimpulan
URL menentukan resource yang dituju, sedangkan HTTP method menentukan operasi terhadap resource tersebut. GET digunakan untuk membaca data, POST untuk menambah data, PUT untuk mengganti keseluruhan data, PATCH untuk memperbarui sebagian data, dan DELETE untuk menghapus data. Namun, JSONPlaceholder merupakan fake REST API sehingga perubahan dari POST, PUT, PATCH, dan DELETE hanya disimulasikan dalam respons dan tidak disimpan secara permanen di server.
