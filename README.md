# advprog-modul6

Reflection 1

Di commit ini saya bikin Tpelajarin di PBP, bedanya sekarang saya liat rawnya menerima koneksi dari browser.
Awalnya saya hanya print "Connection established!" buat ngecek koneksinya masuk atau tidak.
Ternyata satu kali refresh browser bisa bikin beberapa koneksi masuk sekaligus.
Lalu saya pisahkan logicnya ke function handle_connection yang nerima TcpStream, di dalamnya saya pake BufReader untuk baca stream-nya per baris.
Saya sempat bingung kenapa tidak baca langsung dari stream, ternyata BufReader lebih efisien karena dia menyimpan data dulu sebelum dibaca dan juga punya method lines yang enak dipake buat iterate.
Untuk parsing requestnya, saya chain method lines, map, take_while, dan collect.
Bagian take_while penting banget karena dipakai untuk berhenti pas ketemu baris kosong.
Kalau tidak ada itu, programnya mungkin bakal nunggu terus dan gak selesai-selesai.
Output yang muncul di terminal itu isinya HTTP request lengkap, ada request line, Host, User-Agent, dan lain-lain.
Ini mirip kayak yang saya pelajarin di PBP, bedanya sekarang saya liat rawnya langsung tanpa framework.


Reflection 2

Di commit ini saya tambahkan logic agar server tidak cuma nerima koneksi, tapi juga balikin response HTML ke browser. 
Lalu membuat file hello.html di root folder project, isinya HTML sederhana sama heading sama paragraf.
Lalu di handle_connection, saya tambahin beberapa hal. 
Pertama baca isi hello.html pake fs::read_to_string. Lalu saya siapkan status line "HTTP/1.1 200 OK" yang menandakan response suksesnya. 
Lalu saya hitung panjang kontennya pake contents.len() untuk dipakai di Content-Length header.
Response-nya bikin dengan format! yang gabungin status line, Content-Length, sama isi HTML-nya.
Terakhir saya kirim response ke browser pake stream.write_all yang minta bentuk bytes, makanya pake as_bytes(). 
Lalu saat tes di browser, halaman HTML-nya langsung muncul sesuai yang saya tulis. 
Dari sini saya mengerti kalau web server pada dasarnya cuma nulis string dengan format tertentu ke stream, terus browser yang nerjemahin jadi halaman web.

![Commit 2 screen capture](/assets/images/commit2.png)

Reflection 3

Di commit ini saya nambahin logic agar server bisa bedain request yang valid dan yang tidak. 
Jika request-nya GET ke root path ("GET / HTTP/1.1"), server balikin hello.html dengan status 200 OK. 
Jika request-nya selain itu, server balikin 404.html dengan status 404 NOT FOUND.
Awalnya saya sempet bikin versi pertama pake if-else biasa yang ngulang kode hampir semuanya, cuma beda di status_line sama nama file-nya.
Lalu saya refactor dengan tuple (status_line, filename) yang dikembaliin dari if-else, llau sisa kodenya cuma ditulis sekali.
Logic pilihannya ada di atas, sementara logic bikin response-nya ada di bawah. 
Jika nanti mau menambahkan path baru, tinggal tambah cabang di if-else tanpa perlu duplikat kode response-nya.
Lalu membuat file 404.html untuk halaman error-nya. 

![Commit 3 screen capture](/assets/images/commit3.png)

