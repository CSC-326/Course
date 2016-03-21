Configuration Management Workshop
----------------------------------

The goal of this workshop is to demonstrate how to use some tools to aid with the creation of virtual machines and configure software on them.  At the end of this workshop, you should be able to automatically create a virtual machine and configure it to run a simple web server.

# Creating Virtual Machine

Install [vagrant](https://www.vagrantup.com/downloads.html).

You will also need a [virtual machine provider](https://docs.vagrantup.com/v2/providers/). [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is a recommended provider.

Initialize a virtual machine. `ubuntu/trusty64` is one default image. A list of other virtual machine images can be found [here](https://atlas.hashicorp.com/boxes/search).

    vagrant init ubuntu/trusty64

Start up the virtual machine.

    vagrant up

Then    

    vagrant ssh

You should be able to connect to the machine.

# Configuration with Ansible

Ansible is a tool for configuring and coordinating software on multiple machines.
In general, Ansible (like Salt, Chef, and Puppet), use a central server that controls other nodes.  Unlike the other tools, Ansible does not require a client service to run on the nodes.

### Let's setup some stuff first

We want to create a master server that runs Ansible. First, use a binary package manager to setup some basic stuff missing.

    sudo apt-get update
    sudo apt-get -y install git make vim python-dev python-pip


Now get ansible itself.

    git clone git://github.com/ansible/ansible.git --recursive
    cd ./ansible
    source ./hacking/env-setup

Install dependencies, using pip, a package manager for Python.
    
    sudo pip install paramiko PyYAML Jinja2 httplib2 six

Build the software.

    sudo make install

Test ansible

    ansible all -m ping

**Note, you should not expect to see anything working, quite yet, this just makes sure, python, etc. is setup properly.**

# Node Machine

**Exit your virtual machine, or create new terminal in your host machine**.

We now want to create a new node that will be serving as our web server.  Normally, this could be a server hosted on AWS or a droplet on digitalocean.  Instead, for testing purposes, we will be creating another local virtual machine. Follow the instructions we previously followed for creating a virtual machine, but with some exceptions:

#### Creating node virtual machine.

Vagrant only allows one virtual machine configuration per directory. Make a new directory, called node, then run the commands in there.

#### Enabling a reachable ip address.

Virtual machines typically have [four ways](http://catlingmindswipe.blogspot.com/2012/06/how-to-virtualbox-networking-part-two.html) to set up the network:
- Network Address Translation (NAT), *which is the default*,
- Bridged,
- Internal network
- Host Only.

Unlike the first virtual machine, you cannot use the default mode, because there will be no way for the ansible VM to talk to the node VM. Instead, you much choose between bridged or host-only.

##### Host-only

For linux/mac:
Uncomment the following line in Vagrantfile:
```
config.vm.network "private_network", ip: "192.168.33.10"
```
Then run, `vagrant reload`. **Bonus**: You can use hard-coded ip address in your scripts.

##### Bridged
For windows:
Uncomment the following line in Vagrantfile:
```
config.vm.network "public_network"
```
Then run, `vagrant reload`

* When launching the VM, you may be asked which network to bridge off of, you can use the same interface as the "wireless" network, usually option 1.

* Inside the VM, run `ifconfig` and note the second ip address listed.

#### Setting up ssh keys

**You will need the following information to help connect to this node.**

* On the host machine, run `vagrant ssh-config` to get path of the private_key (IdentityFile), open it up and copy contents into textfile. In *nix, you can run `pbcopy < path/private_key` to copy contents into clipboard.

# Back to Ansible Server

#### Creating a private key to slave nodes.

You need a way to automatically connect to your server without having to manually authenicate each connection. Using a public/private key for ssh, you can ssh into your node VM from the Ansible Server automatically.

Create a `keys/node0.key` file that contains the private_key you previously copied.  You may need to `chmod 500 keys/node0.key`.

#### Creating an inventory file

An inventory file allows ansible to define, group, and coordinate configuration management of multiple machines. At the most basic level, it basically lists the names of an asset and details about how to connect to it.

Create a `inventory` file that contains something like the following.  **Note use your ip address and private_key**:
    
    node0 ansible_ssh_host=192.168.1.103 ansible_ssh_user=vagrant ansible_ssh_private_key_file=./keys/node0.key

#### Testing connection

Now, run the ping test again to make sure you can actually talk to the node!

    ansible all -m ping -i inventory -vvvv

#### Performing configuration management
    
Let's install a web server, called nginx (say like engine-X), on the node.

    ansible all -s -m apt -i inventory -a 'pkg=nginx state=installed update_cache=true'
    
Start the web server.
    
    ansible all -s -m shell -i inventory  -a 'nginx'

Open a browser and enter in your node's ip address, e.g. http://192.168.1.103/

Removing nginx.

    ansible all -s -m apt -i inventory -a 'pkg=nginx state=absent update_cache=true'

Actually, nginx is a metapackage, show you also need to run this:

    ansible all -s -m shell -i inventory -a 'sudo apt-get -y autoremove'
    
Webserver should be dead.

# Programming Ansible

Instead of running shell commands, you can directly use Ansible's Python API.  For example, this snippet will execute the simple ping test you've been using before.  See if you can adapt this code snippet to perform the `nginx` install instead of running a shell script.

```
import ansible.runner
from ansible.inventory import Inventory

runner = ansible.runner.Runner(
   module_name='ping',
   module_args='',
   pattern='*',
   forks=10,
   inventory=Inventory('inventory')
)
datastructure = runner.run()
print( datastructure )
```    
    
# Errors

Some errors you may receive:

Init should initiatiate a download of the box, if not, you can manually add it as follows:

    vagrant box add precise32 http://files.vagrantup.com/precise32.box

If you see:

> No usable default provider could be found for your system.
> Vagrant relies on interactions with 3rd party systems, known as
"providers", to provide Vagrant with resources to run development
environments. Examples are VirtualBox, VMware, Hyper-V.

Then make sure to install a virtual machine system,  [e.g. VirtualBox](https://www.virtualbox.org/wiki/Downloads).

    vagrant up

> The guest machine entered an invalid state while waiting for it
to boot. In Virtual Box, can see: "Failed to load VMMR0.r0 (VERR_VMM_SMAP_BUT_AC_CLEAR)."

* Latest version of Mac OS/laptops may have trouble with current VirtualBox release. Instead, download one of the [test builds](https://www.virtualbox.org/wiki/Testbuilds). 

* If you change your Vagrantfile, and not seeing a change (public ip address), then you need to run `vagrant reload` so that it can reboot machine and read config file.

* If you have trouble connecting to your node, simply run `ssh -i keys/node0.key 10.139.67.140` to make sure you can even ssh into your machine. Make sure you're using private version of key, and it has chmod 500 permissions.

* If you have trouble getting a second ip, e.g., the node's ip, make sure you've properly bridged your connection. Inside the VM, you can't talk to the other VM's localhost, or even your hosts ip address (unless you've setup port forwarding). You need to allocate a ip address just for the node that is visible to everyone on your host machine, so you can see it while you're in the ansible VM. Incidentally, you know ssh-ing into the node machine can work, because you did `vagrant ssh` to get in the first place.
