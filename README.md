# Cyber 2019 Raspberry PI Class

## Pre-requisites

### Flashing the PI with Kali Linux

* Download the latest image at https://www.offensive-security.com/kali-linux-arm-images/. The current latest image as of this writing is https://images.offensive-security.com/arm-images/kali-linux-2019.2-rpi3-nexmon.img.xz.

* De-compress it with `tar xf kali-linux-2019.2-rpi3-nexmon.img.xz`

* Find the disk from the inserted card with `diskutil list`

* Unmount the disk with `diskutil unmount /dev/disk6`

* Write it to the sdcard with `dd if=kkali-linux-2019.2-rpi3-nexmon.img of=/dev/sdb bs=512k`

Once complete the output will look similar to:

```bash
2911+1 records in
2911+1 records out
3053371392 bytes transferred in 2151.132182 secs (1419425 bytes/sec)
```

## Exercises

* [Capture The Flag with Metasploit](metasploit-exercises)
* [Minecraft](minecraft-exercises/README.md)