# MSB

In this challenge there is an image which said to be corrupt but it looks like i can open it just fine.
I checked using tools like exiftool, pngcheck, etc. and it seems like a normal .png image.

Looking at the name of the challenge, MSB could be a clue which means most significant byte.
I then search some tools online to find the MSB from an image, and i found one named sigBits.

I run the tool and found something that looks like a whole page from a book.

```
#python sigBits.py -t=msb ninja.png
```

I tried using grep and ack on the output file but found nothing but in a weird way, when i grep for "picoCTF" it prints out the whole content of the output file.
And finally i tried using nano to search for the flag.

**FLAG: picoCTF{15_y0ur_que57_qu1x071c_x071c_0r_h3r01c_06326238}**
