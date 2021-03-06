.. _quickstart:

Quickstart
==========

This guide will walk you through the creation of a development environment using an example playbook and an example Vagrantfile. The result is a Vagrant virtual machine running Pulp from source code checked out on the host machine.

Host Requirements:
------------------

* Python 3.6
* Ansible 2.2

Other requirements vary depending on how you configure your Vagrantfile. If using ``Vagrantfile.example``, install vagrant, ansible, and the SSHFS plugin for vagrant. SSHFS will be used to share your local code directory with the deployed virtual machine::

    $ sudo dnf install vagrant ansible vagrant-sshfs vagrant-libvirt

Optional ProTip: The ``vagrant-hostmanager`` plugin is handy. If installed on the host and enabled in the Vagrantfile, the Pulp server will be accessable at ``dev.example.com``::

    $ sudo dnf install vagrant-hostmanager


Put Source Code on your Host
----------------------------

Use the :ref:`getsource` guide to put the code into a development directory on your *host machine*. If you use one of the default playbooks, ``pulp_devel_dir`` is set for you, see :ref:`variables`.

Provision a Vagrant Virtual Machine
-----------------------------------

Create a Vagrantfile in the root of the ``devel`` repository by copying ``Vagrantfile.example``.::

    $ cp Vagrantfile.example Vagrantfile

Have a look at your new Vagrantfile, notice that you can execute one or more ansible playbooks::

    ansible.playbook = "ansible/pulp-from-source.yml"

If you want to configure your environment differently, you can edit your Vagrantfile and any playbooks that it calls.

With a ``Vagrantfile`` in place, use the basic Vagrant commands to get started.

vagrant up
    Spin up a new Vagrant machine and provision as specified by the Vagrantfile.

vagrant destroy
    Remove the vm.

vagrant reload
    Reboot the vm.

vagrant provision
    Reprovision. For the default Vagrantfile, this will rerun the Ansible playbooks.

vagrant ssh
    Connect to your Vagrant machine with ssh.


    If you are using ``pulp-from_source.yml``, connecting to Vagrant will greet you with a message of the day and some helpful tips to get started.


Alternative: Vagrant--Docker Provision
--------------------------------------

Alternatively, one can deploy Pulp in a Docker container. Note please this
involves (automatically) downloading a Fedora container and rebuilding it in
order to update the ``vagrant`` user and group IDs to match the host user for the
mounts between the host and the container to work. Also note the host user has
to have permissions to interact with the docker daemon. See the `Docker
post-installation instructions
<https://docs.docker.com/install/linux/linux-postinstall/>`_ for a how-to.

vagrant up --provider docker
   Spin up Pulp in a docker container

Mind please the absence of ``sudo``. Also mind Docker mounts shared folders over
a loop-back device so no NFS/SSHFS is required.
