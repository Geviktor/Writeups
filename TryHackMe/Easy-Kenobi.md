# Kenobi

[<img src=".Images/kenobi.png" height="199">](https://tryhackme.com/room/kenobi)

*"Walkthrough on exploiting a Linux machine. Enumerate Samba for shares, manipulate a vulnerable version of proftpd and escalate your privileges with path variable manipulation. "* -[tryhackme](https://tryhackme.com/p/tryhackme)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması yaparak başlıyorum. Daha ayrıntılı ve kolay olduğunu düşündüğüm için öncelikle T4 hızında bütün portları tarıyorum.

`nmap -T4 -vv -p- <IP>`

Daha sonra ise çıkan portları "-A" parametresi ile tarayarak ayrıntılı sonuçlar ediniyorum.

`nmap -A -p <PORTS> -oN kenobi.nmap <IP>`

![kenobi-1](.Images/kenobi-1.png)

Cihazda: 21 portunda FTP, 22 portunda SSH, 80 portunda HTTP, 111 portunda NFS, 139 ve 445 portunda ise Samba servisleri çalışıyor.

Nmap taramasında ftp için "Anonymous allowed" çıktısını vermemesine rağmen anonymous olarak ftp'ye giriş yapmayı denedim. Başaramadım.

![kenobi-2](.Images/kenobi-2.png)

Giriş yapmayı başaramasam da kullanıcıların giriş yapması için email adreslerini şifre olarak kullandıklarını öğrenmiş oldum. 111 portunda çalışan NFS servisinin paylaştığı dosyaları görmek için "showmount" komutunu kullanıyorum.

`showmount -e <IP>`

![kenobi-3](.Images/kenobi-3.png)

Görüldüğü gibi "/var" dizininin paylaşıldığı görüyorum ve bu dizinin altında hem ftp dosyaları hem de 80 portundaki siteye ait dosyalar bulunabileceğini umduğum için bilgisayarımdaki "/tmp" dizini altında bir dizin yaratıp oraya mount ediyorum.

![kenobi-4](.Images/kenobi-4.png)

Beklediğim gibi ftp dosyalarını bulamıyorum fakat 80 portunun içindeki dosyaları görebiliyorum. Hem 80 portunun içindeki dosyalara hem /var dizinindeki diğer dosyalara göz gezdirdikten sonra işime yarayabilecek pek bir şey bulamıyorum. Hala bakabileceğimiz samba servisi olduğu için 80 portundaki görsellere stenografi yapmayı daha sonraya bırakıyorum.

![kenobi-5](.Images/kenobi-5.png)

Samba servisinde paylaşılan dosyaları "smbclient"i kullanarak buluyorum ve bağlanabileceğim "anonymous" dizinine yine smbclient ile bağlanıyorum.

![kenobi-6](.Images/kenobi-6.png)

******

## [Gain Shell]

Burada bulduğum log.txt'yi ve mount ettiğimiz /var/log dizinindeki log'ları inceledim fakat "kenobi" isminde bir kullanıcı olduğu ve bu kullanıcının "/home/kenobi/.ssh/id_rsa" dizininde ssh bağlantısı için kullanabileceğim key'i olduğu bilgisinden başka bir bilgiye ulaşamadım. Burada biraz takıldıktan sonra nmap çıktısında çalışan programların versiyonlarında zafiyet olup olmadığını kontrol etmeye karar verdim. ProFTPD 1.3.5 versiyonu için bir [exploit](https://www.exploit-db.com/exploits/36742) bulabildim. Dosyaları makine içinde bir yerden başka bir yere kopyalamaya yarayan bu zafiyet ile id_rsa dosyasını benim okuyabildiğim /var dizini içinde herhangi bir yere kopyalayabileceğimi anladım. /var dizini içinde bir çok yerde "Permission denied" ile karşılaştım fakat /var/tmp dizini içine kopyalama işlemini yapabildim.

![kenobi-7](.Images/kenobi-7.png)

Kenobi kullanıcısı olarak giriş yapabiliyoruz. Kullanıcının sudo yetkisi olmasına rağmen şifresini bilmediğimiz için kullanamıyoruz.

![kenobi-8](.Images/kenobi-8.png)

******

## [Privilege Escalation]

Yetki yükseltebilmek için makinede sırasıyla, sudo yetkisini - SUID dosyalarını - cron job'ları kontrol ediyorum. SUID dosyalarını kontrol ederken "/usr/bin/menu" isminde bir dosyayla karşılaşıyorum.

`find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null`

Dosyayı biraz incelediğimde ve strings komutuyla içindeki okunabilir çıktılara baktığımda curl, uname ve ifconfig binary'lerini kullandığını anlıyorum. Bu dosya SUID yetkisiyle çalıştığı için dosya içinde alabileceğimiz bir shell'in root yetkisine sahip olacağını umarak, çalıştırılan üç binary dosyası yerine kendi dosyamı kullanmayı deniyorum. Makine üzerinde `echo $PATH` komutuyla /home/kenobi/bin dizininde bulunan dosyaların daha önce okunacağını öğreniyorum. /home/kenobi/bin dizinini oluşturup içinde "ifconfig" binary'sini oluşturuyorum ve menu binary'sini çalıştırıyorum.

![kenobi-9](.Images/kenobi-9.png)


