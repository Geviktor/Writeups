# Anonforce

[<img src=".Images/anonforce.jpeg" height="199">](https://tryhackme.com/room/bsidesgtanonforce)

*"boot2root machine for FIT and bsides guatemala CTF* -[stuxnet](https://tryhackme.com/p/stuxnet)

1. [Scan/Enumeration](#scan/enumeration)
2. [User](#user)
3. [Root](#root)

******

## [Scan/Enumeration]
We can start by doing an nmap scan.

`nmap -sVC -T4 -oN anon.nmap <IP>`

![anonforce-1](.Images/anonforce-1.png)

As you can see, Anonymous FTP login allowed. Let's try to connect to the FTP service. USERNAME: anonymous / PASSWORD: blank

`ftp <IP>`

![anonforce-2](.Images/anonforce-2.png)

******

## [User]
It looks like we are connected to the / directory of the linux system via FTP. We can try going into the /home directory and looking at the user's files.

![anonforce-3](.Images/anonforce-3.png)
![anonforce-4](.Images/anonforce-4.png)

We found the user.txt file in the home directory of the user named melodias. You can download this file via FTP with the "get" command and read it in your current directory.

******

## [Root]
If you have some knowledge of the linux filesystem, you may notice that there is a directory in the / directory that should not be.

![anonforce-5](.Images/anonforce-5.png)

When you check the directory, you will see a pgp encrypted file and pgp private key. We can get both files using the "get" command.

![anonforce-6](.Images/anonforce-6.png)

We need the password of the private.asc file to be able to read the backup.pgp file. We can crack the private.asc file using the john tool.

`gpg2john private.asc > crackme`
`john crackme`

![anonforce-7](.Images/anonforce-7.png)

Now that we know the password, we can import the key.

`gpg --import private.asc`

![anonforce-8](.Images/anonforce-8.png)

Now we can open the file.

`gpg --decrypt backup.pgp`

![anonforce-9](.Images/anonforce-9.png)

This looks like a shadow file. We can use the hash here to crack the root user's password. First, we need to create a file and write the root user's hash into the file. We can use the HashCat tool to crack the hash, look at the [hashcat wiki](https://hashcat.net/wiki/doku.php?id=example_hashes) to determine the type of hash.

`echo 'root:$6$07nYFaYf$F4VMaegmz7dKjsTukBLh6cP01iMmL7CiQDt1ycIm6a.bsOIBp0DwXVb9XI2EtULXJzBtaMZMNd2tV4uob5RVM0:18120:0:99999:7:::' > hash`
`hashcat -m 1800 hash <wordlist.txt>`

![anonforce-10](.Images/anonforce-10.png)

We found the root password. Let's try to connect to the SSH service with this password. 

`ssh root@<IP>`

We are in, we can read the root.txt

![anonforce-11](.Images/anonforce-11.png)
