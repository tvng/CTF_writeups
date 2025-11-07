# Hidden in plainsight
* Difficulty : easy
* Tag : Forensic
* Author : Yahaya Meddy

This challenge is about finding a flag within a JPG named `img.jpg`. Being marked as "easy", the flag shouldn't be hidden too deep. Let's start with some fundamental checks !

For this challenge I used PicoCTF's webshell.

## First inspection

I started with the Linux command `file` as a basic inspection to verify the actual type of file.

    file img.jpg

Which gives us :

    img.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, comment: "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9", baseline, precision 8, 640x640, components 3

The file is a proper JPG. 

I tried to open it, so make some visual checks just in case. I'd be looking for some artifacts, but nothing was off on the actual image. It looked like a proper, untouched image. At least, to human eyes.

But it seems that there is a cryptic `comment` right here... Looks like Base64 to me. Let's check out with the following command :

    base64 -d <<< c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
 
It gives us `steghide:cEF6endvcmQ=`.

Nested encoding, alright :
    
    base64 -d <<< cEF6endvcmQ=

It gives us `pAzzword`. Surely, it's a password for something, and probably not the flag itself. Maybe the lock is `steghide` ?

It seems indeed that `steghide` is a tool to perform image and audio steganography. With a little research on how to use this tool, I tried the following command to extract hidden data from out image file, and entered the password when asked.

    steghide extract -sf img.jpg

It resulted in a `flag.txt` file containing our flag !!

And voilà ! Thank you for reading ! 🤓


