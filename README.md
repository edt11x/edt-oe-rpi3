To build a Raspberry Pi 3B+ OpenEmbedded image, you will need the following:

* A Linux host machine
* Git
* A C compiler
* A Python interpreter
* At least 40GB of free disk space

Getting the meta layers

All meta layers are usually available via git. Don’t worry for now, if you have no clue what git means. We’re just using one command for now. The git clone command gets the repository from the internet. The -b dunfell switch we're using specifies which version to get. At the time of writing the dunfell version is the most recent version with long-term support. See https://wiki.yoctoproject.org/wiki/Releases for an overview of the most recent releases.

Before getting any meta layers we’re gonna create a project folder named yocto and a folder named source for all the meta layers inside.

mkdir yocto
cd yocto
mkdir sources

The meta layer we’re always needing is poky. It contains all the basic stuff for yocto to work. We get it by executing the following git command.

git clone git://git.yoctoproject.org/poky -b dunfell

For the Raspberry Pi, there is a nicely made meta layer with all the definitions needed to run the Raspberry Pi. We get it with this command.

git clone git://git.yoctoproject.org/meta-raspberrypi -b dunfell

Meta layers always state on which meta layers they depend. The readme of meta-raspberrypi states that we need poky, which we already have, and meta-openembedded. This meta-layer itself is divided into multiple layers. We get them all by this command.

git clone https://git.openembedded.org/meta-openembedded -b dunfell

Those are all the meta layers we need. Let’s go back to our project folder and initialize the build environment.

cd .. 
. sources/poky/oe-init-build-env

We’re almost ready to build our first image. We only need to edit two configuration files.

The bblayers.conf file in the conf folder contains all the path to the meta layers we're gonna use.

nano conf/bblayers.conf

Last but not least, the local.conf file in the conf folder does contain some basic configurations and, most important for us, the name of the machine to build for. If we want to build for the Raspberry Pi 4, we're using raspberrypi4, for the Raspberry Pi 3, raspberrypi3. This is one of the big benefits of using Yocto. It's just a matter of changing one configuration line to build the same Image for another system.

nano conf/local.conf[...]MACHINE ?= "raspberrypi4"[...]

With all these steps done, it’s time to start the actual build and get some (or multiple) coffee(s) while we wait for it to be finished.

bitbake core-image-base

Congratulations! You’ve built your first Linux Image for the Raspberry Pi. You’ll find the completed image under tmp/deploy/images/repberrypi4/core-image-base-raspberrypi4.wic.bz2 This file is compressed. To decompress it, use the bzip2 command or a tool like 7zip.

bzip2 -d -f tmp/deploy/images/raspberrypi4/core-image-base-raspberrypi4.wic.bz2

You only have to flash it to an SD-Card like any other Raspian image, and you’re ready to boot. https://www.raspberrypi.org/documentation/installation/installing-images/

By default, the username is root with an empty password.
