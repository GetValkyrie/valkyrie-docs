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
currently recommend using the latest Drush 7.x. Earlier versions *may* work,
but are not currently well-tested or supported. Note that Drush can be
installed stand- alone, if you are unable or unwilling to upgrade your system
install.

 * https://github.com/drush-ops/drush#installupdate---a-global-drush-for-all-projects

Valkyrie depends on Vagrant, which in turn requires Virtualbox. These projects
publish their own packages for most operating systems. We recommend installing
these, as it will ensure that you are running on recent versions, and thus can
take advantage of newer features.

 * https://www.vagrantup.com/downloads.html
 * https://www.virtualbox.org/wiki/Downloads

Valkyrie also uses git quite extensively. So if you don't already have it
installed, go ahead and do that next.

 * http://git-scm.com/downloads
