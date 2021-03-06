# Jarkom-Modul-2-A06-2021
Lapres Praktikum Jaringan Komputer 2021 - Modul 2
- Muh. Nur Fajrin Amiruddin (05111940000005)
- Rihan Farih Bunyamin (05111940000165)
- Lathifa Itqonina Mardiyati (05111940000176)


## Soal  dan Jawaban
### Soal 1
Luffy adalah seorang yang akan jadi Raja Bajak Laut. Demi membuat Luffy menjadi Raja Bajak Laut, Nami ingin membuat sebuah peta, bantu Nami untuk membuat peta berikut :

![image9](https://user-images.githubusercontent.com/61973814/139194256-c0640fcf-d5ad-448f-a38d-905adf9a82dd.png)

EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

### Jawaban 1
Pertama, tambahkan node dan hubungkan masing-masing node hingga sesuai dengan gambar dan ganti hostname dari masing-masing node. Lalu setting network masing-masing node dengan fitur Edit network configuration. Kemudian jalankan perintah ``iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.204.0.0/16`` pada router Foosha. Lalu ketikkan command ``echo nameserver 192.168.122.1 > /etc/resolv.conf`` pada masing-masing node ubuntu. Lalu lakukan pengecekkan dengan melakukan ping ke google pada setiap node untuk mengetahui apakah sudah terhubung dengan internet. Salah satunya sebagai berikut :  

![image](https://user-images.githubusercontent.com/55240758/139542298-d05c1cee-9fb7-4ee6-9b2f-3fd84d3b0bf3.png)


### Soal 2
**Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku**
### Jawaban 2
- **Langkah satu** : Melakukan penginstallan konfigurasi yang diperlukan 
    ``` 
      apt-get update
      apt-get install bind9
    ```
- **Langkah kedua** : Setelah melakukan penginstalan, kemudian pada file ```/etc/bind/named.conf.local``` di EniesLobby, buat zone **franky.a06.com** dengan type master dan file     ```/etc/bind/kaizoku/franky.a06.com```.  
   
    ![1 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139537797-e978c7e3-3eb3-4015-9a9c-1c143b10924e.jpg)

- **Langkah ketiga** : Buat folder kaizoku di /etc/bind Lalu copykan file db.local pada path /etc/bind ke dalam folder kaizoku yang baru saja dibuat dan ubah namanya menjadi franky.a06.com
    ```
     cp /etc/bind/db.local /etc/bind/kaizoku/franky.a06.com
    ```
  
- **Langkah keempat** : Edit file **franky.a06.com** pada folder ```/etc/bind/kaizoku``` pada node EniesLobby dengan mengatur nama domain dan IP yang menuju ke EniesLobby (10.2.2.2). menjadi sebagai berikut :  
  
    ![1 - konf franky di kaizoku](https://user-images.githubusercontent.com/55240758/139537827-7e66c1aa-8a1e-4d98-ae76-d161b89295ef.jpg)

- **Langkah kelima** : lakukan restart bind pada node eniesLobby
    ```
    service bind9 restart
    ```
- **Langkah keenam** : menambahkan server eniesLobby (10.2.2.2) pada **/etc/resolv.conf** pada node client yaitu loguetown & alabasta 
    ```
    nameserver 10.2.2.2
    ```
- **Langkah keenam** : Setelah itu ```ping www.franky.a06.com``` atau ```ping franky.a06.com``` pada node Loguetown dan akan menampilkan tampilan sebagai berikut.  
  
    ![1 - ping franky loguetown](https://user-images.githubusercontent.com/55240758/139537867-233548c2-8ab9-4e29-ad28-d8d7207a4526.jpg)  



### Soal 3
**Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie**
### Jawaban 3
- **Langkah pertama** : mengatur nama subdomain, CNAME dan IP yang menuju ke Skypie (10.2.2.4) pada file **franky.a06.com** di folder ```/etc/bind/kaizoku``` pada node EniesLobby.
  
   ![2 - konf super di kaizoku](https://user-images.githubusercontent.com/55240758/139537898-9ca23739-e9ed-4079-8915-fa431853bc7b.jpg)

- **Langkah kedua** : Lakukan pengecekan dengan ```ping www.super.franky.a06.com``` pada node loguetown 
     
   ![2 - ping super alabasta](https://user-images.githubusercontent.com/55240758/139537901-26919d84-9aff-42c9-9861-8c5e3e759c22.jpg)

### Soal 4
 **Buat juga reverse domain untuk domain utama**
### Jawaban 4
- **Langkah pertama** : Edit file ```nano /etc/bind/named.conf.local``` dan tambahkan konfig seperti gambar berikut :    

    ![3 - konf di kaizoku](https://user-images.githubusercontent.com/55240758/139537984-62aca77a-87a7-45fc-b881-aaf949cb3d15.jpg)

- **Langkah kedua** : Tambahkan zone 2.2.10.in-addr.arpa dengan type master dan file ```/etc/bind/kaizoku/2.2.10.in-addr.arpa``` pada ```file /etc/bind/named.conf.local``` di node EniesLobby.
       
    ![3 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139537990-86f0c592-b9a5-4847-bdaf-57072b873180.jpg)

- **Langkah ketiga** : Kemudian lakukan pengecekkan dengan command ```host -t PTR 10.2.2.2``` yang akan mengarah ke **franky.a06.com**
      
    ![3 - host logue](https://user-images.githubusercontent.com/55240758/139537997-4ac0738e-1248-47dc-9083-3c1392665cd7.jpg)

### Soal 5
**Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama**
### Jawaban 5
- **Langkah 1** : melakukan edit file ```/etc/bind/named.conf.local``` pada node eniesLobby dengan menambahkan notify yes, also-notify dan allow-transfer dengan IP Water7 seperti pada gambar berikut.
   
    ![4 - konf di bind named local](https://user-images.githubusercontent.com/55240758/139538065-733e0054-df73-4f0d-9bfe-dffcc30c7cc8.jpg)

- **Langkah 2** : Lalu, edit file ```/etc/bind/named.conf.local``` pada node Water7 dengan menambahkan zone franky.a06.com dengan type slave, masters IP eniesLobby, dan file ```/etc/bind/kaizoku/franky.a06.com```. Seperti gambar berikut :
    
    ![4 - konf di bind named local water 7](https://user-images.githubusercontent.com/55240758/139538061-c5c21ba5-2bd5-4fb6-aefc-4edb51652fca.jpg)


### Soal 6
**Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo**
### Jawaban 6
- **Langkah kesatu** : Melakukan edit pada file ```/etc/bind/named.conf.options``` pada node eniesLobby dengan meng-comment dnssec-validation auto; dan menambahkan allow-query(any;};. seperti gambar berikut :
  
    ![5 - konf di bind named options](https://user-images.githubusercontent.com/55240758/139538101-1f3d0489-12c1-4175-9dfb-b74a0dbf8e3c.jpg)

- **Langkah kedua** : Lalu, ubah ```/etc/bind/kaizoku/franky.a06.com``` pada node eniesLobby menjadi domain utama yang IP yang menuju ke node Skypie, dan membuat domain mecha yang didelegasikan ke IP Water7. Seperti gambar berikut :
   
    ![5 - konf di kaizoku franky enieslobby](https://user-images.githubusercontent.com/55240758/139538095-8b049487-56bf-4be6-994f-fa137fe353e7.jpg)

- **Langkah ketiga** : Kemudian, buat file ```/etc/bind/sunnygo/mecha.franky.a06.com``` pada node Water7 dan mengatur domainnya menuju `mecha.franky.a06.com dengan IP Skypie  dengan CNAME www.
   
    ![5 - konf di bind sunnygo mecha water7](https://user-images.githubusercontent.com/55240758/139538092-bcf38d64-189f-4e05-98ae-3958c3997912.jpg)


- **Langkah keempat** : Lalu lakukan ```ping www.mecha.franky.a06.com``` pada node loguetown dan akan menerima paket dari IP Skypie.
   
    ![5 - ping mecha logue](https://user-images.githubusercontent.com/55240758/139538106-8d694f46-061c-49e1-81c8-1b0ce92ce7e6.jpg)


### Soal 7
**Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie**
### Jawaban 7
pertama buka file ```/etc/bind/sunnygo/mecha.franky.a06.com``` pada Water7 dan buat subdomain general yang mengarah ke IP skypie dan alias www.general yang mengarah ke general.mecha.franky.a06.com.

![image](https://user-images.githubusercontent.com/55240758/139562641-819b557c-778a-47a5-936a-5927d88bad65.png)

kemudian lakukan pengecekan dengan ```ping general.mecha.franky.a06.com```
   
![6 - ping general mecha alabasta](https://user-images.githubusercontent.com/55240758/139538130-a1d64fb8-5ecf-4e7d-8ec6-6004718fd0e3.jpg)

### Soal 8
**Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.**
### Jawaban 8
- **Langkah pertama** : Melakukan edit pada file ```/etc/apache2/sites-available/frangky.a06.com.conf``` pada node skypie dengan DocumentRoot /var/www/frangky.a06.com
ServerName frangky.a06.com, ServerAlias www.frangky.a06.com. seperti gambar berikut :
  
    ![8 - konf franky skypie](https://user-images.githubusercontent.com/55240758/139562772-69e3fc00-6fec-4636-97df-bd66c78df80f.jpg)
   
- **Langkah kedua**  : pada /var/www lakukan pendownloadan file yang telah disediakan pada soal shift yaitu pada https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom dengan command  ```wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/franky.zip ```. File yang telah didownload merupakan file zip, lakukan unzip dengan command ```unzip franky.zip```. Setelah itu rename nama folder yang telah di unzip atau **franky** menjadi **franky.a06.com**.
   
- **Langkah ketiga** : Lakukan pengecekan dengan command  ```lynx franky.a06.com``` atau ```lynx www.franky.a06.com``` pada node alabasta
    
    ![7-lynx franky alabasta](https://user-images.githubusercontent.com/55240758/139538207-9aec6f7d-8e29-44de-96f9-7af2332f91ba.jpg)

    kemudian lakukan command ```lynx franky.a06.com`` atau ```lynx www.franky.a06.com``` pada node loguetown. Keduanya akan mengarah ke IP yang sama sehingga menghasilkan webserver yang sama.
      
    ![7 - lynx franky loguetown](https://user-images.githubusercontent.com/55240758/139538209-4af47e87-2ac8-488b-957f-f25d8ff2438e.jpg)


### Soal 9
**Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.**
### Jawaban 9
- **Langkah pertama** : Buka file  dengan ``` nano /etc/apache2/sites-available/franky.a06.com.conf``` pada Skypie dan ganti Alias di file tersebut menjadi ```/home```. Sesuai gambar berikut :
   
    ![8 - konf franky skypie](https://user-images.githubusercontent.com/55240758/139538159-cb9f9804-65cb-48b3-886e-8c8af8a0df05.jpg)
    

- **Langkah kedua** : Kemudian menambahkan file .htaccess pada /var/www dengan isi:

    ```
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^([^\.]+)$ $1.html [NC,L]
    ```
   
- **Langkah ketiga** : Lakukan pengecekan dengan command ```lynx franky.a06.com/index.php/home``` dan ```lynx franky.a06.com/home``` pada node logue town atau alabasta. Kedua command tersebut akan menghasilkan sebagai berikut.
   
    ![8 - lynx franky home](https://user-images.githubusercontent.com/55240758/139538174-7cb8855c-ef5c-4bdc-bf33-b5e3d3b4a966.jpg)


### Soal 10
**Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com**
### Jawaban 10
- **Langkah pertama** : Menambahkan file ```super.franky.a06.com.conf ``` pada /etc/apache2/sites-available/ dengan mengcopy isi dari file franky.a06.com.conf sesuaikan dengan berikut :
    ```
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/super.frangky.a06.com
    ServerName super.frangky.a06.com
    ServerAlias www.super.frangky.a06.com
    ```
    
    ![9 -  konf super skypie](https://user-images.githubusercontent.com/55240758/139538229-ba0dbbf7-e836-4476-b8ab-a013ed12efcb.jpg)
    
- **Langkah kedua** : pada /var/www lakukan pendownloadan file yang telah disediakan pada soal shift yaitu pada https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom dengan command  ```wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/super.franky.zip ```. File yang telah didownload merupakan file zip, lakukan unzip dengan command ```unzip super.franky.zip```. Setelah itu rename nama folder yang telah di unzip atau **super.franky** menjadi **super.franky.a06.com**.
    
- **Langkah ketiga** :  lakukan pengecekan dengan command ```lynx super.franky.a06.com``` pada loguetown atau alabasta dan tampilan yang akan dibuka yaitu asset sebagai berikut.

    ![9 - lynx super](https://user-images.githubusercontent.com/55240758/139538224-9b4ae877-2414-402e-af6d-c5126da1a76f.jpg)

### Soal 11
**Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja**
### Jawaban 11

- **Langkah pertama** : Buka file ```/etc/apache2/sites-available/super.franky.a06.com.conf``` pada Skypie dan tambahkan pada /public "Options +Indexes"
  
    ![10 - konf](https://user-images.githubusercontent.com/55240758/139538231-ab38afab-50fc-4a31-ab99-9516ecb871ca.jpg)

- **Langkah kedua** : lakukan pengecekan dengan command ```lynx super.franky.a06.com/public```, maka akan muncul directory listing.
  
    ![10 - lynx](https://user-images.githubusercontent.com/55240758/139538232-10bf2d1e-8075-4dcb-bb5f-cebcf9372129.jpg)

### Soal 12
**Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /errors untuk mengganti error kode pada apache**
### Jawaban 12
- **Langkah pertama** : Buka file /etc/apache2/sites-available/super.franky.a06.com.conf dan ubah ErrorDocument menjadi 404 /error/404.html

    ![11 - konf](https://user-images.githubusercontent.com/55240758/139563884-4a961ddb-25bb-443a-9d29-4e83666c7130.jpg)


- **langkah kedua** : lakukan pengecekan dengan command ```lynx super.franky.a06.com/lathifa``` maka akan muncul halaman error sebagai berikut.

    ![11 - lynx error](https://user-images.githubusercontent.com/55240758/139563904-82e60748-299e-41a8-b74b-862bc608f1f7.jpg)       


### Soal 13
**Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js**
### Jawaban 13
- **Langkah pertama** : mengubah isi dari file ```/etc/apache2/sites-available/super.franky.a06.com.conf``` dengan menambahkan ```Alias "/js" "/var/www/super.franky.a06.com/public/js"``` seperti pada gambar di bawah ini. 

    ![12 - konf](https://user-images.githubusercontent.com/55240758/139538236-f19c80f5-667a-4ef1-995f-96f7d313c068.jpg)

- **Langkah kedua** : Lakukan pengecekan dengan command ```lynx super.franky.yyy.com/public/js```  pada client loguetown atau alabasta. Maka akan menghasilkan tampilan sebagai berikut dengan alias yang telah berjalan dengan benar.

    ![12 - lynx](https://user-images.githubusercontent.com/55240758/139538238-39c4913c-0ecb-4ea1-9f2d-fb02dc20e609.jpg)

### Soal 14
 Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Jawaban 14
- **Langkah pertama** : Menambahkan file ```general.super.franky.a06.com-15000.conf ``` pada /etc/apache2/sites-available/ dengan mengcopy isi dari file franky.a06.com.conf sesuaikan dengan berikut :

    ![13 - konf](https://user-images.githubusercontent.com/55240758/139538260-d734e0db-a3af-4989-a4e8-ca12a00717b6.jpg)
    
    Kemudian buat file ```general.super.franky.a06.com-15500.conf ``` dengan mngcopy file ```general.super.franky.a06.com-15000.conf``` melalui command ```cp general.super.franky.a06.com-15000.conf general.super.franky.a06.com-15500.conf ```. Lalu ubah portnya menjadi 15500 sebagai berikut :
    
    ![13 - konf 15500](https://user-images.githubusercontent.com/55240758/139538246-6d800c28-5b56-4056-8cbe-8cdb2ef8da07.jpg)
    
- **Langkah kedua** : pada /var/www lakukan pendownloadan file yang telah disediakan pada soal shift yaitu pada https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom dengan command  ```wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/general.mecha.franky.zip ```. File yang telah didownload merupakan file zip, lakukan unzip dengan command ```unzip general.mecha.franky.zip```. Setelah itu rename nama folder yang telah di unzip atau **general.mecha.franky** menjadi **general.mecha.a06.com**.

- **Langkah ketiga** : Lakukan penambahan pada file /etc/apache2/ports.conf dengan Listen 15000 & 15500 sebagai berikut :

    ![13 - konf  ports](https://user-images.githubusercontent.com/55240758/139538242-85108fb5-a2e3-4f51-9cb5-29268f38c081.jpg)

- **Langkah keempat** : Lakukan pengecekan dengan command ```lynx general.super.franky.a06.com:15000 ``` & ```lynx general.super.franky.a06.com:15500 ```pada node loguetown atau alabasta. Maka tampilan yang diberikan sebagai berikut :
    
    Pada port 15000
    
    ![13 - lynx 15000](https://user-images.githubusercontent.com/55240758/139538291-00a07a3b-cce6-4926-9a16-ff607a5161ac.jpg)
    
    Pada port 15500
    
    ![13 - konf 15500](https://user-images.githubusercontent.com/55240758/139564555-9e36ba86-cacb-4e56-82f3-a0152e0567a2.jpg)

    
 
### Soal 15
 **Dengan authentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy**
### Jawaban 15
 - **Langkah pertama** :  Membuat sebuah autentikasi username dan password pada server menggunakan : ```htpasswd -c /etc/apache2/.htpasswd luffy ```
 - **Langkah kedua** : Melakukan penambahan pada file ```/etc/apache2/sites-available/general.mecha.franky.a06.com-15000.conf```, dengan config Authentication sebagai berikut :
    ```
    <Directory /var/www/general.mecha.franky.a06.com>
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </Directory>
    ```
    ![14 - konf 15000](https://user-images.githubusercontent.com/55240758/139538307-b4cb69af-3934-4da7-8356-854012c6d82a.jpg)

    Lalu di dalam file /etc/apache2/sites-available/general.mecha.franky.a06.com-15500.conf, juga ditambahkan config Authentication
    
    ![14 - konf 15500](https://user-images.githubusercontent.com/55240758/139538331-eaf10aaa-f1b0-40d5-96ad-31ebe8439bec.jpg)

- **Langkah ketiga** : lakukan pengecekan dengan command ```lynx general.mecha.franky.a06.com:15000``` & ```lynx general.mecha.franky.a06.com:15500``` pada alabasta atau loguetown. Maka sebelum masuk halaman diminta untuk memasukkan user yang telah terdaftar pada /etc/apache2/.htpasswd yaitu luffy dan password onepiece. 

    ![14 - username](https://user-images.githubusercontent.com/55240758/139538341-f748019d-e43f-4ce9-87d8-fb1f022f0940.jpg)
    ![14 - pass](https://user-images.githubusercontent.com/55240758/139538342-c8a30585-696e-4962-a76b-25c1bb6e8559.jpg)
    
    apabila telah sesuai akan diarahkan ke tampilan halaman sebagai berikut :
    
    pada port 15000
    
    ![13 - lynx 15000](https://user-images.githubusercontent.com/55240758/139538291-00a07a3b-cce6-4926-9a16-ff607a5161ac.jpg)
    
    pada port 15500
    
    ![13 - konf 15500](https://user-images.githubusercontent.com/55240758/139564555-9e36ba86-cacb-4e56-82f3-a0152e0567a2.jpg)

    

### Soal 16
**Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com**
### Jawaban 16
- **Langkah pertama** : pada file ```/etc/bind/kaizoku/franky.a06.com``` di enieslobby lakukan perubahan untuk domain www.franky.a06.com yang semula IP eniesLobby (10.2.2.2) menjadi IP Skypie (10.2.2.4). Sebagai berikut :

    ![15 - konf enies](https://user-images.githubusercontent.com/55240758/139538397-a2b5629c-f2d9-4fff-8b2b-0acccc01aeb6.jpg)

- **Langkah kedua** : Melakukan testing pada client Loguetown atau alabasta dengan command ```lynx 10.2.2.4```
    ![image](https://user-images.githubusercontent.com/55240758/139564945-17bd89aa-b2be-4137-9379-dd4dc4e2a0a0.png)

    Didapatkan hasil sebagai berikut
    ![image](https://user-images.githubusercontent.com/55240758/139564959-1cd8c307-5709-46d9-8e7b-a7222f8b0cff.png)

### Soal 17
**Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring ???franky??? akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!**
### Jawaban 17

- **Langkah pertama** : membuat file .htaccess pada ```/var/www/super.franky.a06.com/public/images``` dengan memasukkan syntax sebagai berikut.
    ```
    RewriteEngine On
    RewriteBase /
    RewriteCond %{REQUEST_FILENAME} !\bfranky.png\b
    RewriteRule franky http://www.super.franky.a06.com/public/images/franky.png$1 [L,R=301] 
    ```
    dimana dilakukan redirect secara custom

    RewriteCond %{REQUEST_FILENAME} !\bfranky.png\b digunakan untuk memfilter setiap file yang tidak bernama franky.png

    Lalu lakukan rewrite kembali dengan menggunakan RewriteRule franky http://www.super.franky.a06.com/public/images/franky.png$1 [L,R=301] yang mendeteksi jika terdapat file yang memiliki substring franky akan diredirect ke file http://www.super.franky.a06.com/public/images/franky.png  . sebagai berikut :

    ![16 - htaccesss](https://user-images.githubusercontent.com/55240758/139538456-1e35bf2a-530d-4737-be35-0d28e495aaf7.jpg)

- **Langkah kedua** : lakukan penambahan ```AllowOverride``` All pada file ```/etc/apache2/sites-available/super.franky.a06.com.conf``` sebagai berikut
    ![16 - super frankky](https://user-images.githubusercontent.com/55240758/139538462-902f3ceb-9225-437c-9801-f27feec32a8c.jpg)

- **Langkah ketiga** : lakukan pengecekan dengan command ```lynx super.frangky.B05.com``` pada node loguetown atau alabasta, Ketika mendownload file eyeoffranky.jpg maka, web akan mendirect ke file franky.png, karena pada file tersebut terdapat substring franky.

    ![16 - mengandung franky](https://user-images.githubusercontent.com/55240758/139538472-31e0f7fc-3334-452f-be54-ead235582806.jpg)

    ![16 - franky png](https://user-images.githubusercontent.com/55240758/139538477-a066fb49-9add-4043-bf01-06f47af52ac3.jpg)



## Kendala yang dialami
- Tingkat kesulitan soal jauh meningkat dari sebelumnya
- Kepadatan jadwal membuat koordinasi kelompok kurang baik
- saat pengumpulan sempat mengalami error sehingga beberapa soal mengulang dari awal
