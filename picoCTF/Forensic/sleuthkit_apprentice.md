# Sleuthkit Apprentice

Unzip the downloaded .gz file to get the .img file. We can use sleuthkit to analyze the disk image but i prefer using autopsy for its gui.

```
#gzip -d disk.flag.img.gz
```

Run autopsy and open it on browser
```
#autopsy
```

Create a new case and add the .img file, and start to check each partition. In my case i head straight to the fourth partition and went to /root and found the flag there.


**FLAG: picoCTF{by73_5urf3r_3497ae6b}**
