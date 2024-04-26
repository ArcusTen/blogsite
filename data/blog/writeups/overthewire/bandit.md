---
title: OverTheWire - Bandit All levels (Solved)
date: '2024-04-26'
tags: ['linux', 'overthewire', 'bandit']
draft: false
summary: ...

---

## Table of Contents

0. **[Level 0](#level-0)**
1. **[Level 1](#level-01)**
2. **[Level 2](#level-02)**
3. **[Level 3](#level-03)**
4. **[Level 4](#level-04)**
5. **[Level 5](#level-05)**
6. **[Level 6](#level-06)**
7. **[Level 7](#level-07)**
8. **[Level 8](#level-08)**
9. **[Level 9](#level-09)**
10. **[Level 10](#level-10)**
11. **[Level 11](#level-11)**
12. **[Level 12](#level-12)**
13. **[Level 13](#level-13)**
14. **[Level 14](#level-14)**
15. **[Level 15](#level-15)**
16. **[Level 16](#level-16)**
17. **[Level 17](#level-17)**
18. **[Level 18](#level-18)**
19. **[Level 19](#level-19)**
20. **[Level 20](#level-20)**
21. **[Level 21](#level-21)**
22. **[Level 22](#level-22)**
23. **[Level 23](#level-23)**
24. **[Level 24](#level-24)**
25. **[Level 25](#level-25)**
26. **[Level 26](#level-26)**
27. **[Level 27](#level-27)**
28. **[Level 28](#level-28)**
29. **[Level 29](#level-29)**
30. **[Level 30](#level-30)**
31. **[Level 31](#level-31)**
32. **[Level 32](#level-32)**
33. **[Level 33](#level-33)**

---

## Level 0:

**The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.**

```:Password
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```

#### Explanation
    
Use this command to login to the server:
```bash:ssh-command
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

```:ssh-password
bandit0
```
![Login](/static/writeups/overthewire/bandit/login.png)


Use `ls` to see a list of all files and directories. You will see the **readme** file there. Use `cat` to display its contents.

![Level-0](/static/writeups/overthewire/bandit/bandit-level0-1.png)
    
---

## Level 01:

**The password for the next level is stored in a file called `-` located in the home directory**

```:Password
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```

#### Explanation:
    
Use any of these commands:

```bash
cat ←
```
OR
```bash
cat ./-
```  

![Level-1](/static/writeups/overthewire/bandit/bandit-level1-1.png)

---

## Level 02:

**The password for the next level is stored in a file called spaces in this filename located in the home directory.**

```:Password
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```

#### Explanation:
    
Type `cat spaces` then press tab button to get actual name: `spaces\ in\ this\ filename.`

```bash
cat spaces\ in\ this\ filename
```

The backslashes (`\`) in this command are used as escape operators to escape spaces and instruct `cat` to treat them as part of the filename rather than as terminators.

![Level-2](/static/writeups/overthewire/bandit/bandit-level2-1.png)

---

## Level 03:

**The password for the next level is stored in a hidden file in the inhere directory.**

```:Password
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```

#### Explanation:
    
```bash
cd inhere
```

We use `ls` with `-a` flag to also list hidden files and directories:

```bash
ls -a
```

Here, is the hidden file `.hidden`. Use `cat` to print its content on the terminal:

```bash
cat .hidden
```
![Level-3](/static/writeups/overthewire/bandit/bandit-level3-1.png)

---

## Level 04:

**The password for the next level is stored in the only human-readable file in the inhere directory.** 

**(Tip: If your terminal is messed up, try the “reset” command.)**

```bash:Password
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```

#### Explanation:
    
To access the content of all 10 files in this directory, simply use `cat` command along with the wildcard `*`. The wildcard instructs the system to include all files within the specified directory:

```bash
cat ./-file0*
```

`./`: This represents the current directory.

`-file0*`: This part refers to a file or files whose names start with **-file0** in the current directory, with the `*` acting as a wildcard character, matching any characters that follow "file0".

So, this command will display the contents of all files in the current directory whose names begin with **-file0**.

![Level-4](/static/writeups/overthewire/bandit/bandit-level4-1.png)

---

## Level 05:

**The password for the next level is stored in a file somewhere under cd inthe inhere directory and has all of the following properties:**

- **human-readable**

- **1033 bytes in size**

- **not executable**

```:Password
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```

#### Explanation:
    
Command that I used:

```bash
find ~/inhere -type f -readable -size 1033c ! -executable
```

`~/inhere`: This specifies the directory where the search will take place.

`-type f`: This option instructs find to only consider files (not directories).

`-readable`: This flag ensures that only files that are readable by the user executing the command are considered.

`-size 1033c`: This specifies the size of the files to search for. Here, it looks for files with a size of exactly 1033 bytes (`c`).

`! -executable`: This part of the command uses the `!` operator to negate the -executable condition. So, it excludes files that are not executable.

![Level-5](/static/writeups/overthewire/bandit/bandit-level5-1.png)

And now cat that file out:

![Level-5](/static/writeups/overthewire/bandit/bandit-level5-2.png)
    
---

## Level 06:

**The password for the next level is stored somewhere on the server and has all of the following properties:**

- **owned by user bandit7**

- **owned by group bandit6**

- **33 bytes in size**

```:Password
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

#### Explanation:

Command that I used:

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

`/`: This specifies the starting directory for the search. In this case, it starts from the root directory, searching the entire filesystem.

`-type f`: This option specifies that only files should be considered (not directories).

`-user bandit7`: This option specifies that the files must be owned by the user **bandit7**.

`-group bandit6`: This option specifies that the files must belong to the group **bandit6**.

`-size 33c`: This option specifies that the files must have a size of 33 bytes.

`2>/dev/null`: This redirects any error messages generated during the search to `/dev/null`, effectively suppressing them.

So, altogether the command is searching the entire filesystem for regular files owned by user **bandit7**, belonging to group **bandit6** and having a size of 33 bytes. Any errors encountered during the search are discarded.

![Level-6](/static/writeups/overthewire/bandit/bandit-level6-1.png)

And now cat that file out:

![Level-6](/static/writeups/overthewire/bandit/bandit-level6-2.png)
    
---

### ***Level 07:***

***The password for the next level is stored in the file data.txt next to the word millionth.***

```bash
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```

- ***Explanation:***
    
    ```bash
    cat data.txt | grep "millionth"
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/af4bf21b-2eca-414f-b374-49ce0586933c/Untitled.png)
    

---

### ***Level 08:***

***The password for the next level is stored in the file data.txt and is the only line of text that occurs only once.***

```bash
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```

- ***Explanation:***
    
    *I read* **`uniq`** *command and saw this:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/6e29bcb4-b232-4a97-98d3-b6d298862a06/Untitled.png)
    
    ```bash
    sort data.txt | uniq -u
    ```
    

---

### ***Level 09:***

***The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.***

```
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

- ***Explanation:***
    
    ```bash
    strings data.txt | grep "=="
    ```
    
    *(we are using strings command to find for strings (human-readable) in data.txt)*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/c09f9db3-047d-4bea-b405-7abfef3e5d88/Untitled.png)
    

---

### ***Level 10:***

***The password for the next level is stored in the file data.txt, which contains base64 encoded data.***

```
6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```

- ***Explanation:***
    
    ```bash
    base64 -d data.txt
    ```
    

---

### ***Level 11:***

***The password for the next level is stored in the file data.txt,
where all lowercase (a-z) and uppercase (A-Z) letters have been
rotated by 13 positions.***

```bash
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```

- ***Explanation:***
    
    ***Method 1:***
    
    *Copy the contents of data.txt and decode it in* **`dcode.fr/rot-13-cipher`**
    
    ***Method 2:***
    
    *To decode a rot13 encrypted text (within terminal):*
    
    ```bash
    cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    ```
    

---

### ***Level 12:***

***The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the man pages!)***

```bash
wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```

- ***Explanation:***
    
    *Creating a directory under tmp and copying data.txt file into it because we don’t have permission*
    
    ```bash
    xxd -r data.txt > data
    ```
    
    `**xxd**` *is used to make a hexadump or do the reverse* **`-r`** *means ‘to reverse’*
    
    *Now file the data, change its extension to its actual file type, decompress it. OVER AND OVER AGAIN*
    
    ```bash
    mv data data.gz
    ```
    
    ```bash
    gzip -d data.gz
    ```
    
    ```bash
    mv data data.bz2
    ```
    
    ```bash
    bzip2 -d data.bz2
    ```
    
    ```bash
    mv data data.gz
    ```
    
    ```bash
    gzip -d data.gz
    ```
    
    ```bash
    mv data data.tar
    ```
    
    ```bash
    tar -xf data.tar
    ```
    
    ```bash
    mv data5.bin data5.tar
    ```
    
    ```bash
    tar -xf data5.tar
    ```
    
    ```bash
    mv data6.bin data6.bz2
    ```
    
    ```bash
    bzip2 -d data6.bz2
    ```
    
    ```bash
    mv data6 data6.tar
    ```
    
    ```bash
    tar -xf data6.tar
    ```
    
    ```bash
    mv data8.bin data8.gz
    ```
    
    ```bash
    gzip -d data8.gz
    ```
    
    *Finally, you will get an ASCII text, cat that out.*
    
    ```bash
    **cat data8**
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b818cd62-3a1e-4339-be09-da29e2559763/Untitled.png)
    

---

### ***Level 13:***

***The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.***

```bash
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```

***Note:** This is actual password for bandit level 14*

- ***Explanation:***
    
    *To use the private key for* **`SSH`** *authentication, simply use the ‘***`ssh`***’ command with the* **`-i`** *flag
    to specify the path to your private key. i.e.,* **`ssh -i ~/.ssh/id_rsa username@remote_server`**
    
    ```bash
    ssh -i sshkey.private bandit14@localhost -p 2220
    ```
    
    *Now after logging in:*
    
    ```bash
    cat /etc/bandit_pass/bandit14
    ```
    

---

### ***Level 14:***

***The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.***

```bash
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```

- ***Explanation:***
    
    ```bash
    nc [localhost](http://localhost) 30000
    ```
    
    *Submitting password “fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq”
    Remember to stay logged in bandit’s servernc localhost 30000*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/508dc003-c475-4be1-95cd-8cc4b1681ceb/Untitled.png)
    

---

### ***Level 15:***

***The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.***

```bash
JQttfApK4SeyHwDlI9SXGR50qclOAil1
```

- ***Explanation:***
    
    ```bash
    ncat --ssl localhost 30001 
    ```
    
    *Submit password of current level*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/303237eb-546a-4bba-ab64-a3cca79aed2a/Untitled.png)
    

---

### ***Level 16:***

***The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.***

```bash
ssh -i key bandit17@bandit.labs.overthewire.org -p 2220
```

- ***Explanation:***
    
    *Using this command to scan for open ports:*
    
    ```bash
    nmap [localhost](http://localhost) -p 31000-32000
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/f752d94f-addf-4792-abc8-209df724b93a/Untitled.png)
    
    *It will show a list of services. Submit the password of this level to all of them using:*
    
    ```bash
    ncat --ssl [localhost](http://localhost) <port-no>
    ```
    
    *One of them (port:* **`31790`***)will give aN RSA key.* 
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/38231b02-9ecd-4bb1-a53d-f0f2f1f11505/Untitled.png)
    
    *Copy it to your original hostdevice*
    
    *(remember to set permission to 400) and now connect to level 17 using this RSA key:*
    
    ```bash
    ssh -i key bandit17@bandit.labs.overthewire.org -p 2220
    ```
    

---

### ***Level 17:***

***There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new***

***NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19***

```
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```

- ***Explanation:***
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b7b625d8-4b69-4b5f-a14d-fae7f4bb475c/Untitled.png)
    
    *Use this command:*
    
    ```bash
    grep -f passwords.old --invert-match passwords.new
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/4921dd51-42f9-4974-87fd-db776a08c1cc/Untitled.png)
    

---

### ***Level 18:***

***The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.***

```bash
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```

- ***Explanation:***
    
    *No problem, if bashrc is corrupted then we can call simple sh (pseudo-shell)*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/18689b12-72c5-4918-a243-8c0b43099942/Untitled.png)
    
    *Follow these commands to get a shell:*
    
    ```bash
    ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/406cd454-0f7c-474a-8e78-e582edf3fb77/Untitled.png)
    
    *Rest of challenge is simple.*
    

---

### ***Level 19:***

***To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.***

```bash
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```

- ***Explanation:***
    
    *Here, you can see am* **`ELF lsb-file bandit20-do`**  *with* **`bandit20`***’s user SUID set on it:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/859a9504-dd5e-4e8c-9a2b-d071d6e1751e/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b2f5d581-fe6e-425f-aa82-475ceafa1639/Untitled.png)
    
    *And upon running it it tells us how to use it:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/0b46a450-5158-4ae3-a9ac-f32179b210e2/Untitled.png)
    
    *So let’s use use it to get* **`bandit20`***’s pasword:*
    
    ```bash
    **./bandit20-do cat /etc/bandit_pass/bandit20**
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/33394331-140a-4128-a8e8-933aec738680/Untitled.png)
    

---

### ***Level 20:***

***There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).***

***NOTE: Try connecting to your own network daemon to see if it works as you think***

```bash
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
```

- ***Explanation:***
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/cee8d8ae-119d-4589-97c3-f07460de3cd8/Untitled.png)
    

---

### ***Level 21:***

***A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.***

```bash
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
```

- ***Explanation:***
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/0164544d-3aaf-456d-b401-ed5db9f26d0c/Untitled.png)
    

---

### ***Level 22:***

***A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.***

***NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.***

```bash
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
```

- ***Explanation:***
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b68bcea5-6fee-4601-9941-24bd0c4322f7/Untitled.png)
    

---

### ***Level 23:***

***A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.***

***NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!***

***NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…***

```bash
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
```

- ***Explanation:***
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/f28860e0-94ea-42b7-b9e7-5d8d0d2c9f4f/Untitled.png)
    
    *Contents of* **`/tmp/itsctrl/main.sh`***:*
    
    ```bash
    #!/bin/bash
    
    cat /etc/bandit_pass/bandit24 > /tmp/itsctrl/pass.txt
    ```
    

---

### ***Level 24:***

***A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.*** 

***You do not need to create new connections each time***

```bash
p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d
```

- ***Explanation:***
    
    *Let me check if we have hydra:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/ae7ce690-2cab-42b9-b1cb-27819f589dbf/Untitled.png)
    
    *No. We don’t have it. But wait, We can write a bash script to do that:*
    
    ```bash
    #!/bin/bash
    
    for i in {0000..9999}
    do      
    	#echo "$i"
    	echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"
    done | nc localhost 30002
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/fda60151-5505-45fd-bde8-9b629816e1a7/Untitled.png)
    
    *Remember to do this in a writeable directory, you can create one in* **`/tmp`**
    

---

### ***Level 25:***

***Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.***

```bash
c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1
```

- ***Explanation:***
    
    *It automatically logs us out when try to connect:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/7633c31f-e741-4790-8f17-7642ed1dc2cf/Untitled.png)
    
    *They said the shell of bandit26 is different so let’s have a look at it first:*
    
    ```bash
    cat /etc/passwd | grep "bandit26"
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/7c13ee5e-ab82-4937-a3e5-648767ecfcff/Untitled.png)
    
    **`showtext`***??* *Never heard of that type of shell.* 
    
    *So, I searched a bit and yes there is no such such type of shell. Let’s have a look at its executable:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/e98e9cd0-53dd-4db2-9e84-970924f6e617/Untitled.png)
    
    *So, its basically just using more command to read a file and then exiting with exit code of 0. So that’s why it automatically disconnects when tried to run.*
    
    *What we need to do is to trigger more command to go into its command view. So, the ssh request doesn’t just exit.* 
    
    *Let’s make our terminal as small as possible then ssh in:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/60298fd2-f099-409a-9687-fc791ace02e5/Untitled.png)
    
    *So yeah we have successfully launched more command in its command mode.*
    
    ***that’s the way to break out. Very out of the box to get out of the box.***
    
    *Now, If we look at the man-page of more we can see that by pressing the “v” key (while in interactive mode) it will open the current line in an editor that is defined by the VISUAL and EDITOR environment variables. If both the variables are not set then Vim will be used. Lets see on pressing “v” which editor we get.*
    
    *And we got vim:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/577630eb-c8e9-4377-a27b-0834eb71dc18/Untitled.png)
    
    *Now, we can open another file using* **`:e`** *command.* 
    
    *We will want the bandit26 password so the command will be:*
    
    ```bash
    :e /etc/bandit_pass/bandit26
    ```
    
    *And we got the pass:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/d0682ae4-6f41-4e35-b489-0b4a5c59bfe3/Untitled.png)
    
    *but we haven’t got a shell still and when we will ssh into this level it will still kick us out.*
    
    *Thank God we can call a shell from Vim; otherwise, it would be a hell of a difficulty level.*
    
    *(I know how to spawn a shell while staying in vim. I learned it in a tryhackme room if you are wondering 😉)*
    
    *Type the following commands:*
    
    ```bash
    set shell=/bin/bash
    ```
    
    ```bash
    :shell
    ```
    
    *And we are in:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/ab76de12-ca0d-462c-bd07-3b1f1396b80b/Untitled.png)
    
    *Tricky challenge IK.*
    

---

### ***Level 26:***

***Good job getting a shell! Now hurry and grab the password for bandit27!***

```bash
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
```

- ***Explanation:***
    
    *Easy*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/35649867-c941-4c05-a244-54540509138d/Untitled.png)
    

---

### ***Level 27:***

***There is a git repository at*** `ssh://bandit27-git@localhost/home/bandit27-git/repo` ***via the port* `2220`*. The password for the user* `bandit27-git` *is the same as for the user*** `bandit27`***.***

***Clone the repository and find the password for the next level.***

```bash
AVanL161y9rsbcJIsFHuw35rjaOM19nR
```

- ***Explanation:***
    
    *First move into a directory where we have write permissions:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/e631dcb6-38b5-4e79-973e-0928a74ddebc/Untitled.png)
    
    *Now clone the provided repository via port* **`2220`** *by running this command:*
    
    ```bash
    git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
    ```
    
    *It will ask you for the password. Type the password of curent level:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/6c29f959-9e61-457a-80a3-ee3c1b69f1f2/Untitled.png)
    
    *We got a repo having* **`README`** *file in it:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/19bc5d97-c328-41c2-ad45-ec7a3006a973/Untitled.png)
    
    *cat that out:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/da5204d6-f12d-4f98-8746-9b5b0e60dcd9/Untitled.png)
    
    *The password is:* **`AVanL161y9rsbcJIsFHuw35rjaOM19nR`**
    

---

### ***Level 28:***

***There is a git repository at* `ssh://bandit28-git@localhost/home/bandit28-git/repo` *via the port* `2220`*. The password for the user* `bandit28-git` *is the same as for the user* `bandit28`.**

*Clone the repository and find the password for the next level.*

```bash
tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S
```

- ***Explanation:***
    
    *First move into a directory where we have write permissions:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/cc2f7ac6-aa49-4d08-8e6e-506fe3df1da2/Untitled.png)
    
    *Now clone the provided repository via port* **`2220`** *by running this command:*
    
    ```bash
    git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
    ```
    
    *It will ask you for the password. Type the password of curent level:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/6c29f959-9e61-457a-80a3-ee3c1b69f1f2/Untitled.png)
    
    *We got a repo having* **`README.md`** *file in it:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/f6500305-ca0a-4372-87be-92adce0eff1b/Untitled.png)
    
    *cat that out:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b942d088-a838-4237-9bdb-7e9fa7a62e00/Untitled.png)
    
    *UMMM.*
    
    *Let’s have a look at logs:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/b988f0eb-a375-4b4f-86d8-5c9035cbb652/Untitled.png)
    
    *Let’s check the commit “add missing data”:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/2b133e84-7cce-4950-b8e6-fea4dd36db9e/Untitled.png)
    
    *Now, cat that again:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/eb2aa61b-5f58-4f5d-9992-37bfb372d2a9/Untitled.png)
    
    *The password is:* **`tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S`**
    

---

### ***Level 29:***

***There is a git repository at* `ssh://bandit29-git@localhost/home/bandit29-git/repo` *via the port* `2220`*. The password for the user* `bandit29-git` *is the same as for the user* `bandit29`*.***

***Clone the repository and find the password for the next level.***

```bash
xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS
```

- ***Explanation:***
    
    *First move into a directory where we have write permissions:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/5de405cc-cfc9-419f-bd44-702d342ad379/Untitled.png)
    
    *Now clone the provided repository via port* **`2220`** *by running this command:*
    
    ```bash
    git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
    ```
    
    *It will ask you for the password. Type the password of curent level:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/8496c25f-d55a-45ac-9e1b-05f7c4eda9eb/Untitled.png)
    
    *We got a repo having* **`README.md`** *file in it:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/f9a5b8b4-2c09-4f17-99a9-6840ae45e16a/Untitled.png)
    
    *cat that out:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/2b507708-00e6-4e15-a6d1-61aa1aed633a/Untitled.png)
    
    *Hmmmm.*
    
    *Let’s have a look at logs:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/42b3d815-3de6-41ea-b2d6-42d18b4d1ea3/Untitled.png)
    
    *Let’s check the “initial commit”:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/ca044433-6dcc-4e32-8ac0-471445202272/Untitled.png)
    
    *Now, cat that again. Nope same output:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/25ea49d6-8e33-4199-a0da-3b67543d5aae/Untitled.png)
    
    *Let’s see what other branches we have:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/26006be7-70dd-4648-b54d-d8fc6c471198/Untitled.png)
    
    *We are currently in* **`master`***:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/2059ccce-5cf6-42fa-9bc4-fb7655f9f247/Untitled.png)
    
    *Let’s switch to* **`dev`** *and see that we get:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/69d402f4-dff9-4dab-85a9-b6d09e208951/Untitled.png)
    
    *Let’s cat this* **`README.md`** *file:*
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/70b309c8-b3ac-401d-813a-0314217bed11/fa1fe9a3-eb80-495a-9d0c-93a5da6ec7d8/Untitled.png)
    
    *The password is:* **`xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS`**
    

---

### ***Level 30:***

***There is a git repository at* `ssh://bandit30-git@localhost/home/bandit30-git/repo` *via the port* `2220`*. The password for the user* `bandit30-git` *is the same as for the user* `bandit30`*.***

***Clone the repository and find the password for the next level.***

```bash

```

- ***Explanation:***

---

### ***Level 31:***

***There is a git repository at* `ssh://bandit31-git@localhost/home/bandit31-git/repo` *via the port* `2220`*. The password for the user* `bandit31-git` *is the same as for the user* `bandit31`*.***

***Clone the repository and find the password for the next level.***

```bash

```

- ***Explanation:***

---

### ***Level 32:***

***After all this* `git` *stuff its time for another escape. Good luck!***

```bash

```

- ***Explanation:***

---

### ***Level 33:***

***At this moment, level 34 does not exist yet.***

---

# *—————————THE END——————————*

---

---

[OverTheWire (Quest)](https://www.notion.so/OverTheWire-Quest-ba967e2b24f54b15a26ae32d41f65030?pvs=21)