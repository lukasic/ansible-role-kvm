Ansible KVM Hypervisor (libvirt) Role
=====================================

Simple ansible role for managing virtualization hypervisor based on libvirt (kvm/qemu).

Contains scripts for creating, deleting and live migrating virtuals.

Do not use in production. Role is under development.

Installation
------------

Using `ansible-galaxy`:

```bash
$ ansible-galaxy install lukasic.kvm
```

Using `requirements.yml`:

```yaml
- src: lukasic.kvm
```

Requirements
------------

Debian Buster.

Role Variables
--------------

LVM settings - existing volume group name with free space, and logical volume name to be created for storing virtual machines data.

```yaml
lvm_vg_name: "vg0"
lvm_data_name: "fast"
```

Role supports managing `/etc/network/interfaces`. If template file `interfaces.j2` is fine for you, you can change this to true:

```yaml
kvm_manage_interfaces: false
```

and configure `/etc/network/interfaces` variables with:

```yaml
primary_ipv4: null
primary_gw: null
primary_iface: "ens3"
primary_bridge_name: "br0"
```

Virtual machines created with script `create-virtual.sh` uses cloud-init (nocloud) for initialization. Ansible takes `authorized_keys` from `users` variable as defined by Ansible role `weareinteractive.users`, see more at https://github.com/weareinteractive/ansible-users.

```yaml
users: []
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: kvm
  roles:
    - lukasic.kvm
  vars:
    lvm_data_name: "fast"
    lvm_vg_name: "vg0"
    kvm_manage_interfaces: true
    primary_ipv4: "10.252.0.8"
    primary_gw: "10.252.0.1"
    primary_iface: "ens3"
    primary_bridge_name: "br13"
    users:
      - username: awx
        authorized_keys:
          - "ssh-ed25519 AAAAC3N... awx"

```

License
-------

WTFPL

Author Information
------------------

The role was created in 2020 by [Lukáš Kasič](https://github.com/lukasic).
