# 1. Vagrant101
as per wikipedia: Vagrant is an open-source software product for building and maintaining portable virtual software development environments;[5] e.g., for VirtualBox, KVM, Hyper-V, Docker containers, VMware, and AWS. It tries to simplify the software configuration management of virtualizations in order to increase development productivity. Vagrant is written in the Ruby language, but its ecosystem supports development in a few languages.
* Developers can create portable environments
* vagrant save boxes in cache, if not removed it will pick from cache
* runs headless VM

# 2. Vagrant Components:
1. CLI : Start stop vagrant VMs 
2. Vagrant File: define Vagrant VMs, mostly statements not complex logic
3. Vagrant Cloud: Online MarketPlace
  example: ***ubuntu/trusty32*** First part of the name is organization and second part is distribuition
# files created

1. Local directory: `F:\vagrantBoxes\firstBox`
2. VMD FILE: `G:\Virtual Box Vms\firstBox_default_1594055243795_59703`
3. CACHE: `C:\Users\tech sign\.vagrant.d\boxes\bento-VAGRANTSLASH-ubuntu-18.04\202005.21.0\virtualbox` box has settings for memory/network etc

# 3. install first box: ubuntu
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
7. check status 
```
# single VM
vagrant status

# global status
vagrant global-status
```
7. connect/ssh to box
```
vagrant ssh
```
8. Halth a VM
```
vagrant halt <VM_ID>

```

# Create new vagrant file
Open Visual Code
1. mkdir vagrant-code
2. cd in directory
3. core -r . // opens VS code in same directory
4. vagrant init //creates a new vagrant file
5. vagrant up //this will create the box
6. vagrant ssh
7. vagrant reload //restart VM

# shared folder
in VagrantFile below code sets up shared folder/directory between host and VM /data in host /vagrant_data is VM
# config.vm.synced_folder "../data", "/vagrant_data"

# Vagrant Networking
Port Forwarding
  ## Access NGINX from HOST
   in vagrantFile uncomment the line 31, you can change ports
   `config.vm.network "forwarded_port", guest: 80, host: 8088, host_ip: "127.0.0.1"`
Private Networks
Public Networks
