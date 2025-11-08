# Flag in Flame
* Difficulty : easy
* Tag : Forensics
* Author : Prince Niyonshuti N.

This challenge is about finding a flag within a log file named `logs.txt`.

For this challenge I used PicoCTF's webshell.

## Decoding

It seems, per the challenge description, that the content of the file is encoded. I'm starting with the Linux command `file` as a basic inspection to verify the actual type of the file.

    file logs.txt

Upon opening the text file with `less`, we see a huge chunk of letters, numbers and 2 special characters : `+` and `/`. Looks like Base64 ! Let's decode and save the result in a new file.

    base64 -d logs.txt > decoded

We should check first what's the type of the decoded data :

    file decoded

It's a PNG !

> decoded: PNG image data, 896 x 1152, 8-bit/color RGB, non-interlaced

Let's change the extension to a more suited one, and export the file to our browser to visualize it :

    mv decoded decoded.png
    sz decoded.png / rz

# File inspection

We now have a AI generated image of a hackerman, and it seems someone added a string of characters on it. It's a mix of numbers and letters from A to F. Looks like Hexadecimal !

Now, with our trusty fingers or an OCR, we extract the string and paste it to a hex decoder. We now have our flag !!

And voilà ! Thanks for reading ! 🤓