# Tartarus

[<img src=".Images/tartarus.png" height="199">](https://tryhackme.com/room/tartaraus)

*"This is a beginner box based on simple enumeration of services and basic privilege escalation techniques."* -[csenox](https://tryhackme.com/p/csenox)

1. [Scan/Enumeration](#scan/enumeration)
2. [Gain Shell](#gain-shell)
3. [Privilege Escalation](#privilege-escalation)

******

## [Scan/Enumeration]

We can start by doing an nmap scan.

`nmap -A -oN tartarus.nmap <ip>`

![tartarus-1](.Images/tartarus-1.png)

We have an FTP on port 21, SSH service on port 22 and Apache web server on port 80. The results say "Anonymous FTP login allowed". Let's connect to FTP anonymously. We can download the files with the get command.
User: anonymous
Pass: blank

`ftp <ip>`

![tartarus-2](.Images/tartarus-2.png)

We found a file named test.txt. We can try to navigate inside the shared directory on FTP.

`cd ..`
`cd ...`

![tartarus-3](.Images/tartarus-3.png)

We found another file called yougotgoodeyes.txt. Let's check these files.

![tartarus-4](.Images/tartarus-4.png)

/sUp3r-s3cr3t should be a website directory. Now we can start checking the website. If we look at the /sUp3r-s3cr3t directory, we can see that there is a login page. We can try bruteforce here, but first, let's try looking at the robots.txt file.

![tartarus-5](.Images/tartarus-5.png)

We found /admin-dir directory. And d4rckh can be username.

![tartarus-6](.Images/tartarus-6.png)

Let's download credentials.txt and userid file. The credentials.txt file looks like a password list and the userid file looks like a list of usernames. We can use these files to bruteforce the login page. We will use the hydra tool for bruteforce.

First we need to find out what kind of request we sent. We can go to the / sUp3r-s3cr3t directory, open the Inspect Element section and go to the Network section. Now we can try to login with random credentials. After doing this, we will see a POST request in the Network section. We can right click on this request and click Edit and Resend. Here, the Request Body field at the bottom right of the screen gives the information we need. [These are valid for firefox.](https://bentrobotlabs.wordpress.com/2018/04/02/web-site-login-brute-forcing-with-hydra/)

`hydra -L userid -P credentials.txt <IP> http-post-form "/sUp3r-s3cr3t/authenticate.php:username=^USER^&password=^PASS^:F=Incorrect"`

![tartarus-7](.Images/tartarus-7.png)

## [Gain Shell]

We can now login to the site using this information.


