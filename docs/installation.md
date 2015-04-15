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
This is caused by the fact that debian NFS packages are transitioning from using portmapper to using rpcbind, but the package maintainers have not effected the required changes in the init.d scripts. You can fix this by:
```
cd /etc/init.d
sudo sed -i 's%$portmapper%$rpcbind%g' nfs-common nfs-kernel-server umountnfs.sh
sudo apt-get -f install
```
