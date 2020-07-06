# Vagrant101
as per wikipedia: Vagrant is an open-source software product for building and maintaining portable virtual software development environments;[5] e.g., for VirtualBox, KVM, Hyper-V, Docker containers, VMware, and AWS. It tries to simplify the software configuration management of virtualizations in order to increase development productivity. Vagrant is written in the Ruby language, but its ecosystem supports development in a few languages.
* Developers can create portable environments
* 
# Vagrant Components:
1. CLI : Start stop vagrant VMs 
2. Vagrant File: define Vagrant VMs, mostly statements not complex logic
3. VAgrant Cloud: Online MarketPlace
  example: ***ubuntu/trusty32*** First part of the name is organization and second part is distribuition

# install first box: ubuntu
1. Download Virtual Box
2. Install Vagrant
3. create directoy 
```
mkdir VagrantBox
```
4. Add ubuntu box
```
vagrant box add ubuntu/trusty32
```
5. Initialize vagrant box
```
vagrant init ubuntu/trusty32
```
6. Start box
```
vagrant up
```
7. connect/ssh to box
```
vagrant ssh
```
