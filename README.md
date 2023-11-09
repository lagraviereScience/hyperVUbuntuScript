# Scripts for Ubuntu on HyperV on Windows
Because the quick create is broken AF right now

Let me explain: when you create a VM using the QuickCreate feature with HyperV, and in particular, if you're creating a 22.04 (this is probably true for any version of Ubuntu), a bunch of stuff won't be correct.

First of all, the network settings will be all messed up, so I provide below a gist that you can use so that your network will just work

Second, the hard drive, by default the size is like around 15GB or something. And, no, changing the size of the harddrive in the settings of your VM is not enough. And, no, EVEN IF you chose a size bigger than 15GB during the setup of the VM -> you'll still need to extend this harddrive and this is a little bit messy to do -> thus I provide a gist for that too below

# Setting up network with Netplan
Edit this file
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
