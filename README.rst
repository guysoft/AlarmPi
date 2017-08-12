AlarmPi
=======

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution to to run `AlarmBot : Crowdsourced EvolvingAr t <https://github.com/guysoft/AlarmBot>`_ out of the box and the scripts necessary to load it at boot. This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image. `You can download a built image here <http://unofficialpi.org/Distros/AlarmPi>`_

AlarmPi is based on `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_

Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/AlarmPi>`_


How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``alarmpi-network.txt`` or ``alarmpi-wpa-supplicant.txt`` on the root of the flashed card when using it like a flash drive
#. Set a telegram bot token in ``config.ini``
#. Boot the Pi from the SD card


Requirements
------------
* Raspberrypi 1/zero and newser or device running Armbian and an internet connection.
* 2A power supply
* Speakers connected to the Pi.


Features
--------

* An IOT alarm that you can set and edit using Telegarm.

Developing
----------

Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `Raspbian <http://www.raspbian.org/>`_ image.
#. root privileges for chroot
#. Bash
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)

Build AlarmPi From within Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

AlarmPi can be built from Debian, Ubuntu, Raspbian.
Build requires about 2.5 GB of free space available.
You can build it by issuing the following commands::

    sudo apt-get install realpath qemu-user-static
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/guysoft/AlarmPi.git
    cd AlarmPi/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspbian_lite_latest'
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building AlarmPi Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

AlarmPi supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build AlarmPi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd AlarmPi/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd AlarmPi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd AlarmPi/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building AlarmPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
