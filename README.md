# Jarkom Modul 3 F02 2022

### Anggota:

1. [Andymas Narendra Bagaskara](https://github.com/zaibir123) (05111940000192)
2. [Jayanti Totti Andhina](https://github.com/JayantiTA) (5025201037)
3. [Gaudhiwaa Hendrasto](https://github.com/gaudhiwaa) (5025201066)

### Topologi:

![](./media/image23.png)

### DHCP:

1.  WISE sebagai DNS Server:

![](./media/image33.png)

Kemudian install bind9

Westalis sebagai DHCP Server:

![](./media/image32.png)

Kemudian install isc-dhcp-server

Setup /etc/default/isc-dhcp-server:

![](./media/image2.png)

Setup dhcpd.conf:

![](./media/image18.png)

Berlint sebagai Proxy Server:

![](./media/image38.png)

Kemudian install squid

Clients network interfaces (SSS, Garden, Eden, NewstonCastle,
KemonoPark):

![](./media/image37.png)

2.  Setup network interfaces Ostania sebagai DHCP Relay:

![](./media/image12.png)

Kemudian install isc-dhcp-relay

Setup /etc/default/isc-dhcp-relay:

![](./media/image26.png)

3.  Client yang melalui Switch1 mendapatkan range IP dari \[prefix
    IP\].1.50 - \[prefix IP\].1.88 dan 192.200.1.120 - 192.200.1.155

![](./media/image4.png)

Restart node client terlebih dahulu, kemudian testing client pada
switch1:

![](./media/image19.png)

4.  Client yang melalui Switch3 mendapatkan range IP dari \[prefix
    IP\].3.10 - \[prefix IP\].3.30 dan 192.200.3.60 - 192.200.3.85

![](./media/image15.png)

Restart node client terlebih dahulu, kemudian testing client pada
switch3:

![](./media/image29.png)

5.  Client mendapatkan DNS dari WISE dan client dapat terhubung dengan
    internet melalui DNS tersebut.

Tambahkan DNS Forwarder pada WISE:

![](./media/image16.png)

Client pada switch1:

![](./media/image25.png)

Client pada switch3:

![](./media/image9.png)

6.  Lama waktu DHCP server meminjamkan alamat IP kepada Client yang
    melalui Switch1 selama 5 menit sedangkan pada client yang melalui
    Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan
    untuk peminjaman alamat IP selama 115 menit.

Ubah default-lease-time menjadi 300 dan max-lease-time menjadi 6900
pada switch1:

![](./media/image4.png)

Ubah default-lease-time menjadi 600 dan max-lease-time menjadi 6900
pada switch3:

![](./media/image15.png)

7.  Loid dan Franky berencana menjadikan **Eden** sebagai server untuk
    pertukaran informasi dengan **alamat IP yang tetap** dengan IP
    \[prefix IP\].3.13

Cek hwaddress milik Eden:

![](./media/image35.png)

Tambahkan script pada /etc/dhcp/dhcpd.conf:

![](./media/image17.png)

Tambahkan network interfaces pada Eden:

![](./media/image27.png)

Restart isc-dhcp-server dan restart node Eden, kemudian cek ip Eden:

![](./media/image6.png)

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

