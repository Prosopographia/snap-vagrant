# Ansible Vagrant setup for a local SNAP:DRGN development virtual machine

**This does not handle live server provisioning!**

## Getting Started

Vagrant and VirtualBox (or some other VM provider) can be used to quickly build or rebuild virtual servers.
This Vagrant profile installs Apache, and the 4Store triplestore using the [Ansible](http://www.ansible.com/) provisioner.

To use the vagrant file, you will need to have done the following:

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  1. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  1. Install [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)
  1. Checkout this repository
  1. Open a shell prompt (Terminal app on a Mac) and cd into the repository folder (containing the `Vagrantfile`)
  1. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`
  1. Once all of that is done, type in `vagrant up`, and Vagrant will create a new VM, install the base box, and configure it.

Once the new VM is up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into it via SSH if you'd like by typing in `vagrant ssh`. Otherwise, the next steps are below.

### Importing RDF data into 4store

The VM checks out the snap code and data from github into a local shared `docroot` directory, but does not import data into the 4store triplestore. To do this there is a separate playbook called `import-rdf.yml`. To import the RDF data into the 4store service run this playbook separately by typing:

`ansible-playbook provisioning/import-rdf.yml -i provisioning/inventory`

### Notes on 4store configuration

Documentation about the 4store triplestore is currently only available (as of 2018-11) via a mirror site at https://4store.danielknoell.de/trac/wiki/. 4store is installed by the provisioning playbook using `apt-get install 4store` which configures a 'fourstore' user in Ubunutu, as well as the init.d config files, etc. so 4store can be started and stopped via the systemctl commands:

`sudo systemctl start 4store`
`sudo systemctl stop 4store`

The provisioning playbook sets the port to 9000 and creates and sets the default triplestore to 'snap'. The data directory for 'snap' is:

`/var/lib/4store/snap`

and this direcotry needs to be writable by the 'fourstore' user so importing data/reading metadata files work properly.

**To import the SNAP RDF data the 4store httpd daemon cannot be running**. You must stop all 4store processes using the systemctl stop command and run only the backend using `sudo -H -u fourstore 4s-backend snap` to ensure the daemon is running under the 'fourstore' user. If you run `4s-backend` without specifying the user the process will be owned by, probably 'vagrant' or 'root' and you will have problems importing data because of the permissions dissonance between the filesystem and the daemon process.

The `import-rdf.yml` playbook is written so you don't need to worry about this or ssh into the VM to import any data youself. You can simply run the ansible-playbook command to import/re-import the data into your local development SNAP virtual machine.

### 4store triplestore info

When running, the triplestore should be accessible via:

```
192.168.33.33:9000         - base port
192.168.33.33:9000/status  - info
192.168.33.33:9000/query   - web query
192.168.33.33:9000/sparql  - endpoint
```





## Author Information

Original Vagrant/Ansible playbooks created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).

Modified for the 2018 SNAP November hackathon: https://github.com/Prosopographia