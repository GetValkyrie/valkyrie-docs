VALKYRIE
========

INSTALLATION
------------

Installing Valkyrie is as simple as:

    $ drush dl valkyrie

*N.B.* Valkyrie supports both Linux and OSX operating systems. We have no plans
to support Windows.


DEPENDENCIES
------------

Valkyrie is a Drush extension, and thus requires a recent version of Drush. We
currently recommend using Drush 7.x. Earlier versions *may* work, but are not
currently well-tested or supported. Note that Drush can be installed stand-
alone, if you are unable or unwilling to upgrade your system install.

 * https://github.com/drush-ops/drush#installupdate---a-global-drush-for-all-projects

Valkyrie depends on Vagrant, which in turn requires Virtualbox. These projects
publish their own packages for most operating systems. We recommend installing
these, as it will ensure that you are running on recent versions, and thus can
take advantage of newer features.

 * https://www.vagrantup.com/downloads.html
 * https://www.virtualbox.org/wiki/Downloads

Valkyrie also makes use of some Vagrant plugins: vagrant-triggers, and (for Mac
users only) vagrant-dns. Installing these should be as simple as:

    $ vagrant plugin install vagrant-triggers
    $ vagrant plugin install vagrant-dns    # OSX only

Valkyrie also uses git quite extensively. So if you don't already have it
installed, go ahead and do that next.

 * http://git-scm.com/downloads

### Notes for Linux 
**NFS server installed on the host is a requirement for valkyrie**
* On Debian/Ubuntu based distributions, install the nfs-kernel-server package.
* On RedHat based distributions, here is a [good guide](http://computernetworkingnotes.com/network-administration/how-to-configure-nfs-server-in-rhel-6.html) for installing and configuring the nfs server.

**Authorize NFS connections through your host's firewall to the VM**
* Here is a [debian oriented guide](https://wiki.debian.org/SecuringNFS) to force the NFS daemons to run on known ports so you can configure the host's firewall appropriately
* Here is a [redhat oriented guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/s2-nfs-nfs-firewall-config.html) for forcing NFS to run on known ports.
* If you use ufw as your firewall frontend, and you are not that bothered about trusting an embedded VM network, then allow the entire VM network
```
sudo ufw allow from 10.42.0.0/24
sudo ufw allow to 10.42.0.0/24
```
**Fix nfs install issues on Debian**

On Debian wheezy/squeeze, if you encounter errors like the following while installing nfs packages:
```
insserv: Service portmap has to be enabled to start service nfs-common
insserv: exiting now!
update-rc.d: error: insserv rejected the script header
```
This is caused by the fact that debian nfs packages are transitioning from using portmapper to using rpcbind, but the package maintainers have not effected the required changes in the init.d scripts. You can fix this by:
```
cd /etc/init.d
sudo sed -i 's%$portmapper%$rpcbind%g' nfs-common nfs-kernel-server umountnfs.sh
sudo apt-get -f install
```

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

**Working in the VM**
* Logon to the VM, by typing 'vagrant ssh <vm-id>'
* This will log you in as user 'vagrant'
* To become root, just type `sudo -s`, or preferably prepend sudo only when you need to run privileged commands
* To see the nfs shares the vm is mounting from the host `df -h -t nfs`
* /var/aegir/platforms is mounted off the host, and you can develop your platform code in that nfs-shared directory on the host .. perhaps define your IDE project space there.  Each platform is basically a drupal install tree, which could also be created off makefiles, either from the host or the vm.

