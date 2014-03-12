salt-sandbox
============

Salt Sandbox is a multi-VM [Vagrant](http://vagrantup.com/)-based
[Salt](http://saltstack.org) development environment used for creating
and testing new Salt state modules outside of your production environment.
It's also a great way to learn firsthand about Salt and its remote
execution capabilities.

Salt Sandbox will set up three separate virtual machines:

* _salt.example.com_ - the Salt master server
* _minion1.example.com_ - the first Salt minion machine
* _minion2.example.com_ - the second Salt minion machine

These VMs can be used in conjunction to segregate and test your modules
based on node groups, top file environments, grain values etc. You can
even test modules on different Linux distributions or release versions to
better match your production infrastructure.

Requirements
============

To use Salt Sandbox, you must have the following items installed and
working:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://vagrantup.com/)

Salt Sandbox has been designed for and tested with Vagrant base boxes
running:

* Ubuntu 12.04 Precise Pangolin

Usage
=====

Make sure you have a compatible Vagrant base box (if you don't have one
already, it will download a 64-bit Ubuntu 12.04 box for you), and then you
should be good to clone this repo and go:

    $ vagrant box list
    precise64       (virtualbox)
    $ git clone git://github.com/tlyng/salt-sandbox.git
    $ cd salt-sandbox

Initial Startup
---------------

To bring up the Salt Sandbox environment, issue the following command:

    $ vagrant up

The following tasts will be handled automatically:

1. The Salt master daemon will be installed and enabled on the master machine.
2. The _srv_ directory will be mounted as _/srv_ on the master.
3. The Salt minion daemon will be installed and enabled on all minions.
4. A private network will be set up with all machines knowing how to
   communicate with each other.
5. The minion public keys will be automatically accepted by the master server.
6. Minions will run _state.highstate_ when initialized.

All of this is handled using Vagrant's provisioning capabilities and is controlled by the configs in the `salt/` directory. State tree and pillars are available in the `srv/` folder.

Developing new modules
----------------------

To start developing a new SLS modules, just create the standard module structure under `srv/salt/` in your salt-sandbox checkout. This directory is automatically in the Salt master's `file_roots` path and any changes will be picked up immediately.

    $ mkdir -p srv/salt/mymodule
    $ vim srv/salt/mymodule/init.sls

To have your module actually applied to one or more of the minions, edit the `top.sls`file and specify how it should be used during state exectuion.

Check your handiwork
--------------------

To log on to the virtual machines and see the result of your Salt modules, just use standard [Vagrant Multi-VM Environment](http://vagrantup.com/docs/multivm.html) commands, and provide the proper VM name (`master`, `minion1` or `minion2`):

    $ vagrant ssh master

Then instruct all minions to execute a highstate call and apply any applicable modules:

    [vagrant@master ~]$ sudo salt '*' state.highstate

License
=======
Salt Sandbox is provided under the terms of [The MIT License](http://www.opensource.org/licenses/MIT).

Copyright &copy; 2014, [Torkel Lyng](mailto:torkel.lyng@gmail.com)
