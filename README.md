# Jarkom Modul 3 F02 2022

### Anggota:

1. [Andymas Narendra Bagaskara](https://github.com/zaibir123) (05111940000192)
2. [Jayanti Totti Andhina](https://github.com/JayantiTA) (5025201037)
3. [Gaudhiwaa Hendrasto](https://github.com/gaudhiwaa) (5025201066)

### Proxy Server

1.  Client hanya dapat mengakses internet diluar (selain) hari & jam
    kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses
    24 jam penuh)

Install squid pada node Berlint, kemudian backup file
/etc/squid/squid.conf ke file /etc/squid/squid.conf.bak

Tambahkan acl.conf:

![](./media/image34.png)

Tambahkan script pada /etc/squid/squid.conf:

![](./media/image24.png)

Pada client jalankan \`export
http\_proxy="[[http://192.200.2.3:8080]{.underline}](http://192.175.2.3:8080)"\`,
kemudian testing membuka google ketika jam kerja:

![](./media/image20.png)

testing membuka google di luar jam kerja:

![](./media/image14.png)

2.  Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat
    mengakses domain loid-work.com dan franky-work.com (IP tujuan
    domain dibebaskan)

Tambahkan konfigurasi domain pada /etc/bind/named.conf.local pada
WISE:

![](./media/image36.png)

Buat direktori /etc/bind/work, kemudian copy isi db.local ke dalam
file loid-work.com dan franky-work.com:

![](./media/image11.png)

Kemudian edit isi /etc/bind/work/loid-work.com:

![](./media/image30.png)

Edit juga isi /etc/bind/franky-work.com:

![](./media/image13.png)

Kemudian restart service bind.

Tambahkan kedua domain tersebut pada /etc/squid/work-sites.acl:

![](./media/image28.png)

Kemudian tambahkan script pada /etc/squid/squid.conf:

![](./media/image3.png)

Testing pada client:

![](./media/image7.png)

Jika diakses menggunakan lynx pada jam kerja maka akan muncul error
dengan kode 503:

![](./media/image31.png)

Jika diakses menggunakan lynx di luar jam kerja maka akan muncul error
403:

![](./media/image8.png)

3.  Saat akses internet dibuka, client dilarang untuk mengakses web
    tanpa HTTPS. (Contoh web HTTP:
    [[http://example.com]{.underline}](http://example.com))

Tambahkan script pada squid.conf

![](./media/image21.png)

Testing pada client tanpa https:

![](./media/image22.png)

Testing pada client menggunakan https di luar jam kerja:

![](./media/image5.png)

4.  Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan
    maksimum 128 Kbps pada setiap host (Kbps = kilobit per second;
    lakukan pengecekan pada tiap host, ketika 2 host akses internet
    pada saat bersamaan, **keduanya mendapatkan speed maksimal yaitu
    128 Kbps**)

![](./media/image1.png)

5.  Setelah diterapkan, ternyata peraturan nomor (4) mengganggu
    produktifitas saat hari kerja, dengan demikian pembatasan
    kecepatan hanya diberlakukan untuk pengaksesan internet pada hari
    libur

![](./media/image10.png)

