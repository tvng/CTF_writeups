# Riddle Registry
* Difficulty : easy
* Tag : Forensics
* Author : Prince Niyonshuti N.

This challenge is about finding a flag within a PDF named `confidential.pdf`. Being marked as "easy", the flag shouldn't be hidden too deep. Let's start with some fundamental checks !

For this challenge I used PicoCTF's webshell.

## First inspection

I started with the Linux command `file` as a basic inspection to verify the actual type of the file.


    file confidential.pdf

Which gives us

> confidential.pdf: PDF document, version 1.7, 1 pages

The PDF is truly a PDF. So the clue is not here. I also tried to open the PDF and copy/paste the censored text but no flag here.

Now to our next inspection tool : `exiftool` !

    exiftool confidential.pdf

We get 

    ExifTool Version Number         : 12.40
    File Name                       : confidential.pdf
    Directory                       : .
    File Size                       : 178 KiB
    File Modification Date/Time     : 2025:09:29 21:29:21+00:00
    File Access Date/Time           : 2025:11:07 09:26:58+00:00
    File Inode Change Date/Time     : 2025:11:07 09:25:38+00:00
    File Permissions                : -rw-rw-r--
    File Type                       : PDF
    File Type Extension             : pdf
    MIME Type                       : application/pdf
    PDF Version                     : 1.7
    Linearized                      : No
    Page Count                      : 1
    Producer                        : PyPDF2
    Author                          : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jYTc2YmJiMn0=

Doesn't the 'Author' field seem off ? Why would an author be named with so many random letters ?

I can smell Base64 ! 

## Base64

Base64 use the character `=` as a padding when encoding a sequence, so we may find 0, 1 or 2 '=' signs at the end of Base64 strings. To decode, I used the command :

    base64 -d <<< cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jYTc2YmJiMn0=

Which gives us the flag (that I won't disclose here).

And voilà ! Thank you for reading ! 🤓
