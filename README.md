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