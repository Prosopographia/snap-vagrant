# SNAP:DRGN development virtual machine

Ansible Vagrant setup for a local triplestore instance of SNAP:DRGN.

**This does not handle live server provisioning!**

## Getting Started

Vagrant and VirtualBox (or some other VM provider) can be used to quickly build or rebuild virtual servers.
This Vagrant profile installs Apache, and the 4Store triplestore using the [Ansible](http://www.ansible.com/) provisioner.

To use the vagrant file, you will need to have done the following:

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  1. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  1. Install [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)
  1. Checkout this repository
  1. Open a shell prompt (Terminal app on a Mac) and cd into the repository folder (which contains the `Vagrantfile`)
  1. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install --force -r requirements.yml`
  1. Once all of that is done, type `vagrant up`, and Vagrant will create a new VM, install the base box, and configure it.

Once the new VM is up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into it via SSH if you'd like by typing in `vagrant ssh`. Otherwise, the next steps are below.

## Importing RDF data into 4store

The VM checks out the snap code and data from github into a local shared `docroot` directory, but does not import data into the 4store triplestore. To do this there is a separate playbook called `import-rdf.yml`. To import the RDF data into the 4store service run this playbook separately by typing:

`ansible-playbook provisioning/import-rdf.yml -i provisioning/inventory`

### Notes on 4store configuration

*The `import-rdf.yml` playbook is written so you don't need to worry about manually performing these steps to import any data youself. You can simply run the ansible-playbook command above to import/re-import the data into your local development SNAP virtual machine. These notes are here for documentation purposes because it took ruddy ages to figure out the first time!*

Documentation about the 4store triplestore is currently only available (as of 2018-11) via a mirror site at https://4store.danielknoell.de/trac/wiki/. 4store is installed via the provisioning playbook (which uses `apt-get install 4store` in the VM. The package manager install configures a 'fourstore' user in Ubunutu, as well as the init.d config files, etc. This means 4store can be started and stopped via the systemctl commands:

`sudo systemctl start 4store`
`sudo systemctl stop 4store`

The provisioning playbook sets the port to 9000 and creates and sets the default triplestore to 'snap'. The data directory for 'snap' is:

`/var/lib/4store/snap`

and this directory needs to be writable by the 'fourstore' user so importing data/reading metadata files work properly. This directory is internal to the client VM and not a shared folder with the host machine (like docroot is).

**To import the SNAP RDF data the 4store httpd daemon cannot be running**. You must stop all 4store processes using the systemctl stop command and run only the backend using `sudo -H -u fourstore 4s-backend snap` to ensure the daemon is running under the 'fourstore' user. If you run `4s-backend` without specifying the user the process will be owned by, probably 'vagrant' or 'root' and you will have problems importing data because of the permissions dissonance between the filesystem and the daemon process.

## 4store triplestore info

With the VM running the SNAP:DRGN triplestore should be accessible via:

```
192.168.33.33:9000         - base port
192.168.33.33:9000/status  - info
192.168.33.33:9000/query   - web query
192.168.33.33:9000/sparql  - endpoint
```

## Django webapp

These provisioning playbooks are not currently configured to set the web app up. TODO for someone else. ;-)

## Author Information

Original Vagrant/Ansible playbooks created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).

Modified for the 2018 SNAP November hackathon: https://github.com/Prosopographia