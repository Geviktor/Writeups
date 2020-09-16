# Wargames / Bandit

["The Bandit wargame is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames."](https://overthewire.org/wargames/bandit/)

### [LEVEL 0]
For this level, we need to connect to the machine with SSH.

`ssh <username>@<ip/DNS> -p 2220`

![Bandit0](.Images/bandit0.png)

Now, we are in. We can read the readme file with [cat](https://www.cyberciti.biz/faq/howto-use-cat-command-in-unix-linux-shell-script/).

`cat <filename>`

![Bandit0-1](.Images/bandit0-1.png)

(To go to the next level, type "exit" and connect again with SSH as bandit1. We'll repeat this for every level.)

### [LEVEL 1]
In this level, we need read the ["-"](https://www.google.com/search?q=dashed+filename) file.

`cat < -` OR `cat ./-`

![Bandit1](.Images/bandit1.png)

### [LEVEL 2]
We need read the ["spaces in this filename"](https://linoxide.com/linux-command/how-to-read-filename-with-spaces-in-linux/).

`cat <file\ name\>` OR `cat <"file name">`

![Bandit2](.Images/bandit2.png)
(Actually autocomplete can be used with tab key.)

### [LEVEL 3]
We need find a hidden file with [ls](https://www.rapidtables.com/code/linux/ls.html).

![Bandit3](.Images/bandit3.png)

### [LEVEL 4]
We need find human-readable text in 10 files. I will do this with ["strings"](https://www.howtogeek.com/427805/how-to-use-the-strings-command-on-linux/) command.

`strings <file>`

![Bandit4](.Images/bandit4.png)

### [LEVEL 5]
In this challenge we can use [find](https://www.man7.org/linux/man-pages/man1/find.1.html) command for file [size search](https://linuxconfig.org/how-to-use-find-command-to-search-for-files-based-on-file-size).

`find <directory> <flags>`

![Bandit5](.Images/bandit5.png)




