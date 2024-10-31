# Mob Psycho

Download the provided file which is an .apk file.
According to the tips, i should've unzip it first and then i can find for the string,

but the first thing i tried is using strings just in case i can find it.

```
#strings mobpsycho.apk | grep "flag"

res/color/flag.txtUT
res/color/flag.txtUT
```

Apparently i found 2 files named flag inside /res/color. I proceed to unzip it and check the file's content.
Its a string which looks like some hexcodes. Converted it to ASCII and yep, its the flag.

I checked the file type and turns out its not a .pdf but a .png

**FLAG: picoCTF{f1u3n7_1n_pn9_&_pdf_7f9bccd1}**
