# Scripts for Ubuntu on HyperV on Windows
Because the quick create is broken AF right now

Let me explain: when you create a VM using the QuickCreate feature with HyperV, and in particular, if you're creating a 22.04 (this is probably true for any version of Ubuntu), a bunch of stuff won't be correct.

First of all, the network settings will be all messed up, so I provide below a gist that you can use so that your network will just work

Second, the hard drive, by default the size is like around 15GB or something. And, no, changing the size of the harddrive in the settings of your VM is not enough. And, no, EVEN IF you chose a size bigger than 15GB during the setup of the VM -> you'll still need to extend this harddrive and this is a little bit messy to do -> thus I provide a gist for that too below

# Setting up network with Netplan
First, edit this file:
```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```

Replace whatever is in the file with the code below

*You can change to OpenDNS by using those IP addresses: 208.67.222.222, 208.67.220.220*
```yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eth0:
      nameservers:
        addresses:
        - 8.8.8.8
        - 8.8.4.4
      dhcp4: yes
```

Then you can apply this netplan
```bash
sudo netplan apply
```

Try to see if it works using:
```bash
sudo apt update
```
If you can update your apt local repo -> then network is working properly

You can also simply use:
```bash
ping google.com
```

For convenience, this is also available here: https://gist.github.com/lagraviereScience/310bd5759ab20a52c6cccffd89c4550c


# Resizing the disk 
*This tutorial assumes that you already have set a larger than 15GB disk size when creating the VM or using the expand feature in the settings of HyperV*

Based on this answer on superuser.com: https://superuser.com/a/1732294

Before you try to do this, I strongly advise you to setup your network correctly in the Ubuntu VM using this:
* https://gist.github.com/lagraviereScience/310bd5759ab20a52c6cccffd89c4550c

or this

* https://github.com/lagraviereScience/hyperVUbuntuScript/blob/main/README.md

So anyway, let's go, to resize your hard drive on HyperV virtual machine Ubuntu 22.04 start by doing this:
```bash
sudo apt install cloud-guest-utils
```

Use the "Disk" tools on Ubuntu or lsblk to figure out which parition you need to extend
```bash
$ lsblk
NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0     7:0    0     4K  1 loop /snap/bare/5
loop1     7:1    0  61,9M  1 loop /snap/core20/1434
loop2     7:2    0  63,5M  1 loop /snap/core20/2015
loop3     7:3    0  73,9M  1 loop /snap/core22/864
loop4     7:4    0 240,3M  1 loop /snap/firefox/3290
loop5     7:5    0 240,3M  1 loop /snap/firefox/3358
loop6     7:6    0 349,7M  1 loop /snap/gnome-3-38-2004/143
loop7     7:7    0 248,8M  1 loop /snap/gnome-3-38-2004/99
loop8     7:8    0   497M  1 loop /snap/gnome-42-2204/141
loop9     7:9    0  65,2M  1 loop /snap/gtk-common-themes/1519
loop10    7:10   0  91,7M  1 loop /snap/gtk-common-themes/1535
loop11    7:11   0  45,9M  1 loop /snap/snap-store/582
loop12    7:12   0  12,3M  1 loop /snap/snap-store/959
loop13    7:13   0  40,9M  1 loop /snap/snapd/20290
loop14    7:14   0   284K  1 loop /snap/snapd-desktop-integration/10
loop15    7:15   0   452K  1 loop /snap/snapd-desktop-integration/83
sda       8:0    0    90G  0 disk 
├─sda1    8:1    0  11,9G  0 part /var/snap/firefox/common/host-hunspell
│                                 /
├─sda14   8:14   0     4M  0 part 
└─sda15   8:15   0   106M  0 part /boot/efi
```
Here it's the partition sda1 or /dev/sda1 if you prefer that we want to extend.

How do I know that? Simply because of the size. sda1 has a size of 11,9GB so that's the data partition that we want to extend.

Also, you can notice that the disk IS of 90GB but you can also notice that NO partition is actually using the totality of space available.
I am gonna include a picture, that'll do better:
![Default Disk Partitioning on Ubuntu VM using Windows HyperV](https://github.com/lagraviereScience/hyperVUbuntuScript/blob/main/images/image.png?raw=true) 

Anyway, let's do the following:
```bash
sudo growpart /dev/sda 1
```
NB: there is a space between "/dev/sda" and "1". This is very important do not mess this up <3

Then you can do

```bash
sudo resize2fs /dev/sda1
```
This time there is no space!!!

And also -> you're done <3

Also, this mini-tutorial, is available here: https://gist.github.com/lagraviereScience/3c46819a668ac0b9aec819a3d3bfe761


# Keywords

This is just me putting a bunch of relevant keywords for this github gists to be indexed properly (hopefully)

* hyperV
* hyper-v
* hyper v
* Windows
* Windows 10
* VM
* Virtual machine
* Ubuntu
* Ubuntu 22.04
* network
* netplan
* hard drive
* resize hard drive
* resizefs
* extend hard drive
