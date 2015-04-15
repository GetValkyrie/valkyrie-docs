Post-install
------------

**Important installer messages**

At the end of the installation process, the installer will display messages like:
```
==> valkyrie: TASK: [getvalkyrie.valkyrie | Output the deploy key] ************************** 
==> valkyrie: ok: [localhost] => {
==> valkyrie:     "var": {
==> valkyrie:         "ssh_public_key_output.stdout_lines": [
==> valkyrie:             "/////////////////////////////////////////////////////////////////////////////",
==> valkyrie:             "  Add the following deploy key to your project on Github:",
==> valkyrie:             "=============================================================================",
==> valkyrie:             "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCosL1qmesmA55S2ZB3fEdsLQSElHOARcXpu7ZeT5vX+kpKb6OQQ/6zQ9I5nx1GaZORkwlKy4qNxhmUiy7u/aIiPALQlL2m9y7brqFxewPMCcX5vhV84rW/qxieJzx3nSC0s99ui+vnYhylbwBapJmfpUcuCMJzyP+JlusRYTLyk/uIrDtewnk2PgAtFYF/gUyK6It/RIbwdtawXrW/MQuFuYPpVYnvG+bbqmx+t7HBISWBbm/DK2guKhXzkUhdD24sdPp9tjUfLfsWYB3Plh5mhOM8I4cTwjVOxGwVfwC+KiEAxlRbSihsqUaRdIr7AXfZ4pG/b6MelwlcX3W61tvz Valkyrie deploy key",
==> valkyrie:             "=============================================================================",
==> valkyrie:             "/////////////////////////////////////////////////////////////////////////////"
==> valkyrie:         ]
==> valkyrie:     }
==> valkyrie: }
==> valkyrie: 
==> valkyrie: TASK: [getvalkyrie.valkyrie | Output the login link] ************************** 
==> valkyrie: ok: [localhost] => {
==> valkyrie:     "var": {
==> valkyrie:         "aegir_login_link_output.stdout_lines": [
==> valkyrie:             "/////////////////////////////////////////////////////////////////////////////",
==> valkyrie:             "  You can access the front-end of your Aegir install at:",
==> valkyrie:             "=============================================================================",
==> valkyrie:             "http://valkyrie.local/user/reset/1/1429111863/uAvC9VJx-l2CqhaufWg8ElCSFWzfdTiUTxJOQJ-pbz4/login",
==> valkyrie:             "=============================================================================",
==> valkyrie:             "/////////////////////////////////////////////////////////////////////////////"
==> valkyrie:         ]
==> valkyrie:     }
==> valkyrie: }
```

* Use the login link as a one-time access to the now deployed aegir frontend, and reset the admin password immediate.  Don't forget to tick the check boxes for all the roles you would want the admin user to be able to play on the aegir server.
* Copy the displayed deploy ssh key, and add it to your project on github.  This will allow you to publish to the project from valkyrie.

### Looking Around the system

**The directory in which vagrant commands are executed is significant**
* The default location for vagrant base boxes is ~/.vagrant/machine, but valkyrie does not use that
* The base directory for valkyrie vagrant vm is the directory in which you invoked the command `drush vmnew valkyrie`
* To see all vagrant instances for your user account, from anywhere type `vagrant global-status` .. the display of this command is cached and might show recently deleted VMs
* From anywhere you can run vagrant commands against any vm, by postfixing the id displayed by vagrant global-status.  For example:
```
vagrant destroy vmid
vagrant ssh vmid
etc 
```
* From a given install base, you can issue vagrant commands without specifying a vm id, and the commands will be run against the vm defined in the Vagrantfile within that directory
* You can further customize the vm, by editing the Vagrant file

**Working in the VM**
* Logon to the VM, by typing 'vagrant ssh vmid'
* This will log you into the vm's bash shell as user 'vagrant'
* To become root, just type `sudo -s`, or preferably prepend sudo only when you need to run privileged commands
* To see the nfs shares the vm is mounting from the host `df -h -t nfs`
* /var/aegir/platforms is mounted off the host, and you can develop your platform code in that nfs-shared directory on the host .. perhaps define your IDE project space there.  Each platform is basically a drupal install tree, which could also be created off makefiles, either from the host or the vm.

