---
title: TryHackMe - Medium CTF - Biohazard
date: '2024-04-23'
tags: ['ctf', 'tryhackme', 'writeup', 'steganography', 'crypto']
draft: false
summary: A CTF room based on the old-time survival horror game, Resident Evil. Can you survive until the end?

---

## Solution:

Starting of with an `nmap` scan:

```bash:nmap-command
nmap -sC -sV -oN nmap.log $IP
```

3 ports are open including `ftp`, `ssh` and `http`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled.png)

Lets have a look at the website (Running on port 80):

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled1.png)

Checking out its source code:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled2.png)

It is referring us to to webpage `/diningRoom`. Let’s check it out:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled3.png)

In its source code, I got this base64 encoded string:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled4.png)

```
SG93IGFib3V0IHRoZSAvdGVhUm9vbS8=
```

Decoding it and we have new directory to visit `/teaRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled5.png)

We will visit this page in a minute but first, let’s get the emblem:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled6.png)

Here it is:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled7.png)

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled8.png)

Now let’s visit `/teaRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled9.png)

Pick up the Lockpick:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled10.png)

Now let’s visit `/artRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled11.png)

After investigating the paper stick, I found the map here.

Great:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled12.png)

As we have visited `/diningRoom`, `/teaRoomand` `/artRoom`. Let’s visit `/barRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled13.png)

Enter the lockpick here and we are redirected to this page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled14.png)

He found a piano in bar room but I have no clue how to play it, lets read the note written as “moonlight somata”:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled15.png)

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled16.png)

It looks like a base32 encoded string, decode it and now we have music sheet to play the piano:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled17.png)

After playing this music sheet in the `/barRoom`, we are redirected to this page. Take the gold emblem from here:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled18.png)

Here it is:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled19.png)

It is saying to refresh the previous page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled20.png)

Remember the emblem that we got earlier:

```
emblem{fec832623ea498e20bf4fe1821d58727}
```

Enter it here. Now we are redirected to this page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled21.png)

It seems like a key or code word `rebecca`.

Remember in the `/diningRoom` there was a slot for an emblem? What if we place the gold emblem (that we recently acquired) here?

Let’s check:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled22.png)

And we got this cipher text:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled23.png)

To be honest, I had no clue what it is so I used a hint and It says Vigenère cipher:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled24.png)

Using Vigenère cipher with word rebecca as key and got this plain text:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled25.png)

```:plain-text
there is a shield key inside the dining room. The html page is called the_great_shield_key
```

Visiting the page `/diningRoom/the_great_shield_key.html`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled26.png)

Now lets move on to the room `/diningRoom2F`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled27.png)

In its source code i got this:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled28.png)

```
Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy
```

Encoded using `rot13`. Decode it and here is the plain text:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled29.png)

```:plain-text
You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first floor. Visit sapphire.html
```

Visiting the page `/diningRoom/sapphire.html`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled30.png)

Next is the `/tigerStatusRoom`.

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled31.png)

Here, we can place the flag from the previous room to get a piece of string that seems to be a piece of an encoded string, named as `crest 1`.

```
blue_jewel{e1d457e96cac640f863ec7bc475d48aa}
```

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled32.png)

From the notes, it seems like some decoding must be done before putting all the crests together (and there are a total of 4 crests). 

*OOKKK I GUESS…..*

Moving on to the next room `/galleryRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled33.png)

**2nd crest** is in the notes.txt in `/galleryRoom`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled34.png)

Moving on to the next room `/armorRoom`. Enter the shield symbol that we got earlier:

```
shield_key{48a7a9227cd7eb89f0a062590798cbac}
```

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled35.png)

We are redirected to this page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled36.png)

Examine the note and here is the **3rd crest**:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled37.png)

Moving on to the next room `/attic`. Enter the shield symbol that we got earlier:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled38.png)

We are redirected to this page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled39.png)

Examine the note and here is the **4rd crest**:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled40.png)

### Solving all the crests:

Now we will decode all the crest pieces by the number of times mentioned in the notes. For example crest 3 has been encoded three times and rest of pieces are encoded 2 times.

> **Quick Tip:** Use [CybeChef](https://gchq.github.io/CyberChef/) magic operator to know what encoding is being used.

Here is an overview how all the crests will be decoded:

#### Crest 1:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled41.png)

#### Crest 2:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled42.png)

#### Crest 3:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled43.png)

#### Crest 4:
![Screenshot](/static/writeups/tryhackme/biohazard/Untitled44.png)

Concatenating all string:

```
Decoded crest 1: RlRQIHVzZXI6IG
Decoded crest 2: h1bnRlciwgRlRQIHBh
Decoded crest 3: c3M6IHlvdV9jYW50X2h
Decoded crest 4: pZGVfZm9yZXZlcg==

Final string:
RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==
```

It looks like base64 encoded string. Decode it and here is the plain text:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled45.png)

So we got these credentials for `ftp`:

```:ftp-credentials
username: hunter
password: you_cant_hide_forever
```

Login to see what files are available:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled46.png)

These are the files available to us (3 jpeg file, 1 gpg and 1 text file):

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled47.png)

Download them to local machine using `mget`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled48.png)

first let’s have a look at `important.txt`: 

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled49.png)

So we have to first find helmet key. As we are given jpg files, let’s use steganography tools.

A `key-001.txt` is embedded in `001-key.jpg` file using `steghide`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled50.png)

Extract it using this command:

```bash
steghide --extract -sf 001-key.jpg
```

Here is the content of `001-key.txt`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled51.png)


Again seems like parts of an encoded string…

Anyways moving onto the next picture. Using `exiftool`, we got the next part of the string in comment section:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled52.png)

For the 3rd picture, I used `binwalk`:

```bash
binwalk -e 003-key.jpg
```

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled53.png)

Concatenate all of the string’s segments together:

```
cGxhbnQ0Ml9jYW5fYmVfZGVzdHJveV93aXRoX3Zqb2x0
```

It is a base64 encoded string, decode it and here is the plain text:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled54.png)

```
plant42_can_be_destroy_with_vjolt
```

OKK

It is the password for the **`helmet_key.txt.gpg`**. Use this command to unlock the file:

```bash
gpg -d helmet_key.txt.gpg
```

It will prompt you for the password; enter the one we found earlier.

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled55.png)

Here the helmet key:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled56.png)

Put it in the `/hidden_closet`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled57.png)

We are redirected to this page:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled58.png)

So `Enrico` is the leader of the STARS Bravo team. OK

MO Disk 1:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled59.png)

Wolf Medal:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled60.png)

Okay, now we have the ssh password, but where is the username? At this point, I was a bit clueless about what to do next, so I took a hint:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled61.png)

AHHH, of course `/studyRoom`. 

Don’t have a good relationship with studies even in the tryHackMe rooms 🙃

Enter the helmet symbol:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled62.png)

And we are redirected to this page. Examine the book and a new file named `doom.tar.gz` will be download:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled63.png)

Unzip it and here is the `ssh` username:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled64.png)

Now we have `ssh` credentials:

```:ssh-credentials
ssh username: umbrella_guest
ssh password: T_virus_rules
```

Let the fun part begin.

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled65.png)

First time that I noticed after logging in was `.jailcell` directory:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled66.png)

Here I found a note by `chris`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled67.png)

So `weasker` is the traitor.

I think `albert` is the key for MO disk 1:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled68.png)

We got credentials for `weasker`:

```bash
username: weasker
password: stars_members_are_my_guinea_pig
```

Switch to `weasker`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled69.png)

Checking `sudo` permissions for `weasker`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled70.png)

Run `sudo -s`:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled71.png)

We are root.

I got this note from weasker’s home directory:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled72.png)

Root’s flag:

![Screenshot](/static/writeups/tryhackme/biohazard/Untitled73.png)




