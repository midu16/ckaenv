# Certified Kubernetes Administrator Environment Lab
_Powered by Ansible and Vagrant_

## Installation options below:
## macOS
_Gatekeeper will block virtualbox from installing. All you have to do is go into Security & Privacy of System Preferences and click Allow under the General tab and rerun installation._
##### Install all at once with the command below:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" && xcode-select --install &&brew install ansible ; brew install python ; brew cask install vagrant ; brew cask install VirtualBox ; brew cask install virtualbox-extension-pack ; vagrant plugin install vagrant-guest_ansible
```

##### Alternatively, you can install everything individually below.
- [Install the Latest Version of Vagrant](https://www.vagrantup.com/downloads.html) - (`brew cask install vagrant`)
    - Vagrant Plugin - `vagrant plugin install vagrant-guest_ansible`
- [Install the Latest Version of Virtualbox](https://www.virtualbox.org/wiki/Downloads) (`brew cask install VirtualBox`)
    - Virtual Box Extension Pack (`brew cask install virtualbox-extension-pack`)

## CentOS/RHEL/Manjaro/Arch - Install all at once by Copy/Pasting the below command into your terminal as root.
_NOTE - If it's been awhile since you've run yum update, do that first. Reboot if the kernel was updated. There may be some dependencies errors but don't be alarmed as this won't stop the environment from working._

_NOTE2 - If you receive an error for an ansible guest vagrant plugin, DO NOT worry, as there are two different plugins related to Ansible and only one needs to be installed._
##### For CentoOS/RHEL7/Manjaro/Arch (Continue below for RHEL 8 specific script)
```
systemctl stop packagekit; yum install -y epel-release && yum install -y git binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms libvirt libvirt-devel ruby-devel libxslt-devel libxml2-devel libguestfs-tools-c ; mkdir ~/Vagrant ; cd ~/Vagrant ; curl -o  vagrant_2.2.6_x86_64.rpm https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm && yum install -y vagrant_2.2.6_x86_64.rpm && vagrant plugin install vagrant-guest_ansible ; vagrant plugin install vagrant-guest-ansible ; wget -O /etc/yum.repos.d/virtualbox.repo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo ; yum install -y VirtualBox-6.0 && systemctl start packagekit
```
##### If you're using RHEL 8, use the script below:
```
systemctl stop packagekit; dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm ; dnf install -y git binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms libvirt libvirt-devel ruby-devel libxslt-devel libxml2-devel libguestfs-tools-c ; mkdir ~/Vagrant ; cd ~/Vagrant ; curl -o  vagrant_2.2.6_x86_64.rpm https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm && dnf install -y vagrant_2.2.6_x86_64.rpm && vagrant plugin install vagrant-guest_ansible ; wget -O /etc/yum.repos.d/virtualbox.repo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo ; dnf install -y VirtualBox-6.0 && /usr/lib/virtualbox/vboxdrv.sh setup ; usermod -a -G vboxusers root ; systemctl start packagekit
```
##### Also, install the Virtualbox extension pack below
- [Install the Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)

## Windows/Fedora
- If using Windows:
- [Install the Latest Version of Vagrant](https://www.vagrantup.com/downloads.html)
- [Install the Latest Version of Virtualbox and Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)
- Then install the following vagrant plugin via PowerShell as Administrator `vagrant plugin install vagrant-guest_ansible`
- If using Fedora, run `dnf update -y` to update your system, then run the script below as root to install everything at once:
```
dnf -y install wget git binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms libvirt libvirt-devel ruby-devel libxslt-devel libxml2-devel ; wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo ; mv virtualbox.repo /etc/yum.repos.d/virtualbox.repo ; dnf install -y VirtualBox-6.0 ; usermod -a -G vboxusers ${USER} ; /usr/lib/virtualbox/vboxdrv.sh setup ; dnf -y install vagrant ; dnf remove -y rubygem-fog-core ; vagrant plugin install vagrant-guest_ansible
```

## Debian
_NOTE - If it's been awhile since you've run apt update, do that first. Reboot if the kernel was updated._

##### Install all at once by Copy/Pasting the below command into your terminal as root.
```
sudo snap install ruby ; sudo apt install ruby-bundler git -y; wget -c https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb ; sudo dpkg -i vagrant_2.2.6_x86_64.deb ; wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add - ; wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add - ; sudo add-apt-repository "deb http://download.virtualbox.org/virtualbox/debian bionic contrib"; sudo apt update; sudo apt install -y virtualbox-6.0 ; vagrant plugin install vagrant-guest_ansible
```
##### Also, install the Virtualbox extension pack below
- [Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)

_Now the deployment should be up and running!_

## (Recommended) Install Github Desktop to make pulling down changes easier
_NOTE this requires a free Github account_
1. Navigate to https://desktop.github.com/ and download Github Desktop.
2. Create or sign in to your account.
3. Click "Clone a repository from the Internet" and enter "rdbreak/rhce8env" and choose a location then "Clone".
4. You are also able to easily pull changes when they're made available.

## Notable Vagrant Commands to control the environment:
- `vagrant up` - Boots and provisions the environment
- `vagrant destroy -f` - Shuts down and destroys the environment
- `vagrant halt` - Only shuts down the environment VMs (can be booted up with `vagrant up`)
- `vagrant suspend` - Puts the VMs in a suspended state
- `vagrant resume` - Takes VMs out of a suspended state

## Other Useful Information:
You can also use the VirtualBox console to interact with the VMs or through a terminal. If you need to reset the root password, you would need to use the console. I'm constantly making upgrades to the environments, so every once and awhile run `git pull` in the repo directory to pull down changes. If you're using Windows, it's recommended to use Github Desktop so you can easily pull changes that are made to the environment. The first time you run the vagrant up command, it will download the OS images for later use. In other words, it will take longest the first time around but will be faster when it is deployed again. You can run `vagrant destroy -f` to destroy your environment at anytime. **This will erase everything**. This environment is meant to be reuseable, If you run the `vagrant up` command after destroying the environment, the OS image will already be downloaded and environment will deploy faster. Deployment should take around 15 minutes depending on your computer. You shouldn't need to access the IPA server during your practice exams. Everything should be provided that you would normally need during an actual exam. Hope this helps in your studies!
