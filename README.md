# Jarkom-Modul-2-A06-2021
Lapres Praktikum Jaringan Komputer 2021 - Modul 2
- Muh. Nur Fajrin Amiruddin (05111940000005)
- Rihan Farih Bunyamin (05111940000165)
- Lathifa Itqonina Mardiyati (05111940000176)





## Soal  dan Jawaban
### Soal 1
Luffy adalah seorang yang akan jadi Raja Bajak Laut. Demi membuat Luffy menjadi Raja Bajak Laut, Nami ingin membuat sebuah peta, bantu Nami untuk membuat peta berikut :

![image9](https://user-images.githubusercontent.com/61973814/139194256-c0640fcf-d5ad-448f-a38d-905adf9a82dd.png)

EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet
### Jawaban 1
Pertama, tambahkan node dan hubungkan masing-masing node hingga sesuai dengan gambar dan ganti hostname dari masing-masing node. Lalu setting network masing-masing node dengan fitur Edit network configuration. Kemudian jalankan perintah iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.204.0.0/16 pada router Foosha. Lalu ketikkan command echo nameserver 192.168.122.1 > /etc/resolv.conf pada masing-masing node ubuntu. Lalu lakukan pengecekkan dengan melakukan ping ke google.

### Soal 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku
### Jawaban 2
Pertama, buat file franky.e09.com pada folder /etc/bind/kaizoku di EniesLobby. Lalu atur nama domain dan IP yang menuju ke EniesLobby (192.204.2.2). 

Kemudian  pada file /etc/bind/named.conf.local di EniesLobby, buat zone franky.e09.com dengan type master dan file /etc/bind/kaizoku/franky.e09.com. 

Setelah itu ping www.franky.e09.com pada node LogueTown dan akan menampilkan tampilan sebagai berikut.

### Soal 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
### Jawaban 3
atur nama subdomain, CNAME dan IP yang menuju ke Skypie (192.204.2.4) pada file franky.e09.com di folder /etc/bind/kaizoku pada node EniesLobby

Lalu ping www.super.franky.e09.com pada node LogueTown 

### Soal 4
 Buat juga reverse domain untuk domain utama
### Jawaban 4

### Soal 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
### Jawaban 5

### Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
### Jawaban 6

### Soal 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Jawaban 7
pertama buka file /etc/bind/sunnygo/mecha.franky.e09.com pada Water7 dan buat subdomain general yang mengarah ke IP skypie dan alias www.general yang mengarah ke general.mecha.franky.e09.com.

### Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.
### Jawaban 8
isi disini

### Soal 9
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.
### Jawaban 9
Pertama Buka file /etc/apache2/sites-available/franky.e09.com.conf pada Skypie dan ganti Alias menjadi "\home".

lalu Buka lynx franky.e09.com/index.php/home dan lynx franky.e09.com/homedan dan keduanya akan menghasilkan sebagai berikut.

### Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com
### Jawaban 10
Pertama Buka file /etc/apache2/sites-available/super.franky.e09.com.conf pada Skypie dan ubah DocumentRoot menjadi /var/www/super.franky.yyy.com

lalu Buka lynx super.franky.e09.com dan dia akan bisa buka aset.

### Soal 11
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja
### Jawaban 11
Buka file /etc/apache2/sites-available/super.franky.e09.com.conf pada Skypie dan tambahkan pada /public "Options +Indexes"

Buka lynx super.franky.e09.com/public, maka akan muncul directory listing.

### Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /errors untuk mengganti error kode pada apache
### Jawaban 12
buka file /etc/apache2/sites-available/super.franky.e09.com.conf dan ubah ErrorDocument menjadi 404 /error/404.html

Buka lynx super.franky.e09.com/aowkwkwkwk maka akan muncul sebagai berikut.

### Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js
### Jawaban 13
isi disini

### Soal 14
 Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Jawaban 14
isi disini

### Soal 15
 Dengan authentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Jawaban 15
isi disini

### Soal 16
 Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com
### Jawaban 16
isi disini

### Soal 17
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!
### Jawaban 17
isi disini



## Kendala yang dialami
- Tingkat kesulitan soal jauh meningkat dari sebelumnya
- Kepadatan jadwal membuat koordinasi kelompok kurang baik
