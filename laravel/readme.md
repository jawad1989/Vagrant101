https://www.youtube.com/watch?v=n2RAArt1GRI

1. vagrant box add laravel/homestead
2. git clone https://github.com/laravel/homestead.git 
3. cd homestead
4. git checkout release
5. bash init.sh
6. The provider key in your Homestead.yaml file indicates which Vagrant provider should be used: virtualbox, vmware_fusion, vmware_workstation, parallels or hyperv. You may set this to the provider you prefer:
```
provider: virtualbox
```
7. create a ssh certificate
```
ssh-keygen -t rsa -C "jawadsalim12@gmail.com"
```
8. create a new folder and set it up in homestead.yaml
```
folders:
    - map: F:/Laravel_Projects
      to: /home/vagrant/Laravel_Projects

sites:
    - map: jawadxiv.test
      to: /home/vagrant/Laravel_Projects/Pixel_Invoice/public
```
9. update your etc hosts file
```
192.168.10.10 jawadxiv.test
```
10. change name of VM in homestead.yaml
```
ip: "192.168.10.10"
memory: 1028
cpus: 1
provider: virtualbox
name: laravel-projects
```
11.vagrant up
