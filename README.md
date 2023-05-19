# Praktikum 7 - PHP Framework (Codeigniter)

## Langkah-langkah Praktikum

## A. Persiapan 

Sebelum memulai menggunakan Framework Codeigniter, perlu dilakukan konfigurasi pada webserver. Beberapa ekstensi PHP perlu diaktifkan untuk kebutuhan pengembangan Codeigniter 4.

Berikut beberapa ekstensi yang perlu diaktifkan:

* **php-json** ekstension untuk bekerja dengan JSON;
* **php-mysqlnd** native driver untuk MySQL;
* **php-xml** ekstension untuk bekerja dengan XML;
* **php-intl** ekstensi untuk membuat aplikasi multibahasa;
* **libcurl** (opsional), jika ingin pakai Curl.

Untuk mengaktifkan ekstensi tersebut, melalui **XAMPP Control Panel**, pada bagian **Apache** klik **Config** -> **PHP(php.ini)**

![Gambar 1](ss/1.png)


Pada bagian extention, hilangkan tanda ; (titik koma) pada ekstensi yang akan diaktifkan. Kemudian simpan kembali filenya dan restart Apache web server.

![Gambar 2](ss/2.png)


## B. Installasi Codeigniter 4

Untuk melakukan instalasi Codeigniter 4 dapat dilakukan dengan dua cara, yaitu cara **manual** dan menggunakan **composer**. Pada praktikum ini kita menggunakan cara manual.

* Unduh Codeigniter dari website [Codeigniter](https://codeigniter.com/download).
* Extrak file zip Codeigniter ke direktori **htdocs/lab7_ci**.
* Ubah nama direktory **framework-4.x.xx** menjadi **ci4**.
* Buka browser dengan alamat http://localhost/lab7_ci/ci4/public/ 

![Gambar 3](ss/3.png)


## C. Menjalankan CLI

1. Codeigniter 4 menyediakan CLI untuk mempermudah proses development. Untuk mengakses CLI buka terminal/command prompt pada xampp.

2. Arahkan lokasi direktori sesuai dengan direktori kerja project dibuat **(xampp/htdocs/lab7_ci/ci4/)**.

3. Perintah yang dapat dijalankan untuk memanggil CLI Codeigniter adalah : `php spark`

![Gambar 4](ss/4.png)


## D. Mengaktifkan Mode Debugging

Codeigniter 4 menyediakan fitur debugging untuk memudahkan developer untuk mengetahui pesan error apabila terjadi kesalahan dalam membuat kode program. Secara default fitur ini belum aktif. Ketika terjadi error pada aplikasi akan ditampilkan pesan kesalahan seperti berikut.

![Gambar 5](ss/5.png)

Semua jenis error akan ditampilkan sama. Untuk memudahkan mengetahui jenis errornya, maka perlu diaktifkan mode debugging dengan mengubah nilai konfigurasi pada environment variable **CI_ENVIRONMENT** menjadi **development**.

1. Ubah nama file **env** menjadi **.env** kemudian buka file tersebut dan ubah nilai variable **CI_ENVIRONMENT** menjadi **development**, hilangkan juga tanda **'#'**.

![Gambar 6](ss/6.png)

2. Contoh error yang terjadi. Untuk mencoba error tersebut, ubah kode pada file **app/Controller/Home.php** hilangkan titik koma pada akhir kode.

![Gambar 7](ss/7.png)


![Gambar 8](ss/8.png)


## E. Routing dan Controller

Routing merupakan proses yang mengatur arah atau rute dari request untuk menentukan fungsi/bagian mana yang akan memproses request tersebut. Pada framework CI 4, routing bertujuan untuk menentukan Controller mana yang harus merespon sebuah request. **Controller adalah class atau script yang bertanggung jawab merespon sebuah request**. Pada Codeigniter, request yang diterima oleh file **index.php** akan diarahkan ke Router untuk kemudian oleh router tesebut diarahkan ke Controller.

* Router terletak pada file **app/config/Routes.php**

![Gambar 9](ss/9.png)

* Pada file tersebut kita dapat mendefinisikan route untuk aplikasi yang kita buat.
Contoh :
    `$routes->get('/', 'Home::index');`
(Kode tersebut akan mengarahkan rute untuk halaman home.)

### 1. Membuat Route Baru

* Tambahkan kode berikut di dalam **Routes.php**.
    ```bash
        $routes->get('/about', 'Page::about'); 
        $routes->get('/contact', 'Page::contact'); 
        $routes->get('/faqs', 'Page::faqs');
    ```

![Gambar 10](ss/10.png)

* Untuk mengetahui route yang ditambahkan sudah benar, buka CLI dan jalankan perintah berikut.
    `php spark routes`

![Gambar 11](ss/11.png)

* Sebelum mengakses route yang telah dibuat, buka CLI dan jalankan perintah `php spark serve`. Kemudian akses alamat url http://localhost:8080/about

![Gambar 12](ss/12.png)

![Gambar 13](ss/13.png)

Ketika diakses akan mucul tampilan error 404 file not found, itu artinya file/page tersebut tidak ada. Untuk dapat mengakses halaman tersebut, harus dibuat terlebih dahulu Contoller yang sesuai dengan routing yang dibuat yaitu Contoller Page.

### 2. Membuat Controller

* Buat file baru dengan nama page.php pada direktori Controller kemudian isi kodenya seperti berikut.

```php
    <?php 
    namespace App\Controllers; 
    class Page extends BaseController 
    { 
        public function about() 
        { 
            echo "Ini halaman About"; 
        } 
        public function contact() 
        { 
            echo "Ini halaman Contact"; 
        } 
        public function faqs() 
        { 
            echo "Ini halaman FAQ"; 
        } 
    }
```

* Selanjutnya refresh Kembali browser, maka akan ditampilkan hasilnya yaitu halaman sudah dapat diakses.

![Gambar 14](ss/14.png)


### 3. Auto Routing

* Secara default fitur *autoroute* pada Codeiginiter sudah aktif. Untuk mengubah status autoroute dapat mengubah nilai variabelnya. Untuk menonaktifkan ubah nilai **true** menjadi **false**.

`$routes->setAutoRoute(true);`

* Tambahkan method baru pada **Controller Page** seperti berikut.

```php
    public function tos() 
    { 
        echo "ini halaman Term of Services"; 
    }
```

* Method ini belum ada pada routing, sehingga cara mengaksesnya dengan menggunakan alamat: http://localhost:8080/page/tos

![Gambar 15](ss/15.png)


### 4. Membuat View

* Buat file baru dengan nama about.php pada direktori view **(app/view/about.php)** kemudian isi kodenya seperti berikut.

```html
    <!DOCTYPE html> 
    <html lang="en"> 
    <head> 
        <meta charset="UTF-8"> 
        <title><?= $title; ?></title> 
    </head> 
    <body> 
        <h1><?= $title; ?></h1> 
        <hr> 
        <p><?= $content; ?></p> 
    </body> 
    </html>
```

* Ubah **method about** pada class **Controller Page** menjadi seperti berikut:

```php
    public function about() 
    { 
        return view('about', [ 
            'title' => 'Halaman Abot', 
            'content' => 'Ini adalah halaman abaut yang menjelaskan tentang isi halaman ini.' 
        ]); 
    }
```

* Kemudian lakukan refresh pada halaman tersebut.

![Gambar 16](ss/16.png)


### 5. Membuat Layout Web dengan CSS

Pada dasarnya layout web dengan css dapat diimplamentasikan dengan mudah pada codeigniter. Yang perlu diketahui adalah, pada Codeigniter 4 file yang menyimpan asset css dan javascript terletak pada direktori **public**.

* Buat file css pada direktori **public** dengan nama **style.css** (copy file dari praktikum **lab1**. Kita akan gunakan layout yang pernah dibuat pada praktikum 1).

![Gambar 17](ss/17.png)

* Kemudian buat folder template pada direktori view kemudian buat file **header.php** dan **footer.php**.

* File **app/view/template/header.php**

```html
    <!DOCTYPE html> 
    <html lang="en"> 
    <head> 
        <meta charset="UTF-8"> 
        <title><?= $title; ?></title> 
        <link rel="stylesheet" href="<?= base_url('/style.css');?>"> 
    </head> 
    <body>
        <div id="container"> 
        <header> 
            <h1>Layout Sederhana</h1> 
        </header> 
        <nav> 
            <a href="<?= base_url('/');?>" class="active">Home</a> 
            <a href="<?= base_url('/artikel');?>">Artikel</a> 
            <a href="<?= base_url('/about');?>">About</a> 
            <a href="<?= base_url('/contact');?>">Kontak</a> 
        </nav> 
        <section id="wrapper"> 
            <section id="main">
```

* File **app/view/template/footer.php**

```html
    </section> 
    <aside id="sidebar"> 
        <div class="widget-box"> 
            <h3 class="title">Widget Header</h3> 
            <ul> 
                <li><a href="#">Widget Link</a></li> 
                <li><a href="#">Widget Link</a></li> 
            </ul> 
        </div> 
        <div class="widget-box"> 
            <h3 class="title">Widget Text</h3> 
            <p>Vestibulum lorem elit, iaculis in nisl volutpat, malesuada tincidunt arcu. Proin in leo fringilla, vestibulum mi porta, faucibus felis. Integer pharetra est nunc, nec pretium nunc pretium ac.</p> 
        </div> 
    </aside> 
    </section> 
    <footer> 
        <p>&copy; 2021 - Universitas Pelita Bangsa</p> 
    </footer> 
    </div> 
    </body> 
    </html>
```

* Kemudian ubah file **app/view/about.php** seperti berikut.

```php
    <?= $this->include('template/header'); ?> 
    
    <h1><?= $title; ?></h1> 
    <hr> 
    <p><?= $content; ?></p> 
    
    <?= $this->include('template/footer'); ?>
```

* Selanjutnya refresh tampilan pada alamat http://localhost:8080/about

![Gambar 18](ss/18.png)


## Pertanyaan dan Tugas

Lengkapi kode program untuk menu lainnya yang ada pada Controller Page, sehingga semua link pada navigasi header dapat menampilkan tampilan dengan layout yang sama.

## Hasil

![Gambar 19](ss/19.png)


## Terimakasih!