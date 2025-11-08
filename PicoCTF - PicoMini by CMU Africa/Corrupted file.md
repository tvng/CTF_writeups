# Corrupted File
* Difficulty : easy
* Tag : Forensics
* Author : Yahaya Meddy

For this challenge I used PicoCTF's webshell.

> Do you know about **file signatures** aka **magic numbers** ?


## First inspection

I started with the Linux command `file` on our file "file" as a basic inspection to verify the actual type of the file :

    file file

Which gives us

> file: data

Hum... so nothing was detected. It seems or file is corrupted, just like the challenge's description says. It also says 

> Maybe a couple of bytes could make all the difference.

Let's inspect the file content with `less` :

    less file

We can see that the file is quite unreadable for a human, except that it begins with 

>  \x<FF><E0>^@^PJFIF^@^ ...

[JFIF](https://en.wikipedia.org/wiki/JPEG_File_Interchange_Format) means JPEG File Interchange Format !

## Magic Number

 So the file was originally a JFIF and was corrupted. How can we transform the file to make it recognizable as a JFIF ? The answer is `file signature`. Those are a few bytes usually put at the beginning of a file and used to identify the content of a file. 

The magic numbers for JFIF are, in hex, `FF D8 FF E0`.

Let's examine the file in hex. For this, I'm using vim, but feel free to use a hex editor online like [hexed.it](hexed.it).

    vim file
    :%!xxd (in vim)

The file starts with `5c78 ffe0`, so let's edit it to match the JFIF file signature, then save the file.

    :%!xxd -r (in vim)

We can run `file` again to check if it worked :

    file file

> file: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 800x500, components 3

Yes ! Once we download the image and check it's content, we can see the flag.

And voilà ! Thank you for reading ! 🤓
