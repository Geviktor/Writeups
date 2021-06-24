# Wargames / Bandit

[*"The Bandit wargame is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames."*](https://overthewire.org/wargames/bandit/)

0. [Level 0 -> LEVEL 1](#level-0) | 10. [Level 10](#level-10) | 20. [Level 20](#level-20) | 30. [Level 30](#level-30)
1. [Level 1 -> LEVEL 2](#level-1) | 11. [Level 11](#level-11) | 21. [Level 21](#level-21) | 31. [Level 31](#level-31)
2. [Level 2 -> LEVEL 3](#level-2) | 12. [Level 12](#level-12) | 22. [Level 22](#level-22) | 32. [Level 32](#level-32)
3. [Level 3 -> LEVEL 4](#level-3) | 13. [Level 13](#level-13) | 23. [Level 23](#level-23) | 33. [Level 33](#level-33)
4. [Level 4 -> LEVEL 5](#level-4) | 14. [Level 14](#level-14) | 24. [Level 24](#level-24)
5. [Level 5 -> LEVEL 6](#level-5) | 15. [Level 15](#level-15) | 25. [Level 25](#level-25)
6. [Level 6 -> LEVEL 7](#level-6) | 16. [Level 16](#level-16) | 26. [Level 26](#level-26)
7. [Level 7 -> LEVEL 8](#level-7) | 17. [Level 17](#level-17) | 27. [Level 27](#level-27)
8. [Level 8 -> LEVEL 9](#level-8) | 18. [Level 18](#level-18) | 28. [Level 28](#level-28)
9. [Level 9 -> LEVEL 10](#level-9) | 19. [Level 19](#level-19) | 29. [Level 29](#level-29)
******

### [LEVEL 0 -> LEVEL 1]
For this level, we need to connect to the machine with SSH.

`ssh <username>@<IP> -p 2220`

![Bandit0](.Images/bandit0.png)

Now, we are in. We can read the readme file with [cat](https://www.cyberciti.biz/faq/howto-use-cat-command-in-unix-linux-shell-script/).

`cat <filename>`

![Bandit0-1](.Images/bandit0-1.png)

(To go to the next level, type "exit" and connect again with SSH as bandit1. We'll repeat this for every level.)
******

### [LEVEL 1]
In this level, we need read the ["-"](https://www.google.com/search?q=dashed+filename) file.

`cat < -` OR `cat ./-`

![Bandit1](.Images/bandit1.png)
******

### [LEVEL 2]
We need read the ["spaces in this filename"](https://linoxide.com/linux-command/how-to-read-filename-with-spaces-in-linux/).

`cat <file\ name\>` OR `cat <"file name">`

![Bandit2](.Images/bandit2.png)
(Actually autocomplete can be used with tab key.)
******

### [LEVEL 3]
We need find a hidden file with [ls](https://www.rapidtables.com/code/linux/ls.html).

![Bandit3](.Images/bandit3.png)
******

### [LEVEL 4]
We need find human-readable text in 10 files. I will do this with ["strings"](https://www.howtogeek.com/427805/how-to-use-the-strings-command-on-linux/) command.

`strings <file>`

![Bandit4](.Images/bandit4.png)
******

### [LEVEL 5]
In this challenge we can use [find](https://www.man7.org/linux/man-pages/man1/find.1.html) command for file [size search](https://linuxconfig.org/how-to-use-find-command-to-search-for-files-based-on-file-size).

`find <directory> <flags>`

![Bandit5](.Images/bandit5.png)
******

### [LEVEL 6]
Again we can use [find](https://www.man7.org/linux/man-pages/man1/find.1.html) command with required parameters.

`find <directory> -user <user> -group <group> -size <size>`

![Bandit6](.Images/bandit6.png)

(What is [*2>>/dev/null*](https://askubuntu.com/questions/350208/what-does-2-dev-null-mean) and why we use?)
******

### [LEVEL 7]
We will use cat and [grep](https://linux.die.net/man/1/grep) command with ["|(pipe)"](https://www.howtogeek.com/438882/how-to-use-pipes-on-linux/) symbol.

` cat <file> | grep <word>`

![Bandit7](.Images/bandit7.png)
******

### [LEVEL 8]
We will [find unique text](https://stackoverflow.com/questions/13778273/find-unique-lines#13778360) in data.txt with [sort](https://www.man7.org/linux/man-pages/man1/sort.1.html) and [uniq](https://man7.org/linux/man-pages/man1/uniq.1.html) command.

`sort <file> | uniq -u`

![Bandit8](.Images/bandit8.png)
******

### [LEVEL 9]
We can find password with strings and grep command.

`strings <file> | grep <symbol>`

![Bandit9](.Images/bandit9.png)
******

### [LEVEL 10]
In this challenge, the password is encoded with base64. We can decode with [base64](https://www.man7.org/linux/man-pages/man1/base64.1.html) command.

`base64 -d <file>`

![Bandit10](.Images/bandit10.png)
******

### [LEVEL 11]
The password is encoded with rot13. We will decode it with [tr](https://man7.org/linux/man-pages/man1/tr.1.html) command and [this table](https://www.chmag.in/articles/momsguide/decoding-rot-using-the-echo-and-tr-commands-in-your-linux-terminal/).

`cat <file> | tr <parameter>`

![Bandit11](.Images/bandit11.png)
******

### [LEVEL 12]
In this section, we will have to work on the file a bit. So let's create a directory under the [/tmp](https://www.fosslinux.com/41739/linux-tmp-directory-everything-you-need-to-know.htm) directory and copy our file there. We will use [cd](https://man7.org/linux/man-pages/man1/cd.1p.html), [mkdir](https://man7.org/linux/man-pages/man1/mkdir.1.html) and [cp](https://man7.org/linux/man-pages/man1/cp.1.html) command.

![Bandit12](.Images/bandit12.png)

(What does the [~](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html) and [.](https://www.linux.com/training-tutorials/linux-tools-meaning-dot/) do?) 

Now, we need to do reverse hexdump with xxd command.

![Bandin12-1](.Images/bandit12-1.png)

Then we will find the type of file using the [file](https://linux.die.net/man/1/file) command. We will investigate what the extension of this file type should be and add the required extension to the file name with the mv command. If this file is a compressed file (mostly such), we will decompress it with the required command.

![Bandit12-2](.Images/bandit12-2.png)

We will repeat the same things until we reach the text file.

![Bandit12-3](.Images/bandit12-3.png)
******

### [LEVEL 13]
In this section, we are asked to make an [ssh](https://linux.die.net/man/1/ssh) connection using a private key.

`ssh -i <keyfile> <username>@<IP>`

![Bandit13](.Images/bandit13.png)

Now we can read the password file.

![Bandit13-1](.Images/bandit13-1.png)
******

### [LEVEL 14]
We can use [telnet](https://linux.die.net/man/1/telnet) or [netcat](https://linux.die.net/man/1/nc) for this challenge.

![Bandit14](.Images/bandit14.png)
******

### [LEVEL 15]
In this section, the port using [SSL encryption](https://www.digicert.com/ssl/). We will use [openssl](https://linux.die.net/man/1/openssl) command. [Can the telnet or netcat clients communicate over SSL?](https://superuser.com/questions/346958/can-the-telnet-or-netcat-clients-communicate-over-ssl)

`openssl s_client -connect <IP>:<port>`

![Bandit15](.Images/bandit15.png)

![Bandit15-1](.Images/bandit15-1.png)
******

### [LEVEL 16]
We can use [nmap](https://linux.die.net/man/1/nmap) for find ports. We will use [ssl-enum-ciphers](https://nmap.org/nsedoc/scripts/ssl-enum-ciphers.html) script for check SSL.

`nmap --script ssl-enum-chiphers -p 31000-32000 localhost`

According to the result of the scan, 5 ports are open and 2 ports are using SSL.

Lets try normal ports first.

![Bandit16](.Images/bandit16.png)

So the password isn't in these ports. Lets try SSL ports.

`openssl s_client -connect localhost:31790`

![Bandit16-1](.Images/bandit16-1.png)

Yes! that gave us an SSH private key. [We can connect bandit17 with this private key](#level-13).
******

### [LEVEL 17]
We can check the difference between the two files using the [diff](https://www.computerhope.com/unix/udiff.htm) command.

`diff -e <file1> <file2>`

![Bandit17](.Images/bandit17.png)
******

### [LEVEL 18]
In this level we can use [scp](https://tecadmin.net/download-file-using-ssh/) command without connection SSH.

![Bandit18](.Images/bandit18.png)

But also we can do SSH connection with `-t` flag.

`ssh -t <user>@<host> -p <port> /bin/sh`

![Bandit18-1](.Images/bandit18-1.png)
******

### [LEVEL 19]
We can [run binary](https://stackoverflow.com/questions/9477157/how-to-run-binary-file-in-linux) with `./binary` command.

![Bandit19](.Images/bandit19.png)
******

### [LEVEL 20]
By running the binary file, you can find out how it works and what it does.

![Bandit20](.Images/bandit20.png)

While broadcasting bandit20's password from any port, we need to connect to that port using the binary file. We will do this with nc and two ssh connection.

![Bandit20-1](.Images/bandit20-1.png)
******

### [LEVEL 21]
This section is about [cron](https://opensource.com/article/17/11/how-use-cron-linux). If we check the /etc/cron.d/ directory, we will see that there is a file named cronjob_bandit22. We can read it to understand what it does.

![Bandit21](.Images/bandit21.png)

We learned that it runs the file /usr/bin/cronjob_bandit22.sh. When we read this file, we will see that the password of the bandit22 user is written in the file named /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv. We can read this file to learn the password.

![Bandit21-1](.Images/bandit21-1.png)
******

### [LEVEL 22]
We will use cron again. We can check /etc/cron.d again and read the file for bandit23.

![Bandit22](.Images/bandit22.png)

This simple shell script runs the whoami command and takes the md5 summary of its output and creates a file under /tmp. The password of the bandit23 user is written into this file.

![Bandit22-1](.Images/bandit22-1.png)

******

### [LEVEL 23]
Lets check cron.d/cronjob_bandit24

![Bandit23](.Images/bandit23.png)

We can understand bandit24 user running cronjob_bandit24.sh in every second. When we check cronjob_bandit24.sh we see, "Executing and deleting all scripts in /var/spool/$myname" and $myname variable is coming from whoami command so it must be "bandit24". We can write a little script for cat /etc/bandit_pass/bandit24 and write a readable file for us. For this I create a directory under /tmp/. An important thing, you need to do this directory and file permission 777 because bandit24 user can be run this file and write your directory.

![Bandit23-1](.Images/bandit23-1.png)

******

### [LEVEL 24]
We need write a script for this level.

![bandit24](.Images/bandit24.png)

******

### [LEVEL 25]

When we login bandit25 we see that there is a id_rsa file for bandit26 user. As we know from level goal bandit26 user's shell is not /bin/bash. For learn which shell is using we can check passwd file with `cat /etc/passwd | grep bandit26`. We see it's running more command when we read this `/usr/bin/showtext` file.

![bandit25](.Images/bandit25.png)

After try to connect bandit26 with ssh and a lot of google search I realize that when we shrink the terminal we can run command over more. For trigger this I set terminal row size with `stty rows 5` after that, when I connect ssh I see I can run command over more. As I learned I open editor with `v` key. In here I change the shell veriable with `:set shell=/bin/bash` and after run `:!/bin/bash` 

![bandit25-1](.Images/bandit25-1.png)

******

### [LEVEL 26]

I try to run `bandit27-do` binary and it's saying *"Run a command as another user."*. So I'm trying `./bandit27-do cat /etc//etc/bandit_pass/bandit27`

![bandit26](.Images/bandit26.png) 

******

### [LEVEL 27]

I clone the repo to my computer with git using `git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo` command.

![bandit27](.Images/bandit27.png)

******

### [LEVEL 28]

I clone the repo to my computer with git using `git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo` command. In .git folder I use `git log` command to see commits. I found a commit named *'add missing data'* and I use `git show <commit>` for see this commit.

![bandit28](.Images/bandit28.png)

******

### [LEVEL 29]

I clone the repo to my computer with git using `git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo` command. I check git logs again but I cant find anything. I try to list branchs with `git branch -a` and I see that we have several branch in here. I search on google how to use git branchs and I find [this source](https://www.nobledesktop.com/learn/git/git-branches).

![bandit29](.Images/bandit29.png)

******

### [LEVEL 30]

I clone the repo to my computer with git using `git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo` command. I try to check logs and branchs but I cant find anything. I found [this page](https://www.atlassian.com/git/tutorials/inspecting-a-repository) and I see 'secret' tag when I try to `git tag` command. I use `git show secret` command for read this tag.

![bandit30](.Images/bandit30.png)

******

### [LEVEL 31]

I clone the repo to my computer with git using `git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo` command. I see .gitignore file and its ignore \*.txt files. When I read the README, I see we need to do push this repo with key.txt file. I remove the .gitignore file and create key.txt and push this repo.

![bandit31](.Images/bandit31.png)

******

### [LEVEL 32]

In this level when we login bandit32 with ssh, we are in a shell named uppershell. In here I try several things, I login with another bandit and check this shell path, it's in /home/bandit32/uppershell but we can't read it. After a while I realize we can use some things with `$`. We can run '$home', '$pwd', '$path' etc. I try to define variable and call that but I can't. After an eternity later, I decide to check sh man page for learn what can I do with '$'. I search for '$' and I see this: `$0 : zero) Expands to the name	of the shell script if passed on the command line, the name operand if given (with -c) or otherwise argument 0 passed to the shell.`

![bandit32](.Images/bandit32.png)
