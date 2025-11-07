# Riddle Registry
* Difficulty : easy
* Tag : General Skills
* Author : Yahaya Meddy

This challenge is about finding scattered flag in a log file, `server.log`. 
For this challenge I used PicoCTF's webshell.

## First inspection

To do some basic inspections, I started with the Linux commands `file` and `exiftool` to check the content of the file without opening it.

    file confidential.pdf
    exiftool confidential.pdf

They don't give us anythining noteworthy except that the file is ASCII text.

Next I looked at the content of the log file with `less`:

    less server.log

We can see a very long log file that starts with the line :

    [1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_

## Filtering the logs

So the flag seems scattered all over the log file, and each log line containing a flag piece contains `FLAGPART`. We can of course do a manual reconstruction but that wouldn't be fun, right ? It's time to use `grep` to do some text searching on `FLAGPART`!

    grep FLAGPART server.log 

We get 

    [1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 10:02:55] INFO FLAGPART: y0urlinux_
    [1990-08-09 10:05:54] INFO FLAGPART: sk1lls_
    [1990-08-09 10:05:55] INFO FLAGPART: sk1lls_
    [1990-08-09 10:10:54] INFO FLAGPART: cedfa5fb}
    [1990-08-09 10:10:58] INFO FLAGPART: cedfa5fb}
    [1990-08-09 10:11:06] INFO FLAGPART: cedfa5fb}
    [1990-08-09 11:04:27] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 11:04:29] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 11:04:37] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 11:09:16] INFO FLAGPART: y0urlinux_
    [1990-08-09 11:09:19] INFO FLAGPART: y0urlinux_
    [1990-08-09 11:12:40] INFO FLAGPART: sk1lls_
    [1990-08-09 11:12:45] INFO FLAGPART: sk1lls_
    [1990-08-09 11:16:58] INFO FLAGPART: cedfa5fb}
    [1990-08-09 11:16:59] INFO FLAGPART: cedfa5fb}
    [1990-08-09 11:17:00] INFO FLAGPART: cedfa5fb}
    [1990-08-09 12:19:23] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 12:19:29] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 12:19:32] INFO FLAGPART: picoCTF{us3_
    [1990-08-09 12:23:43] INFO FLAGPART: y0urlinux_
    [1990-08-09 12:23:45] INFO FLAGPART: y0urlinux_
    [1990-08-09 12:23:53] INFO FLAGPART: y0urlinux_
    [1990-08-09 12:25:32] INFO FLAGPART: sk1lls_
    [1990-08-09 12:28:45] INFO FLAGPART: cedfa5fb}
    [1990-08-09 12:28:49] INFO FLAGPART: cedfa5fb}
    [1990-08-09 12:28:52] INFO FLAGPART: cedfa5fb}

## Filtering a bit further

It seems we have some duplicate lines... We could stop here, but I want to clean this output a little bit with `sed` and remove duplicates with `uniq`:

    grep FLAGPART server.log | sed 's/^.*FLAGPART: // | uniq -u'

I used a find and replace pattern that could translate as : for each line, take the text from the beginning of the line up to "FLAGPART:" and delete it (replace it with nothing). Which gives us 

    picoCTF{us3_
    y0urlinux_
    sk1lls_

Somehow, the `cedfa5fb}`line is omitted by `uniq -u`, which I suspect may be related to the brackets ? Please tell me if you have an actual explanation !! So I tried `awk` instead

 
    grep FLAGPART server.log | sed 's/^.*FLAGPART: //' | awk '!a[$0]++ {print}' ORS=''
    
With `!a[$0]++` being the command to delete duplicates, and the rest to concatenate all the lines. We get a nice flag in one single piece.


And voilà ! Thank you for reading ! 🤓