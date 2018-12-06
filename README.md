# MAAS

Everything that is related to MAAS automation.

## 0 Local Dependencies

- VirtualBox and Vagrant (used only for the `local` inventory)
- Ansible (used both for local, and on the maas-server)

## 1 Inventories

- local

The local one is only used for some local test and play with MAAS.

More detailed steps see [HERE](maas_local_install_and_test_with_virtualbox.md).

- dev

An example for non-vm env.

## 2 Installation of MAAS Region/Rack Controller

This is done via a manual install, everything default, with Ubuntu Server ISO latest version Ubuntu 18.04.1 LTS.

## 3 Run Playbook

### 3.1 Local ENV

If you want to create a maas in a VM locally to have a test:

```
vagrant up
ansible-playbook -i inventories/local playbooks/local.yml
```

This will launch a VM with Ubuntu 18.04 and install maas, and create a default admin user.

**Note:** since this role creates an MAAS admin, and without the admin user it's impossible to check existing users from CLI, so:

- this playbook can't be run twice
- if it fails somewhere after admin user creation, you will have to delete the admin via web UI then re-run it

To cleanup local VMs: `vagrant destroy -f`

### 3.2 Non-VM ENV Main playbook

Because configuring maas requires login, maas-cli, etc, this has to be done on the maas server.

```
# MAAS server admin user and IP, can only reached within office network
ssh tiexin@192.168.200.9
# clone this repo
ansible-playbook -i inventories/dev main.yml
```

This playbook also is compatible with local VM maas created by the previous step.

**Note:** this is not Not tested because the maas server does not have access to all the nodes, some are created by others.

## 4 Others

### 4.1 MAAS CLI

Examples see [HERE](maas_cli.md). 

### 4.2 MAAS HA

Discussion see [HERE](maas_ha.md).

