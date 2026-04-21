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

Di commit ini saya nambahin logic biar server gak cuma nerima koneksi, tapi juga balikin response HTML ke browser. Saya bikin file hello.html di root folder project, isinya HTML sederhana sama heading sama paragraf.

Di handle_connection, saya tambahin beberapa hal. Pertama baca isi hello.html pake fs::read_to_string. Terus saya siapin status line "HTTP/1.1 200 OK" yang nandain response suksesnya. Habis itu saya itung panjang kontennya pake contents.len() buat dipake di Content-Length header.

Response-nya saya bikin pake format! yang gabungin status line, Content-Length, sama isi HTML-nya. Yang agak tricky itu bagian \r\n\r\n sebelum contents, karena itu yang misahin header sama body di HTTP response. Kalau salah satu \r\n kurang, browser gak bakal bisa parse response-nya dengan bener.

Terakhir saya kirim response ke browser pake stream.write_all yang minta bentuk bytes, makanya pake as_bytes(). Pas saya tes di browser, halaman HTML-nya langsung muncul sesuai yang saya tulis. Dari sini saya ngerti kalau web server pada dasarnya cuma nulis string dengan format tertentu ke stream, terus browser yang nerjemahin jadi halaman web.

![Commit 2 screen capture](/assets/images/commit2.png)