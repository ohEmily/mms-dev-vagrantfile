## setup
1) install Vagrant from [here](https://www.vagrantup.com/downloads.html).
install VirtualBox from [here](https://www.virtualbox.org/wiki/Downloads).
2) run the following:
```
git clone https://github.com/ohEmily/mms-dev-vagrantfile ~/mms-vagrant
cd ~/mms-vagrant
vagrant plugin install vagrant-disksize
vagrant up
```
3) to access the guest OS command line, run `vagrant ssh`.


## faq
_note: make sure to read the Vagrantfile and check the Vagrant docs.
They will likely be able to help!_

#### I have a port collision! why?
this box will need port 27017 and the range of ports defined in the Vagrantfile
networking section to be free. If there is a process on your host machine that
is using any of those ports, `vagrant up` will fail.

If you can use `netstat`/`lsof`/`ps` output to find what process is using the port,
kill it. If you can't, chances are there is a process that is using that port
ocasionally. To remedy this, there are a couple options you can do:
1) change the port range by editing the Vagrantfile or using Virtualbox
2) restart your machine (this might reduce the amount of stuff running on your
system, so you might have fewer processes trying to take ports)

#### I want to start over, deleting all the data on my VM and all snapshots.
`vagrant destroy`

#### I want to shut down this VM but keep my data in case I want it later.
`vagrant halt`
