# Sleuthkit Apprentice

Unzip the downloaded .gz file to get the .img file. We can use sleuthkit to analyze the disk image but i prefer using autopsy for its gui.

```
#gzip -d disk.flag.img.gz
```

Run autopsy and open it on browser
```
#autopsy
```

Create a new case and add the .img file, start analyzing the partition, but in my case i use the search feature on autopsy and searched for "picoCTF" and apparently i found the flag


**FLAG: picoCTF{f0r3ns1c4t0r_n30phyt3_ad5c96c0}**
