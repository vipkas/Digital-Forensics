# Lab 1 — Network Traffic Forensics

## Network Traffic Forensics

Di dunia saat ini, jaringan adalah bagian penting dari kehidupan sehari-hari yang memungkinkan kita untuk berkomunikasi, mengakses informasi, dan melakukan berbagai aktivitas online. Namun, jaringan juga menjadi target utama bagi aktor jahat yang mencoba memanfaatkan kerentanan untuk menyebabkan kerugian.

Dalam lab ini, kita akan memulai dengan penyegaran dasar jaringan, termasuk alamat IP, protokol, dan port. Kita akan mengeksplorasi bagaimana elemen-elemen ini bekerja sama untuk memfasilitasi komunikasi antar perangkat.

Selanjutnya, kita akan melakukan analisis forensik lalu lintas jaringan. Kita akan menganalisis lalu lintas yang tertangkap untuk menemukan aktivitas yang mencurigakan atau berbahaya, yang mencakup mencari anomali dalam pola lalu lintas, mengidentifikasi upaya akses tidak sah, dan melacak sumber serangan apa pun.

## Basic Networking Refresher

Secara sederhana, jaringan adalah sekumpulan perangkat yang terhubung untuk berkomunikasi, berbagi informasi, file, dan sumber daya. Perangkat ini membutuhkan alamat IP, protokol, dan port untuk dapat saling berkomunikasi. Memahami elemen-elemen ini sangat penting bagi setiap analis forensik digital karena ini adalah dasar dari investigasi aktivitas berbahaya di jaringan.

### IP Addresses

Alamat IP adalah pengenal unik yang diberikan ke setiap perangkat di jaringan. Alamat ini digunakan untuk mengidentifikasi lokasi perangkat dan memfasilitasi komunikasi dengan perangkat lain. Contoh alamat IP adalah “192.168.1.1”.

### Protocols

Protokol adalah serangkaian aturan yang mengatur komunikasi antar perangkat di jaringan. Beberapa protokol umum termasuk HTTP, SSH, dan FTP. Misalnya, ketika Anda mengunjungi sebuah situs web, komputer Anda mengirim permintaan HTTP ke server web, yang kemudian merespons dengan mengirim halaman yang diminta.

### Ports

Port adalah nomor yang digunakan untuk mengidentifikasi aplikasi atau layanan tertentu pada perangkat. Saat perangkat menerima lalu lintas jaringan, perangkat tersebut menggunakan nomor port untuk menentukan aplikasi atau layanan yang dimaksud. Misalnya, ketika Anda mengakses situs web menggunakan HTTP, permintaan dikirim ke port 80 pada server web.

Tabel berikut menunjukkan pemetaan antara beberapa protokol umum dan nomor port mereka.

| Protocol                                    | Port  |
| ------------------------------------------- | ----- |
| FTP (File Transfer Protocol)                | 21    |
| SSH (Secure Shell)                          | 22    |
| Telnet (Teletype Network)                   | 23    |
| SMTP (Simple Mail Transfer Protocol)        | 25    |
| DNS (Domain Name System)                    | 53    |
| DHCP (Dynamic Host Configuration Protocol)  | 67/68 |
| HTTP (Hyper Text Transfer Protocol)         | 80    |
| HTTPS (Hyper Text Transfer Protocol Secure) | 443   |
| SMB (Server Message Block)                  | 445   |
| RDP (Remote Desktop Protocol)               | 3389  |
| ICMP (Internet Control Message Protocol)    | N/A   |

## Network Traffic Forensics

Di bagian ini, kita akan mengeksplorasi  tools dan techniques yang digunakan untuk menangkap dan menganalisis lalu lintas jaringan, mengekstrak informasi penting, mendeteksi perilaku mencurigakan, dan menyelidiki serangan di jaringan.

### Capturing Network Traffic

Prasyarat untuk melakukan forensik lalu lintas jaringan adalah menangkap lalu lintas jaringan. Ini dapat dilakukan menggunakan alat penangkap paket atau alat _sniffing_, seperti Wireshark atau tcpdump. Untuk lab ini, kita akan menggunakan Wireshark, jadi pastikan Anda telah menginstalnya.

Langkah-langkah untuk menangkap lalu lintas jaringan:

1. Buka Wireshark melalui terminal di Linux atau melalui menu Start di Windows.
2. Pilih antarmuka jaringan yang ingin Anda pantau, biasanya eth0.
3. Klik ikon sirip hiu berwarna biru di kiri atas untuk memulai penangkapan lalu lintas.
4. Anda seharusnya sudah melihat paket mulai muncul. Jika tidak, cobalah mengunjungi situs seperti [https://www.google.com/](https://www.google.com/).
5. Untuk menghentikan penangkapan, klik tombol stop berwarna merah di kiri atas.
6. Untuk menyimpan lalu lintas yang tertangkap, pilih **File → Save as**, lalu tentukan nama dan lokasi penyimpanan.

Catatan: Secara default, ekstensi file harus .pcapng, tetapi ekstensi umum lain untuk file penangkapan adalah .pcap.

### Analyzing Network Traffic

Langkah selanjutnya dalam forensik lalu lintas jaringan adalah menganalisis lalu lintas yang tertangkap. Ini mencakup pemeriksaan lalu lintas jaringan untuk mengidentifikasi aktivitas mencurigakan, upaya akses tidak sah, dan mengekstrak informasi seperti:

* Alamat IP sumber dan tujuan serta port
* Protokol yang digunakan
* Data/payload yang dikirim
* Tanggal dan waktu aktivitas

Ada banyak fitur yang tersedia di Wireshark untuk forensik jaringan, termasuk melihat hierarki protokol, menerapkan filter, melihat detail paket dan byte paket, mengikuti aliran TCP, dan mengekspor objek.

Untuk mulai menganalisis lalu lintas jaringan yang tertangkap, unduh file dari URL [capture.pcapng](01/files/capture.pcapng) dan buka di Wireshark.

#### Protocol Hierarchy

Untuk mendapatkan gambaran umum tentang lalu lintas jaringan yang tertangkap, buka **Statistics → Protocol Hierarchy** untuk melihat protokol yang digunakan dan jumlah relatif paket untuk setiap protokol. Ini membantu mempersempit analisis kita dan memfilter lalu lintas yang mencurigakan.

![protocol\_hierarchy](.gitbook/assets/protocol\_hierarchy.png)

#### Filters

Filter dapat diterapkan untuk memfokuskan analisis pada lalu lintas yang kita butuhkan. Hal ini mempermudah analisis hanya pada paket yang relevan.

![filter\_http](.gitbook/assets/filter\_http.png)

Demikian pula, kita dapat memfilter paket FTP:

![filter\_ftp](.gitbook/assets/filter\_ftp.png)

Untuk melihat filter lainnya yang didukung Wireshark, kunjungi [Display Filters Wireshark](https://wiki.wireshark.org/DisplayFilters).

#### Packet Details dan Packet Bytes

Panel detail paket menunjukkan paket yang dipilih secara rinci, sedangkan panel byte paket menunjukkan data dari paket yang dipilih dalam format hexdump. Kedua panel ini berada di bagian bawah jendela Wireshark

![details\_and\_bytes](.gitbook/assets/details\_and\_bytes.png)

**Mengikuti Aliran TCP**

Fitur **Follow TCP Stream** di Wireshark menampilkan seluruh percakapan untuk koneksi TCP tertentu, sehingga memudahkan melihat detail penuh dari koneksi dan data yang dikirim selama koneksi tersebut.

![tcp\_stream](.gitbook/assets/tcp\_stream.png)

#### Export Objects

Fitur **Export Objects** memungkinkan kita untuk mengekstrak file dari lalu lintas jaringan yang tertangkap. Pilih **File → Export Objects** dan, pada sub-menu "HTTP", lihat daftar file yang ditransfer melalui HTTP selama penangkapan. Setelah memilih file yang ingin diekstrak, simpan dengan menekan tombol "Save".

![export\_objects](.gitbook/assets/export\_objects.png)

**Kesimpulan**

Forensik lalu lintas jaringan adalah bagian penting dari forensik digital, karena memungkinkan kita mengidentifikasi aktivitas jahat, potensi pelanggaran keamanan, dan anomali lain di jaringan. Selain Wireshark, terdapat alat lain seperti Network Miner, Brim, dan Zeek yang bermanfaat untuk menganalisis lalu lintas yang tertangkap, jadi cobalah untuk mengeksplorasi dan bereksperimen.

**Latihan**

Organisasi yang sebelumnya mempekerjakan Anda untuk menyelidiki serangan web menghubungi Anda lagi. Kali ini, mereka telah berhasil menangkap lalu lintas jaringan selama serangan. Mereka menyediakan file lalu lintas yang tertangkap untuk membantu menyusun niat penyerang dan sejauh mana kerusakan terjadi. Tugas Anda adalah menganalisis lalu lintas yang tertangkap dan menjawab pertanyaan berikut:

1. Apa saja protokol yang ada dalam file lalu lintas yang tertangkap?
2. Tampaknya penyerang mencoba _brute force_ kata sandi FTP pengguna. Apakah ada bukti kata sandi yang benar, dan jika ada, apa itu?
3. Informasi tambahan apa yang berhasil diekstrak penyerang dari akun FTP pengguna?
4. Apa tindakan yang dilakukan penyerang dengan informasi dari akun FTP pengguna?
5. Apa kata sandi akun root?
6. Dapatkah Anda mengidentifikasi nomor paket di mana penyerang mengeksploitasi kerentanan Eksekusi Kode Jarak Jauh untuk mendapatkan akses ke sistem? Apa payload yang digunakan penyerang?
7. Setelah mendapatkan akses ke sistem, apa yang dilakukan penyerang?
8. Penyerang membaca file dari direktori home root. Apa isi file tersebut?
9. Penyerang mengunduh file di direktori home root. Apa tujuan file tersebut?
10. Informasi apa yang ditransmisikan melalui saluran komunikasi tersembunyi yang dibuat oleh penyerang?

File lalu lintas dapat diunduh dari [challenge.pcapng](01/files/challenge.pcapng).
