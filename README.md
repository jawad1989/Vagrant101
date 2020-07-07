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

# 4. Create new vagrant file
Open Visual Code
1. mkdir vagrant-code
2. cd in directory
3. core -r . // opens VS code in same directory
4. vagrant init //creates a new vagrant file
5. vagrant up //this will create the box
6. vagrant ssh
7. vagrant reload //restart VM

# 5. Shared folder
 In VagrantFile below code sets up shared folder/directory between host and VM /data in host /vagrant_data is VM
 ```
 config.vm.synced_folder "../data", "/vagrant_data"
 ```

# 6. Vagrant Networking
## 1. Port Forwarding
  #### Access NGINX from HOST
  in vagrantFile uncomment the line 31, you can change ports
  ```
  config.vm.network "forwarded_port", guest: 80, host: 8088, host_ip: "127.0.0.1"
  ```
## 2. Private Networks
By using this we can create a dhcp network and this will assig a non changing static IP to VM
*  VagrantFile Line 35 change 
```
config.vm.network "private_network", type: "dhcp"
```
* vagrant reload
* vagrant ssh
* ifconfig //get the ip and goto host browser and test the nginx now on port 80 http://172.28.128.3:80/
## 3. Public Networks

# 7. Vagrant Providers
1. Hyper-V
2. Virtual Box
3. Docker Containers

* Provider section is used to set provider-specific configurations

### Update RAM and CPU
in your VM run below commands, currently we have 1 GB ram and 1 CPU
* to get memory/RAM
```
vmstat -s
```
* to get cpu
```
lscpu
```
* we will change ram to 512 MB and cpu to 2, goto vagrantFile and between line 52-59 do the below changes and reload vagrant. You will see the changes.
```
config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "512"
     vb.cpus = 2
   end
```

### Create a GUI for VM

in your vagrant file do below changes:
```
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
     vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "512"
     vb.cpus = 2
     vb.customize ["modifyvm", :id, "--vram", "16"]
   end
```
* vagrant reload and vagrant ssh
* check VideoMemory
```
 lspci -v -s 00:02.0
```

# 7. Vagrant Provisioners
* scripts  in a vagrantfile or externaly in a separate file, 
* by default scripts run first time a box runs
* can install software, download app code and set configuration needed for dev environment
* by default provisioners run only first time, we can change options to configure to run on every boot

### create a provisioner - install nginx
* we create a new directory and name it `provisioners` and create a new file `nginx-install.sh`
* in vagrant file we add a new line to call this external file
```
# Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
   
   config.vm.provision "shell", path: "provisioners/nginx-install.sh"
  
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
```
* recreate the vagrant box
```
vagrant halt
vagrant destory
vagrant up
vagrant ssh
nginx -v
```
### install multiple provisoers
we can create multiple provisioners and in vagrantFile we ca call those in order to run
```
   config.vm.provision "shell", path: "provisioners/install-mongo.sh"
   config.vm.provision "shell", path: "provisioners/install-node.sh"
```
* the folder we vagrant init run from is by default a shared directory in vagrant VM



# 8. Running Multiple VMs
for creating multiple boxes we have to add conf in vagrant file, in example below we are creating two boxes one more mongo other for node
```
# Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
 
  config.vm.define "mongoDB" do |mongoDB| 
    mongoDB.vm.box = "bento/ubuntu-16.04" # base image
    mongoDB.vm.provider "virtualbox" do |vb| 
      vb.memory = "512"
    end
    mongoDB.vm.network "private_network", ip: "192.168.33.20" # create private network and set an IP
    mongoDB.vm.provision "file", source: "files/mongod.conf", destination: "~/mongod.conf"
    mongoDB.vm.provision "shell", path: "provisioners/install-mongo.sh"
  end

  config.vm.define "node" do |node|
    node.vm.box ="bento/ubuntu-16.04"
    node.vm.network "forwarded_port", guest: 3000, host:8080
    node.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
    node.vm.network "private_network", ip: "192.168.33.10"
    node.vm.provision "shell", path: "provisioners/install-node.sh"
  end
```
