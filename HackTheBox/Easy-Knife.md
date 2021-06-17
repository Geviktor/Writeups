# Knife

[<img src=".Images/knife.png" height=199px>](https://app.hackthebox.eu/machines/Knife)

[MrKN16H](https://app.hackthebox.eu/users/98767)

Difficulty: *Easy*

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

## [Scan/Enumeration]

Let's start with nmap scan.

![knife-1.png](.Images/knife-1.png)

We have SSH on 22 and HTTP on 80. I check source codes on the web site but I didn't find anything. I try to find directory with gobuster. I try php,html,txt,db,bak extensions but there is nothing. Lastly I try to check requests and responses and I see this `X-Powered-By: PHP/8.1.0-dev`. So I search PHP/8.1.0-dev for exploit and I find [this](https://www.exploit-db.com/exploits/49933).

## [Gain Shell]

I understand this, we can code execution with User-Agentt header and zerodiumsystem parameter. In firefox developer tools I try to edit and resend `User-Agentt: zerodiumsystem('whoami; id; pwd');`.

![knife-2](.Images/knife-2.png)

I'm looking james user .ssh directory for find an id_rsa. I found but it is not work. So on my machine I create a id_rsa and id_rsa.pub with ssh-keygen after that i add my id_rsa.pub key to target machine in /home/james/.ssh/authorized_keys file. Now i can connect with my own id_rsa key.

## [Privilege Escalation]

When I check james user sudo privilege with `sudo -l` I see we can run /usr/bin/knife. I cat this file and see this is a ruby file. I googled ruby knife and I found [this](https://docs.chef.io/workstation/knife_exec/). I understand we can run a ruby command with `knife exec -E 'command'` and a little bit search I do this little command for make me root: `knife exec -E 'system("/bin/bash")'`

![knife-3](.Images/knife-3.png)
