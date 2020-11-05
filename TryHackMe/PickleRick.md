# Pickle Rick

[<img src=".Images/pickle.jpeg" height="199">](https://tryhackme.com/room/picklerick)

*"A Rick and Morty CTF. Help turn Rick back into a human!"* -[tryhackme](https://tryhackme.com/p/tryhackme)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Öncelikle nmap taraması yaparak başlıyorum.

`nmap -A -oN pickle.nmap <IP>`

![pickle-1](.Images/pickle-1.png)

Taramanın sonucunda, 22 portunda SSH, 80 portunda HTTP servisinin çalıştığını öğrendim. 80 portunda çalışan web sayfasının kaynak kodunu kontrol ettiğimde bir kullanıcı adıyla karşılaştım.

![pickle-2](.Images/pickle-2.png)

Arama motorlarının siteyi indexlemek için kullandığı "robots.txt" dosyasına bakıyorum ve içinde sadece bir kelime ile karşılaşıyorum. Bu kelimenin bulduğumuz kullanıcı adı için şifre olabileceğini düşünerek kaydediyorum.

Sitede daha derinlere inebilmek için gobuster'ı kullanarak dizin tarması yapıyorum.

`gobuster dir -u http://<IP>/ -w mywordlist.txt -x php,txt -o pickle.buster -t 64`

![pickle-3](.Images/pickle-3.png)

******

## [Gain Shell]

Bulduğumuz login.php üzerinden yine daha önce bulduğumuz kullanıcı adını ve şifreyi kullanarak giriş yapıyorum. "portal.php" sayfasına ulaştığımda öncelikle kaynak kodu kontrol ediyorum ve şifrelenmiş bir metin ile karşılaşıyorum. Bunu çözmeyi denedim fakat başaramadım, şu anlık bırakıp sayfanın ne yaptığını incelemeye karar veriyorum.

Sayfa benden aldığı input'u makinede çalıştırıyor ve output'u bana geri döndürüyor. Fakat bir kaç denemeden sonra anlıyorum ki bir kaç komut yasaklı. Bir dosyayı okumak için "cat" komutunu yasaklı olduğundan ötürü kullanamıyorum ben de dosya okuma işlemi için "dd" komutunu kullanıyorum.

`dd if=filename`

Ayrıca sistemde home dizininde "rick" isimli bir dizinle karşılaşıyorum. Doğal olarak bunun bir kullanıcı olduğunu düşünüyorum fakat daha sonra bu dizinin root tarafından oluşturulduğunu ve rick isimli bir kullanıcı olmadığını anlıyorum.

![pickle-4](.Images/pickle-4.png)

******

## [Privilege Escalation]

Bir kaç enumarete denemesinden sonra bulunduğum kullanıcı olan www-data kullanıcısıyla `sudo -l` komutunu çalıştırarak kullanıcının sudo yetkisi olduğunu öğreniyorum. Makinedeki root dizininde bulunan .ssh dizininin içindeki authorized_keys dosyasına kendi ssh public keyimi yazmaya çalışıyorum fakat yasaklı bir komut içerdiği için engelleniyorum. Bunun üzerine makinemde netcat reverse shell'i içeren bir .sh dosyası oluşturup wget ile makinenin /tmp dizinine yüklüyorum. Dosyayı executable yaptıktan sonra sudo yetkisi ile çalıştırarak root olarak reverse shell alabiliyorum.

![pickle-5](.Images/pickle-5.png)
