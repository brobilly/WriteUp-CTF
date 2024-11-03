# Trivial Flag Transfer Protocol

Open up wireshark and load the .pcapng file

Judging by the name of the challenge, it is about tftp which basically just simple ftp without any security
I started with filtering it to TFTP only, analyze it for a while and it seems pretty normal, there are some files being transfered.

I proceed to go to **file > export project > tftp**, and export all the files.

```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# ls 
instructions.txt  picture2.bmp  plan         tftp.pcapng
picture1.bmp      picture3.bmp  program.deb

```
There are 6 files in total. According to the packets i analyzed earlier, **instructions.txt** is the first file that was transfered.

```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# cat instructions.txt 
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
```

After checking it on dcode, it seems that line of string is encoded using ROT-13 so i proceed to decode it using some decoder online.

```
TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN

TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN
```

Hmmmmm, next lets try the **plan** file

```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# cat plan            
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
```

```
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS

I USED THE PROGRAM AND HID IT WITH - DUE DILIGENCE. CHECK OUT THE PHOTOS
```

I checked the photo but all of it seems normal except for **picture2.bmp**. The size seems a bit too big.
Used binwalk on it and found some compressed and random files, yet it doesnt look related to the challenge.

Reading the **plan** again, it seems the author used program.deb to encrypt or hide something.


```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# dpkg -i program.deb
```

I ran the command above to install it but its error, but now i know that its trying to install steghide, so then i install it myself using apt install.
Next, i tried to extract the hidden data on **picture2.bmp** using steghide but it seems it needs a passphrase. Im pretty convinced that the pass is DUEDILIGENCE, but i was trying to find the right format such as do i use capital letters, or the spaces.

Eventually i found it but its apparently on **picture3.bmp**.

```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# steghide extract -sf picture3.bmp -p DUEDILIGENCE
wrote extracted data to "flag.txt".
``` 

**FLAG: picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}**
