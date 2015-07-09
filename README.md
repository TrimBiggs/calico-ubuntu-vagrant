# Calico Docker libnetwork Ubuntu Vagrant example

This example shows how to network Docker containers using Calico with Docker's new [libnetwork network driver support](https://github.com/docker/libnetwork) that was introduced on the Docker [experimental channel](https://github.com/docker/docker/tree/master/experimental) alongside the Docker 1.7 release.  Docker's experimental channel is moving fast and some of its features are not yet fully stable.  For this reason this vagrant example currently uses a specific Docker experimental build with a specific build of Calico (rather than tracking master in both the [docker](https://github.com/docker/docker) and [calico-docker](https://github.com/Metaswitch/calico-docker) repos).

For a more stable version of Calico's integration with Docker, see [the Calico CoreOS example](https://github.com/Metaswitch/calico-coreos-vagrant-example) based on Powerstrip.

### A note about names & addresses
In this example, we will use the following server names and IP addresses.

| hostname  | IP address   |
|-----------|--------------|
| ubuntu-1  | 172.17.8.101 |
| ubuntu-2  | 172.17.8.102 |

## Set up your cluster

1) Install dependencies

* [VirtualBox][virtualbox] 4.3.10 or greater.
* [Vagrant][vagrant] 1.6 or greater.
* [Git][git]

2) Clone this project and get it running!

    git clone https://github.com/Metaswitch/calico-ubuntu-vagrant.git
    cd calico-ubuntu-vagrant

3) Startup and SSH

There are two "providers" for Vagrant with slightly different instructions.
Follow one of the following two options:

**VirtualBox Provider**

The VirtualBox provider is the default Vagrant provider. Use this if you are unsure.

    vagrant up

**VMware Provider**

The VMware provider is a commercial addon from Hashicorp that offers better stability and speed.
If you use this provider follow these instructions.

VMware Fusion:

    vagrant up --provider vmware_fusion

VMware Workstation:

    vagrant up --provider vmware_workstation

To connect to your servers
* Linux/Mac OS X
    * run `vagrant ssh <hostname>`
* Windows
    * Follow instructions from https://github.com/nickryand/vagrant-multi-putty
    * run `vagrant putty <hostname>`

4) Verify environment

You should now have two Ubuntu servers, each running etcd in a cluster. The servers are named ubuntu-1 and ubuntu-2.

At this point, it's worth checking that your servers can ping each other.

From ubuntu-1

    ping 172.17.8.102

From ubuntu-2

    ping 172.17.8.101

If you see ping failures, the likely culprit is a problem with the VirtualBox network between the VMs.  You should check that each host is connected to the same virtual network adapter in VirtualBox and rebooting the host may also help.  Remember to shut down the VMs with `vagrant halt` before you reboot.

You should also verify each host can access etcd.  The following will return an error if etcd is not available.

    curl -L http://localhost:4001/version

## Try out Calico Networking

Now you have a basic two node CoreOS cluster setup and you are ready to try Calico neworking.

Follow the step by step [Getting Started][using-calico] instructions in the main calico-docker repo.

[calico-ubuntu-vagrant]: https://github.com/Metaswitch/calico-ubuntu-vagrant-example
[virtualbox]: https://www.virtualbox.org/
[vagrant]: https://www.vagrantup.com/downloads.html
[using-calico]: https://github.com/Metaswitch/calico-docker/blob/master/docs/GettingStarted.md
[git]: http://git-scm.com/
