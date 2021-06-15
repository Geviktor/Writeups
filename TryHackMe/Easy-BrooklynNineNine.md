# Brooklyn Nine Nine

[<img src=".Images/nine.jpeg" height="199">](https://tryhackme.com/room/brooklynninenine)

*"This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box."* -[Fsociety2006](https://tryhackme.com/p/Fsociety2006)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması yaparak başlıyorum.

![nine-1](.Images/nine-1.png)

Cihazda: 21 portunda FTP, 22 portunda SSH ve 80 portunda HTTP servisi çalışıyor. Nmap taramasından FTP servisinin anonymous girişe izin verdiğini görüyorum ve giriş yapıyorum. İçerideki txt dosyasını okuduğumda kullanıcı olabilecek "amy" ve "jake" isimleriyle karşılaşıyorum ve jake'in şifresinin zayıf olduğunu öğreniyorum.

![nine-2](.Images/nine-2.png)

Jake kullanıcısı için bruteforce saldırısı denemeden önce web sitesine de bakmak istiyorum. Sitede bir jpg ve stenografi yapmamızı söyleyen bir yorum satırı ile karşılaşıyorum.

******

## [Gain Shell]

Görsel üzerinde bir kaç deneme yaptıktan sonra stegcracker ile bruteforce saldırısı yapıyorum ve içinden çıkan txt'de bir şifre ile karşılaşıyorum. Bir şifre bulmuş olmama rağmen jake kullanıcısına da hydra ile bruteforce saldırısı yapıyorum ve onun da şifresini buluyorum.

![nine-3](.Images/nine-3.png)

jake'in şifresiyle giriş yapıyorum.

![nine-4](.Images/nine-4.png)

******

Jake olarak giriş yaptıktan sonra resmin içinden bulduğum şifreyle holt kullanıcısına geçiş yapıyorum ve holt kullanıcısının sudo yetkisiyle nano'yu kullanabildiğini görüyorum.

![nine-5](.Images/nine-5.png)

Nano'yu sudo yetkisiyle kullanarak sudoers dosyasını editliyorum ve holt kullanıcısına ALL yetkisi veriyorum.

![nine-6](.Images/nine-6.png)


