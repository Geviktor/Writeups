# Bounty Hacker

[<img src=".Images/bounty.jpeg" height="199">](https://tryhackme.com/room/cowboyhacker)

*"You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!"* -[Sevuhl](https://tryhackme.com/p/Sevuhl)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması yaparak başlıyoruz.

![bounty-1](.Images/bounty-1.png)

Chihazda: 21 portunda FTP, 22 portunda SSH, 80 portunda ise HTTP servisleri çalışıyor.

Nmap taramasında gördüğümüz gibi FTP servisi anonymous erişime izin veriyor. Bu servise anonymous olarak bağlantı kurup işimize yarayabilecek şeyler bulmaya çalışabiliriz.

![bounty-2](.Images/bounty-2.png)

Burada lock.txt ve task.txt olmak üzere iki adet dosya buluyoruz. "task.txt" dosyasının içinde kullanıcı adı olabilecek "lin" ismini buluyoruz. "lock.txt" dosyası ise olası şifreleri bulunduran bir wordlist gibi duruyor. Bu wordlist'i kullanarak web servisinde bulduğumuz bir admin paneline veya direkt olarak açık olan SSH servisine "lin" kullanıcı adı ile giriş yapmayı deneyebiliriz. 

******

## [Gain Shell]

İçinde 26 adet şifre olduğu ve denemesi çok kısa süreceği için hydra aracılığıyla SSH servisine bruteforce saldırısı yapmayı deniyorum.

`hydra -l lin -P locks.txt 10.10.253.121 ssh`

![bounty-3](.Images/bounty-3.png)

Lin kullanıcısı için bulduğumuz şifreyi kullanarak SSH aracılığıyla giriş yapıyorum.

## [Privilege Escalation]

Giriş yaptığım kullanıcının sudo yetkisi olup olmadığını `sudo -l` komutu ile kontrol ediyorum ve /bin/tar binary'si için sudo yetkimiz olduğunu görüyorum. [GTFOBins](https://gtfobins.github.io/gtfobins/tar/)'in yardımıyla nasıl root kullanıcıya geçebileceğimi öğreniyorum.

![bounty-4](.Images/bounty-4.png)
