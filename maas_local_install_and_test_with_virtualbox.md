
# 1 VirtualBox Setup

This document is not very detailed since it's only for local test with VirtualBox which we will not use at all.

You will need virtualbox locally.

On mac, from the menu File - Host Network Manager (or shortcut cmd+w), create a host only adapter with preconfigured IP mask, for example, 172.16.1.1/24.

Create one VM for maas server, which will need two network interfaces, one for NAT to access public internet, the other one choose the host only created above.

Create another VM for testing maas enlistment. One nic is enough, use the same host only adapter as above. Note that for this host, you need to adjust the boot order to make sure network is selected and the first.

# 2 MAAS Server Installation

MAAS offers several installation options:

- Snap. The quickest and easiest way to install MAAS. Benefits from autonomous upgrades and direct access to beta and developmental versions
- Packages. Versatile deb-based installation with manual control over where specific components are placed, when upgrades are applied, and where packages are installed from
- Ubuntu Server ISO. Conveniently install MAAS as you provision a new server

For local installation in a VM, both deb package and ubuntu server iso are tested.

For future installation on MAAS physical server, both are OK; but regarding in the first release we probably will run a VM on the jump host, installing from ubuntu server iso is actually easier and one step less. So for local test, download the ubuntu server iso and install it in the maas VM created in step 1.

In the beginning of installation with iso, choose region controller. This will actually install an eIisntire MAAS environment. That is, it is equivalent to installing the 'maas' metapackage.

# 3 Initialization

Access the web UI via port 5240.

A new admin user needs to be created first.

You need to make sure both network interfaces are up in the maas server. If one is missing, config /etc/network/interface and reboot.

You should be able to see the subnet for the host only adapter, in which you are going to plugin new servers and let maas do auto enlistment.

I had to make Virtualbox save a video of the node’s boot process to find out that the node was trying to get a file from the MAAS server on the wrong interface, the NAT one that I configured for accessing the internet, not the one that is shared between server/node (the host only adapter).

Solution is to edit `/etc/maas/regiond.conf` and set:

```
maas_url: http://the_host_only_nic_ip:5240/MAAS
```

# 4 Adding a New Host into the Subnet

Now it's time to launch a new VM in the same subnet (host only adapter) which is the second VM you created in Step 1.

Since we already prepared it for netboot in step 1, here is what happens next:

- DHCP server is contacted
- kernel and initrd are received over TFTP
- machine boots
- initrd mounts a Squashfs image ephemerally over HTTP
- cloud-init runs enlistment scripts
- machine shuts down

At the end it will shut down and this is normal.

In maas machines tab, you should already see it, but you can't boot it because maas does not support virtualbox power type. There are plugins for that in github but that was relatively out of date and it seems the directory structure for maas has already changed since then so I did not give it a try.

So choose your newly enlisted host, edit the power type to manual.

# 5 Commission

The commissioning scripts will talk to the region API server to ensure that everything is in order and that eventual deployment will succeed.

Choose the new machine and in actions, choose commission.

Since this is virtualbox env, you will have to click the VM and power it on.

After commission is done, it's in ready state, and you can see the machine info in the dashboard.

# 6 Deploy

Choose the new machine and from actions choose deploy, and then select what operating system you want to install.

