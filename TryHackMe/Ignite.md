# Ignite

[<img src=".Images/ignite.png" height="199">](https://tryhackme.com/room/ignite)

*"A new start-up has a few issues with their web server."* -[DarkStar7471](https://tryhackme.com/p/DarkStar7471)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması ile başlıyorum.

![ignite-1](.Images/ignite-1.png)

Cihazda yalnızca 80 portunda HTTP servsisi çalışıyor. Web sitesine girildiği gibi "Fuel CMS Version 1.4" yazısıyla karşılaşıyorum ve bu versiyon için exploit aramaya başlıyorum. 

******

## [Gain Shell]

RCE için yazılmış bir [exploit](https://www.exploit-db.com/exploits/47138) buluyorum ve dosyayı indirip url kısmını düzenledikten sonra www-data olarak komut çalıştırabiliyorum fakat çıktıları temiz bir şekilde alamadığım için buradan reverse shell için komut çalıştırıp nc ile yakalıyorum.

![ignite-2](.Images/ignite-2.png)

******

## [Privilege Escalation]

www-data kullanıcısı ile `sudo -l` komutunu çalıştırdım fakat şifresiz çalıştırabileceği herhangi bir komut olmadığı için bir sonuç alamadım. SUID dosyalarını kontrol etmeye çalıştım fakat çalıştırdığım komutdan uzun süre çıktı alamamam nedeniyle shell'i kırıp yeniden bağlandım. Cronjob olarak da herhangi bir şey bulamadığımdan linpeas'i cihaza atıp onun sonuçlarına bakma kararı aldım. Fakat linpeas çıktısından da istediğim sonuçları alamadım ve sistemi daha ayrıntılı incelemeye başladım. Web sitesinin dosyalarının arasında fuel'in içinde /application/config dizininde bulunan database.php dosyasına geldiğimde dosyayı okuduğum gibi bir root şifresi ile karşılaştım. Bu şifreyi kullanarak root kullanıcısına geçiş yapabildim.

![ignite-3](.Images/ignite-3.png)
