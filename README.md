# Scripts for Ubuntu on HyperV on Windows
Because the quick create is broken AF right now

Let me explain: when you create a VM using the QuickCreate feature with HyperV, and in particular, if you're creating a 22.04 (this is probably true for any version of Ubuntu), a bunch of stuff won't be correct.

First of all, the network settings will be all messed up, so I provide below a gist that you can use so that your network will just work

Second, the hard drive, by default the size is like around 15GB or something. And, no, changing the size of the harddrive in the settings of your VM is not enough. And, no, EVEN IF you chose a size bigger than 15GB during the setup of the VM -> you'll still need to extend this harddrive and this is a little bit messy to do -> thus I provide a gist for that too below

# Setting up netork with Netplan, look right there
https://gist.github.com/lagraviereScience/310bd5759ab20a52c6cccffd89c4550c
