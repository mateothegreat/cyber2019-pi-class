# Setup the user account

First we need to create the minecraft user account.
This is done for security reasons.. i.e.: if our minecraft server
gets compromised we don't want our entire system to fall victim.

```
$ sudo useradd -m minecraft
```
Now we can start a shell account as the new minecraft user to do the fun stuff:

```
$ sudo -u minecraft /bin/bash
```

# Minecraft Server

Setting up our minecraft server takes a couple of steps...

## Download Spigot Build Tools

Before we can build our minecraft server let's download the latest spigot build tools which does this for us auto-magically:

```
$ wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar
```

## Build the Minecraft Server

```
$ java -jar BuildTools.jar
```

## Accept the EULA
Open the file `eula.txt` in a text editor so we can change "False" to "True":

```
$ nano eula.txt
```

## Start the Minecraft Server

```
java -Xms512M -Xmx1008M -jar spigot-*.jar nogui
```
Our server should now be running and listening on port 25565.

# Connecting to our Minecraft Server
Now that we've setup minecraft and got it up and running we're ready to connect to the server from our windows machine(s).

First we need to get the network address (ip) by running:

```
ip a
```

The ip address will be after the "inet" portion of the "eth0" interface such as:
```
inet 172.17.0.2/16
```

We'll use the ip address 172.17.0.2 and port 25565 in our minecraft client.