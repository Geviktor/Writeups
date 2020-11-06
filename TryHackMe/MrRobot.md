# Mr. Robot CTF

[<img src=".Images/robot.jpeg" height="199">](https://tryhackme.com/room/mrrobot)

*"Based on the Mr. Robot show, can you root this box?"* -[stuxnet](https://tryhackme.com/p/ben)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması yaparak başlayabiliriz.

`nmap -A -oN robot.nmap <IP>`

![robot-1](.Images/robot-1.png)

Yalnızca web portlarının açık olduğunu görebiliyoruz. 80 portuna browser aracılığıyla bağlanıp biraz bakındıktan sonra, arama motorlarının sitede yolunu bulmak için kullandıkları "robots.txt" dosyasına bakıyorum ve 2 adet dosya ile karşılaşıyorum. Bu dosyalardan biri odanın bizden istediği key'lerden ilkini içerirken diğeri 858160 kelime içeriyor. Bulduğumuz wordlist'i bir bruteforce saldırısında kullanacağımızı düşünerek içinde birden fazla kez yazılan kelimeleri çıkarıp alfabetik olarak sıralıyorum.

`sort -u <FILE> > <NEWFILE>`

![robot-2](.Images/robot-2.png)

Siteye gobuster taraması yaparak "/wp-admin" sayfasındaki login page'i buluyorum. Öncelikle olası bir kaç default cred'ler ile giriş denemesi yapıyorum. Başaramıyorum ama kullanıcı adlarına "user is not here" cevabını verdiğini görerek sistemde var olan bir kullanıcı adı girdiğimde bu cevabı almayacağımı anlıyorum. Olası kullanıcı adlarını bulmak için "Mr Robot" isimli dizinin IMDb sayfasından yararlanarak "elliot" kullanıcı adının sisteme kayıtlı olduğunu öğreniyorum. Wpscan aracılığıyla düzenlediğimiz wordlist'i kullanarak bruteforce saldırısına başlıyorum.

`wpscan --url http://<IP> -P <wordlist> --usernames elliot`

![robot-3](.Images/robot-3.png)

## [Gain Shell]

Wordpress'e giriş yaptıktan sonra appearance menüsündeki 404 template'i php reverse shell olarak ayarlayıp deamon olarak shell alıyorum.

![robot-4](.Images/robot-4.png)

Home dizini altında bulunan kullanıcıların dosyalarına bakarken robot kullanıcısının şifresinin md5 halinin olduğu bir dosya buluyorum.

![robot-5](.Images/robot-5.png)

Bu hash'i kırmak için hashcat ve rockyou.txt'yi kullanmadan önce online olarak hash'i kırmaya çalışan [sitelerden](https://crackstation.net/) birini deniyorum ve hashcat'e gerek kalmadan şifreyi alabiliyorum. Aldığım shell'de kullanıcı değiştirebilmek için python'u kullanarak /bin/bash'e geçiyorum ve robot kullanıcısına geçiş yapabiliyorum.

![robot-6](.Images/robot-6.png)

## [Privilege Escalation]

Öncelikle kullanıcının şifresine sahip olduğumuz için sudo yetkisini kontrol ediyorum fakat herhangi bir şeyde sudo kullanamadığımızı öğreniyorum. İkinci olarak da SUID dosyalarını kontrol ediyorum ve nmap'in SUID dosyaları arasında olduğunu görüyorum. [GTFOBins](https://gtfobins.github.io/) aracılığıyla SUID dosyası olan nmap'i kullanarak nasıl yetki yükseltebileceğimi öğreniyorum.

![robot-7](.Images/robot-7.png)
