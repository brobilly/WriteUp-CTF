# Write-Up PicoCTF

## Verify

First download the **challenge.zip** file and unzip it using the command:

```
#unzip challenge.zip
```

Cd into the directories and we will find a **checksum.txt** file, decrypt.sh and a folder named **files** containing the encrypted files

Check the sha256 hash inside **checksum.txt**

```
#cat checksum.txt

5848768e56185707f76c1d74f34f4e03fb0573ecc1ca7b11238007226654bcda
```

Now check the sha256sum of the files inside the folder **files** and check for a matching hash

```
#sha256sum files/* | grep "5848768e56185707f76c1d74f34f4e03fb0573ecc1ca7b11238007226654bcda"

5848768e56185707f76c1d74f34f4e03fb0573ecc1ca7b11238007226654bcda  files/8eee7195
```

After finding the right one, use **decrypt.sh** to decrypt it. But apparently, if you're doing it on your local machine, the script would be broken so in my case i decided to ssh into the instance and do the same steps there


**FLAG: picoCTF{trust_but_verify_8eee7195}**



