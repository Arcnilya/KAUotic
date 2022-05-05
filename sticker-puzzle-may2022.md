# The KAUotic Sticker Puzzle of May 2022

## Step 1: The Hidden Zip
The puzzle starts with a gif hosted on my website: <https://www.jonathanmagnusson.com/do_you_liek_mudkipz.gif>.
The reason why the gif was not simply posted in the Discord chat is because Discord had a tendency to remove any concatenated files...
The file was created by simply concatenating the original mudkips.gif with secret.zip using the cat command:

``$ cat mudkips.gif secret.zip > do_you_liek_mudkipz.gif``

There are multiple ways to find/extract any potential hidden zip files:
- ``$ binwalk -e do_you_liek_mudkipz.gif``
- ``$ unzip do_you_liek_mudkipz.gif``
- Open with winrar


## Step 2: Stegonography
Extracting the contents of secret.zip gives you a file called **longcat.jpg**.
This file is also containing some secret information that is not visible on the surface,
and there is also a hint slightly less hidden in the file.
- ``$ file longcat.jpg``
- ``$ strings longcat.jpg``
- ``$ exiftool longcat.jpg``
- ``$ vim longcat.jpg``
- or any other text editor

Using any of the above commands will get you the following interesting string: **steghide pass:kentuckyfriedchicken**.

A quick search on the interwebs says the following:
"Steghide is steganography program which hides bits of a data file in some of the least significant bits of another file in such a way that the existence of the data file is not visible and cannot be proven."
After installing steghide, the hidden bits can be extracted with the following command:
- ``$ steghide extract -sf longcat.jpg -p kentuckyfriedchicken``


## Step 3: The Obfuscated URL
The extracted information from **longcat.jpg** using steghide seem to be a file named **owo.txt** which contains the following:

**lmth.rekcits/ftc/moc.nossungamnahtanoj.www//:sptth**

Upon further inspection, this seem to be a reversed URL, lets pipe the output of the file to the rev command to reverse the string:
- ``$ cat owo.txt | rev``
- <https://www.flipyourtext.com/>


## Step 4: The HTML comment
Visiting <https://www.jonathanmagnusson.com/ctf/sticker.html> greets you with a suspiciously dismissive message... By either pressing F12 or adding "view-source:" before the URL we can find an HTML comment containing some weird letters and numbers.


## Step 5: The Encoding
If a string seem completely random there are some nice resources for identifying the algorithms used to encode it:
- <https://www.boxentriq.com/code-breaking/cipher-identifier>
- [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/#recipe=Magic(3,true,false,''))

Some of you might even see that it is base64 encoded by just looking at it!
To decode the string you can use:
- ``$ echo c3ludHtjeW1fdHZvX3huaGJndnBfZmd2cHhyZX0K | base64 -d``
- <https://www.boxentriq.com/code-breaking/base64-decoder>
- [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true))
- <https://www.base64decode.org/>


## Step 6: Caesar Cipher
The base64 encoded string seem to have been: **synt{cym_tvo_xnhbgvp_fgvpxre}**
This is looking a lot like the structure of other flags (<https://www.jonathanmagnusson.com/ctf/hidden.html>).
Four letters ("flag"?) followed by a hard-to-guess string enclosed by curly-brackets...
Lets assume that the first four letters "synt" are "flag".
F is the 6th letter in the alphabet, and LAG would be 12,1,7.
S is the 19th letter, and YNT would be 25,14,20.
They are consistently 13 steps away from each other!
Lets try a *caesar cipher* with a shift/rotation of 13, also known as rot13:
- <https://rot13.com/>
- <https://www.dcode.fr/caesar-cipher>
- [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13))
- ``$ echo "synt{cym_tvo_xnhbgvp_fgvpxre}" | tr "a-z" "n-za-m"``

