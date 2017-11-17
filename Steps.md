# Steps to Run

## Install required tools

* Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Install [vagrant](https://www.vagrantup.com/downloads.html)

## Setup Project

### Clone this repository:
* $ `git clone https://www.vagrantup.com/downloads.html`
* $ `cd HW5`

### Setup Client VM
Go to the client folder and start the Client VM. Connect to the VM and then exit if everything is running:
* $ `cd node0`
* $ `vagrant up`
* $ `vagrant ssh`
* $ `exit`

Check Client VM config
* $ `vagrant ssh-config`

**Note**: Check the path for the `private_key` present in the value `IdentityFile`. Copy the contents of this private key for later use in Ansible VM.

### Setup Configuration Server VM running Ansible
Go to the ansible folder and start the Ansible VM. Then, connect to the VM:
* $ `cd ansible`
* $ `vagrant up`
* $ `vagrant ssh`

Install Ansible
* $ `sudo apt-add-repository ppa:ansible/ansible`
* $ `sudo apt-get update`
* $ `sudo apt-get install ansible`

All the data files should already be present in the `share` directory inside the ansible VM:
* $ `cd share`

This folder should contain
* playbook.yml
* inventory
* keys/node0.key // the private key of the client VM needs to be copied inside this file.
* stopforever.yml

**Note:** At this point, copy the contents of the `private_key` that we copied from the Client VM inside the `keys/node0.key` file.

Give proper permissions to the key file:
* $ `chmod 600 keys/node0.key`

check if ansible is able to ping the Client VM and you are able to ssh to the Client VM
* $ `ansible all -m ping -i inventory`
* $ `ssh -i keys/node0.key vagrant@192.168.33.11`

**Note**: here 192.168.33.11 is the IP address of the client VM

Exit from the client vm:
* $ `exit`

### Run playbook.yml from Ansible playbook:
* $ `ansible-playbook -i inventory playbook.yml -s`

Check that the app is running properly on the forever server: Open Chrome and go to `192.168.33.11:3001` The result should besomething like:

![output](media/screen.png)

To stop the running forver server, run the stopforever.yml playbook:
* $ `ansible-playbook -i inventory stopforever.yml -s`


