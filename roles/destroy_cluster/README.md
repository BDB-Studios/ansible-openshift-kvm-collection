### Ansible Role - KVM/Destroy Cluster


bdbstudios.openshift_kvm.destroy_cluster
=========

Role to destroy vms and associated snapshots/drives. Will also remove the network portions if instructed.


Requirements
------------

Requires the `community.libvirt` collection to be installed

Role Variables
--------------

Global config
```yaml
# KVM Instances
remove_bootstrap: "boolean" # requires bootstrap list of dicts
remove_controlplanes: "boolean" # requires control_planes list of dicts
remove_workers: "boolean" # requires workers list of dicts

# KVM Networking
remove_network: "boolean"

# Openshift Configuration
remove_cluster_config: "boolean"
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Destroy KVM Openshift Cluster
  hosts: kvm_host
  become: true

  roles:
    - role: bdbstudios.openshift_kvm.destroy_cluster
      vars:
        remove_bootstrap: true
        remove_controlplanes: true
        remove_workers: true
        remove_network: true
        remove_cluster_config: true
  tags:
    - kvm
```

License
-------

MIT
