# Cap

[<img src=".Images/cap.png" height="199">](https://app.hackthebox.eu/machines/Cap)

[InfoSecJack](https://app.hackthebox.eu/users/52045)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

## [Scan/Enumeration]

Let's start with nmap scan.

![cap-1.png](.Images/cap-1.png)

We have FTP on 21, SSH on 22 and HTTP on 80. I'm trying log in ftp with default credentials (anonymous:anonymous) but it doesn't work. In web page we have capture directory and this directory have several sub directory by number. I download data/1, data/2, data/3 and data/4; and i see these numbers are going to high. I check the downloaded pcap files but they are only me or other users access logs. 

## [Gain Shell]

So I try to reach negative number like data/-1, data/-2, etc. but it doesn't work. When i try reach data/0, I encounter 0.pcap file and i check it. 

![cap-2.png](.Images/cap-2.png)

We found FTP login creds in here. I'm login FTP and checked the home directory but i can't find usefull things. I try to login SSH with same creds and it works.

## [Privilege Escalation]

I try to use sudo but nathan user can't, I check suid files but nothing interesting, I check cronjobs but there is no cronjobs. I try to check capabilities with `getcap -r / 2>/dev/null` and I see we have python as capabilities. With a little search I learn how to exploit it and I do.

![cap-3.png](.Images/cap-3.png)
