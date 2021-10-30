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
Pertama, tambahkan node dan hubungkan masing-masing node hingga sesuai dengan gambar dan ganti hostname dari masing-masing node. Lalu setting network masing-masing node dengan fitur Edit network configuration. Kemudian jalankan perintah ``iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.2.0.0/16`` pada router Foosha. Lalu ketikkan command ``echo nameserver 192.168.122.1 > /etc/resolv.conf`` pada masing-masing node ubuntu. Lalu lakukan pengecekkan dengan melakukan ping ke google. sebagai berikut :  
![image](https://user-images.githubusercontent.com/55240758/139538822-8e619888-2a43-467b-ae71-627c3746e5f8.png)


### Soal 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses **franky.yyy.com** dengan alias **www.franky.yyy.com** pada folder kaizoku
### Jawaban 2
Pertama, buat file **franky.a06.com** pada folder ``/etc/bind/kaizoku`` di EniesLobby. Lalu atur nama domain dan IP yang menuju ke EniesLobby (10.2.2.2).     
  
![1 - konf franky di kaizoku](https://user-images.githubusercontent.com/55240758/139537827-7e66c1aa-8a1e-4d98-ae76-d161b89295ef.jpg)
  

Kemudian  pada file ``/etc/bind/named.conf.local`` di EniesLobby, buat zone **franky.a06.com** dengan type master dan file ``/etc/bind/kaizoku/franky.a06.com``.  
  
![1 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139537797-e978c7e3-3eb3-4015-9a9c-1c143b10924e.jpg)

Setelah itu ping **www.franky.a06.com**  atau **franky.a06.com** pada node LogueTown dan akan menampilkan tampilan sebagai berikut.    
  
![1 - ping franky loguetown](https://user-images.githubusercontent.com/55240758/139537867-233548c2-8ab9-4e29-ad28-d8d7207a4526.jpg)  

### Soal 3
Setelah itu buat subdomain **super.franky.yyy.com** dengan alias **www.super.franky.yyy.com** yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
### Jawaban 3
Pertama, melakukan pengaturan nama subdomain, CNAME dan IP yang menuju ke Skypie (10.2.2.4) pada file **franky.a06.com** di folder ``/etc/bind/kaizoku`` pada node EniesLobby  
  
![2 - konf super di kaizoku](https://user-images.githubusercontent.com/55240758/139537898-9ca23739-e9ed-4079-8915-fa431853bc7b.jpg)
  
Lalu ping **www.super.franky.a06.com** pada node LogueTown, sebagai berikut :  
  
![2 - ping super alabasta](https://user-images.githubusercontent.com/55240758/139537901-26919d84-9aff-42c9-9861-8c5e3e759c22.jpg)  
 
### Soal 4
Buat juga reverse domain untuk domain utama  
### Jawaban 4  
Pertama, tambahkan zone **2.204.192.in-addr.arpa** dengan type master dan file ``/etc/bind/kaizoku/2.204.192.in-addr.arpa`` pada file ``/etc/bind/named.conf.local`` di node EniesLobby. 
  
![3 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139537990-86f0c592-b9a5-4847-bdaf-57072b873180.jpg)

Lalu buat file **2.204.192.in-addr.arpa** pada folder ``/etc/bind/kaizoku di EniesLobby``. Lalu atur nama domain dan PTR yang menuju ke **franky.a06.com** seperti pada gambar berikut.  
  
![3 - konf di kaizoku](https://user-images.githubusercontent.com/55240758/139537984-62aca77a-87a7-45fc-b881-aaf949cb3d15.jpg)  

Kemudian lakukan pengecekkan dengan command ``host -t PTR 192.204.2.2`` yang akan mengarah ke **franky.06.com  **
  
![3 - host logue](https://user-images.githubusercontent.com/55240758/139537997-4ac0738e-1248-47dc-9083-3c1392665cd7.jpg)  

### Soal 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
### Jawaban 5
Pertama, edit file ``/etc/bind/named.conf.local`` pada node EniesLobby dengan menambahkan notify yes, also-notify dan allow-transfer dengan IP Water7 seperti pada gambar berikut.   
![4 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139538065-733e0054-df73-4f0d-9bfe-dffcc30c7cc8.jpg)
  
Lalu, edit file ``/etc/bind/named.conf.local`` pada node Water7 dengan menambahkan zone **franky.a06.com** dengan type slave, masters IP EniesLobby, dan file ``/etc/bind/kaizoku/franky.a06.com``.  
  
![4 - konf di bind named local water 7](https://user-images.githubusercontent.com/55240758/139538061-c5c21ba5-2bd5-4fb6-aefc-4edb51652fca.jpg)
  
Kemudian stop node EniesLobby dengan command ``service bind9 stop`` pada node EniesLobby. Setelah itu dilakukan pengecekan dengan command ``ping franky.a06.com`` pada node LogueTown yang akan menerima paket dari IP EniesLobby walaupun telah dimatikan. Sebagai berikut :  
  
![image](https://user-images.githubusercontent.com/55240758/139539821-215249c1-e602-475b-b601-db8a023672e2.png)


### Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
### Jawaban 6
Pertama, ubah file /etc/bind/named.conf.options pada node EniesLobby dengan meng-comment dnssec-validation auto; dan menambahkan allow-query(any;};.
![5 - konf di bind named options](https://user-images.githubusercontent.com/55240758/139538101-1f3d0489-12c1-4175-9dfb-b74a0dbf8e3c.jpg)
![5 - konf di bind sunnygo mecha water7](https://user-images.githubusercontent.com/55240758/139538092-bcf38d64-189f-4e05-98ae-3958c3997912.jpg)
![5 - konf di kaizoku franky enieslobby](https://user-images.githubusercontent.com/55240758/139538095-8b049487-56bf-4be6-994f-fa137fe353e7.jpg)

![5 - ping mecha logue](https://user-images.githubusercontent.com/55240758/139538106-8d694f46-061c-49e1-81c8-1b0ce92ce7e6.jpg)


### Soal 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Jawaban 7
pertama buka file /etc/bind/sunnygo/mecha.franky.a06.com pada Water7 dan buat subdomain general yang mengarah ke IP skypie dan alias www.general yang mengarah ke general.mecha.franky.e09.com.
![6 - ping general mecha alabasta](https://user-images.githubusercontent.com/55240758/139538130-a1d64fb8-5ecf-4e7d-8ec6-6004718fd0e3.jpg)

### Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.
### Jawaban 8
isi disini
![7-lynx franky alabasta](https://user-images.githubusercontent.com/55240758/139538207-9aec6f7d-8e29-44de-96f9-7af2332f91ba.jpg)
![7 - lynx franky loguetown](https://user-images.githubusercontent.com/55240758/139538209-4af47e87-2ac8-488b-957f-f25d8ff2438e.jpg)


### Soal 9
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.
### Jawaban 9
Pertama Buka file /etc/apache2/sites-available/franky.e09.com.conf pada Skypie dan ganti Alias menjadi "\home".
![8 - konf franky skypie](https://user-images.githubusercontent.com/55240758/139538159-cb9f9804-65cb-48b3-886e-8c8af8a0df05.jpg)
![8 - lynx franky home](https://user-images.githubusercontent.com/55240758/139538174-7cb8855c-ef5c-4bdc-bf33-b5e3d3b4a966.jpg)
lalu Buka lynx franky.e09.com/index.php/home dan lynx franky.e09.com/homedan dan keduanya akan menghasilkan sebagai berikut.

### Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com
### Jawaban 10
Pertama Buka file /etc/apache2/sites-available/super.franky.e09.com.conf pada Skypie dan ubah DocumentRoot menjadi /var/www/super.franky.yyy.com
![9 -  konf super skypie](https://user-images.githubusercontent.com/55240758/139538229-ba0dbbf7-e836-4476-b8ab-a013ed12efcb.jpg)

lalu Buka lynx super.franky.e09.com dan dia akan bisa buka aset.
![9 - lynx super](https://user-images.githubusercontent.com/55240758/139538224-9b4ae877-2414-402e-af6d-c5126da1a76f.jpg)

### Soal 11
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja
### Jawaban 11
Buka file /etc/apache2/sites-available/super.franky.e09.com.conf pada Skypie dan tambahkan pada /public "Options +Indexes"
![10 - konf](https://user-images.githubusercontent.com/55240758/139538231-ab38afab-50fc-4a31-ab99-9516ecb871ca.jpg)

Buka lynx super.franky.e09.com/public, maka akan muncul directory listing.
![10 - lynx](https://user-images.githubusercontent.com/55240758/139538232-10bf2d1e-8075-4dcb-bb5f-cebcf9372129.jpg)

### Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /errors untuk mengganti error kode pada apache
### Jawaban 12
buka file /etc/apache2/sites-available/super.franky.e09.com.conf dan ubah ErrorDocument menjadi 404 /error/404.html
![12 - konf](https://user-images.githubusercontent.com/55240758/139538236-f19c80f5-667a-4ef1-995f-96f7d313c068.jpg)

Buka lynx super.franky.e09.com/aowkwkwkwk maka akan muncul sebagai berikut.
![12 - lynx](https://user-images.githubusercontent.com/55240758/139538238-39c4913c-0ecb-4ea1-9f2d-fb02dc20e609.jpg)

### Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js
### Jawaban 13
isi disini
![13 - konf](https://user-images.githubusercontent.com/55240758/139538260-d734e0db-a3af-4989-a4e8-ca12a00717b6.jpg)
![13 - konf 15500](https://user-images.githubusercontent.com/55240758/139538246-6d800c28-5b56-4056-8cbe-8cdb2ef8da07.jpg)
![13 - konf  ports](https://user-images.githubusercontent.com/55240758/139538242-85108fb5-a2e3-4f51-9cb5-29268f38c081.jpg)

![13 - lynx 15000](https://user-images.githubusercontent.com/55240758/139538291-00a07a3b-cce6-4926-9a16-ff607a5161ac.jpg)
![13 - lynx 15500](https://user-images.githubusercontent.com/55240758/139538302-57aa9be2-29b3-4ded-9d5e-d2a43aa739c9.jpg)

### Soal 14
 Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Jawaban 14
isi disini

### Soal 15
 Dengan authentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Jawaban 15
isi disini
![14 - konf 15000](https://user-images.githubusercontent.com/55240758/139538307-b4cb69af-3934-4da7-8356-854012c6d82a.jpg)
![14 - konf 15500](https://user-images.githubusercontent.com/55240758/139538331-eaf10aaa-f1b0-40d5-96ad-31ebe8439bec.jpg)
![14 - username](https://user-images.githubusercontent.com/55240758/139538341-f748019d-e43f-4ce9-87d8-fb1f022f0940.jpg)
![14 - pass](https://user-images.githubusercontent.com/55240758/139538342-c8a30585-696e-4962-a76b-25c1bb6e8559.jpg)
![14 - user 15500](https://user-images.githubusercontent.com/55240758/139538351-4ba9dc61-6d7d-45ec-8042-203cf5ae2e74.jpg)
![14 - pass 15500](https://user-images.githubusercontent.com/55240758/139538387-5eeade35-aba6-4d01-9f11-696d47ebbca4.jpg)

### Soal 16
 Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com
### Jawaban 16
isi disini
![15 - konf enies](https://user-images.githubusercontent.com/55240758/139538397-a2b5629c-f2d9-4fff-8b2b-0acccc01aeb6.jpg)
![15 - lynx](https://user-images.githubusercontent.com/55240758/139538440-c29a7606-6256-4423-9bf2-981a85d8c5f2.jpg)
![15 - hasil](https://user-images.githubusercontent.com/55240758/139538428-40b006dc-5cda-4da9-8321-8e7e3a3e9006.jpg)

### Soal 17
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!
### Jawaban 17
isi disini
![16 - htaccesss](https://user-images.githubusercontent.com/55240758/139538456-1e35bf2a-530d-4737-be35-0d28e495aaf7.jpg)
![16 - super frankky](https://user-images.githubusercontent.com/55240758/139538462-902f3ceb-9225-437c-9801-f27feec32a8c.jpg)
![16 - mengandung franky](https://user-images.githubusercontent.com/55240758/139538472-31e0f7fc-3334-452f-be54-ead235582806.jpg)
![16 - franky png](https://user-images.githubusercontent.com/55240758/139538477-a066fb49-9add-4043-bf01-06f47af52ac3.jpg)



## Kendala yang dialami
- Tingkat kesulitan soal jauh meningkat dari sebelumnya
- Kepadatan jadwal membuat koordinasi kelompok kurang baik
- saat pengumpulan sempat mengalami error sehingga beberapa soal mengulang dari awal
