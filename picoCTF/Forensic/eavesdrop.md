# Eavesdrop

Open wireshark and add the provided **.pcap** file

After following the tcp stream, there's some converastion inside it and it said something about a file being trasfered on port 9002

```
Hey, how do you decrypt this file again?
You're serious?
Yeah, I'm serious
*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
Ok, great, thanks.
Let's use Discord next time, it's more secure.
C'mon, no one knows we use this program like this!
Whatever.
Hey.
Yeah?
Could you transfer the file to me again?
Oh great. Ok, over 9002?
Yeah, listening.
Sent it
Got it.
You're unbelievable
```

So i tried to filtered it to port 9002 only, and i found something that might be the file from the conversation but im not really sure.

I proceed to try using binwalk and found something encrypted after the address 5882.

I used dd to split the encrypted data from the **.pcap** file

```
#dd if=capture.flag.pcap of=file.des3 bs=1
```

i named the output file the same as from the conversation which is **file.des3** just to make it easier, and now after the encrypted data has been taken, i decrypt it using the command specified from the conversation

```
#openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword1
```
After that i cat the file but i dont see any flag, then i tried using strings and i found it.


**FLAG: picoCTF{nc_73115_411_dd54ab67}**
