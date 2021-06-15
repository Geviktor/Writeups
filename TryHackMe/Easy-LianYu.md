# Lian Yu

[<img src=".Images/lian.jpeg" height="199">](https://tryhackme.com/room/lianyu)

*"A beginner level security challenge"* -[Deamon](https://tryhackme.com/p/Deamon)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

Nmap taraması yaparak başlıyorum.

![lian-1](.Images/lian-1.png)

Cihazda: 21 portunda FTP, 22 portunda SSH, 80 portunda HTTP, 111 ve 45796 portlarında ise RPC servisi çalışıyor. Nmap çıktısında FTP servisine anonymous olarak giriş yapmanın kısıtlı olduğunu görebiliyoruz. Web sayfasına bakarap başlamaya karar veriyorum. Karşılaştığım index sayfasında kullanıcı adı olabilecek bir kaç şey dışında ilgimi çeken bir şey olmuyor ve gobuster ile dizin taraması yapmaya başlıyorum. Taramanın sonucunda `/island` dizinini buluyorum. Burada `vigilante` kodunu buluyorum fakat bunun ne kodu olduğu hakkında hiç bir fikrim yok. Belki önceki sayfadaki kullanıcı adları için bir SSH şifresi veye önceki sayfada bulunan arkaplan resminin içindeki dosyayı extract etmek için gereken şifre olabilir. Arkaplanda bu dizinin alt dizinleri için bir gobuster taraması daha başlattıktan sonra olası kullanıcı adları için bu şifreyi deniyorum. Başarıya ulaşamıyorum aynı şekilde arkaplanda gördüğümüz resim için de şifreyi denemek istiyorum fakat bu resim png formatında olduğu için steghide tool'unu üzerinde çalıştıramıyorum. Sonrasında ftp servisine bağlanmak için bu şifreyi denerken "vigilante"nin bir kullanıcı adı olduğunu fark ediyorum. Bu kullanıcının şifresini bulabilmek için hydra ile bruteforce saldırısı yapmaya başlıyorum. Bu sırada gobuster'ın `/2100` dizinini bulduğunu görüyorum. Dizine gittiğimde ise kaynak kodunda `.ticket`ın geçtiğini görüyorum ve ticket extension'lı dosyaları bulacak bir gobuster taraması daha başlatıyorum. Bu taramanın sonucunda da `green_arrow.ticket` dizinine ulaşabiliyorum. Burada bir şifre olabileceğini düşündüğüm metin ile karşılaşıyorum. Bunu kullanarak ftp servisine giriş yapmaya çalışıyorum fakat başramıyorum. Bulduğum metinin şifrelendiğini düşünerek çözmek için bir çok yol deniyorum ve sonunda Base58 ile anlamlı sayılabilecek bir metine ulaşıyorum. Bunun ile ftp servisine bağlanmaya çalıştığımda giriş yapabiliyorum.

![lian-2](.Images/lian-2.png)

******

## [Gain Shell]

Buradan aldığım aa.jpg bir jpg dosyası olduğu için bunun içinde şifreyle çıkarılabilecek dosyalar olduğunu ve bu şifrenin de diğer resimlerin içine gizlendiğini düşünüyorum be "Queen's_Gambit.png"ten bakmaya başlıyorum. Bu görselin içinde herhangi bir şey bulamıyorum. "Leave_me_alone.png" dosyasına geçiyorum ve bu dosyanın png uzantısında olmasına rağmen png dosyası olmadığını görüyorum. Dosyanın hex kodlarını vim aracılığıyla `:%!xxd` komutuyla hex düzenleme moduma geçerek png dosyasının olması gerek değerlerini [bulduğum sitedeki](https://www.garykessler.net/library/file_sigs.html) hale getirerek düzenliyorum. Sonunda görseli açtığımda şifreyi buluyorum. Bu şifre ile aa.jpg dosyasının içindeki dosyayı çıkarıyorum ve çıkan zip'i de açtığımda slade kullanıcısının şifresini buluyorum.

![lian-3](.Images/lian-3.png)

Bu şifre ile slade olarak giriş yapabiliyorum.

![lian-4](.Images/lian-4.png)

******

## [Privilege Escalation]

Giriş yaptığımız kullanıcının sudo yetkisini kontrol ediyorum ve pkexec için sudo yetkisi olduğunu görüyorum. [GTFOBins](https://gtfobins.github.io/) aracılığıyla pkexec'i sudo yetkisiyle kullanarak nasıl root kullanıcıya geçebileceğimi öğrenip uyguluyorum.

![lian-5](.Images/lian-5.png)
