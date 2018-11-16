# Ansible Vagrant setup for a local SNAP:DRGN development virtual machine

**This does not handle live server provisioning!**

## Getting Started

Vagrant and VirtualBox (or some other VM provider) can be used to quickly build or rebuild virtual servers.
This Vagrant profile installs Apache, and the 4Store triplestore using the [Ansible](http://www.ansible.com/) provisioner.

To use the vagrant file, you will need to have done the following:

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  3. Install [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)
  4. Open a shell prompt (Terminal app on a Mac) and cd into the folder containing the `Vagrantfile`
  5. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`

Once all of that is done, you can simply type in `vagrant up`, and Vagrant will create a new VM, install the base box, and configure it.

Once the new VM is up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into it via SSH if you'd like by typing in `vagrant ssh`. Otherwise, the next steps are below.

### Setting up your hosts file

The machine is based on Geerlingguy's LAMP server (with the PHP bits removed) and the VM is configured to run on http://192.168.33.33

You can modify your host machine's hosts file to something different (Mac/Linux: `/etc/hosts`; Windows: `%systemroot%\system32\drivers\etc\hosts`), by adding the line below:

    192.168.33.33  snap

(Where `snap` is the hostname configured in the `Vagrantfile`).

After that is configured, you could visit http://snap/ in a browser, and you'll see the SNAP Django site.

### SNAP setup

The VM checks out the snap code and data from github into a local shared `docroot` directory.




## Author Information

Original Vagrant/Ansible playbooks created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).

Modified for the 2018 SNAP November hackathon: https://github.com/Prosopographia