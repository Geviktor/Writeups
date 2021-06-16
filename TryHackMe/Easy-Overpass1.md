# Overpass

[<img src=".Images/overpass.png" height="199">](https://tryhackme.com/room/overpass)

*'What happens when some broke CompSci students make a password manager?'* - [NinjaJc01](https://tryhackme.com/p/NinjaJc01)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******
## [Scan/Enumeration]

Let's start with nmap scan.

[overpass-1](.Images/overpass-1.png)

We have SSH on 22 and HTTP on 80. I'm checking web pages source codes. In index.html source codes I find this comment: 

`<!--Yeah right, just because the Romans used it doesn't make it military grade, change this?-->`

So I think they are probably using 'Ceaser Cipher'. We have a /download directory with some files, I decide to leave them here to look at them later. Other directory is /aboutus, in this directory i can't find anything but possible usernames. I also check main.js but it's only have 'hello world!'. So I'm doing gobuster scan.

[overpass-2](.Images/overpass-2.png)

## [Gain Shell]

We found /admin directory. In this directory source codes we can see login.js file and when we look at that we can see: if our cookie is "Incorrect credentials" we cant log in but else it is set our 'SessionToken' to our value and we can log in. So I'm open my browser developer tools and set a 'SessionToken' with 'gev' value. After that I refresh the page and I'm logged in.

[overpass-3](.Images/overpass-3.png)

Now we have encrypted id_rsa key. We can crack it with john and rockyou.txt.

[overpass-4](.Images/overpass-4.png)

For use this id_rsa we need set privilege with `chmod 600 id_rsa`. Now we know the password for id_rsa but we don't know this file is which user's. I'm trying all name on the about page but no one is work and I try to use the name on the password...

[overpass-5](.Images/overpass-5.png)

## [Privilege Escalation]

In james home directory, we have a todo file and its inculudes this: `Write down my password somewhere on a sticky note so that I don't forget it. Wait, we make a password manager. Why don't I just use that?`. When we do `ls -la` on james directory we can see .overpass file and now we know this file have james password and we can use this password for sudo privilege. We knew that, they are using 'Ceaser Cipher'. This cipher means 'It is a type of substitution cipher in which each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.' Also usually this cipher is used as ROT13 and variants today. I'm taking this text and try to solve with two most popular ROT variants: ROT13 and ROT47. And the ROT47 is solve.

[overpass-6](.Images/overpass-6.png)

When we try to `sudo -l` command with this password we see we don't have sudo privilege. LOL. So let's check suid files.

`find / -perm -4000 2>/dev/null`

Nothing interesting. I check again todo.txt and i see james talking about some automated scripts. It can be a cron job, and we can see it `/etc/crontab`.

`* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash`

This mean, every seconds root user take overpass.thm/downloads/src/buildscript.sh and run it with bash. If we can edit this buldscript.sh we can gain shell as root.But there is no www directory in the /var directory so can be this server in /root directory and we cant access that. But as you can see it is using overpass.thm hostname for curl command so on linux system we can edit hosts in /etc/hosts file. Before that I create downloads/src/buildscript.sh with reverse shell and open a server with python. When i edit the /etc/hosts file:

[overpass-7](.Images/overpass-7.png)


