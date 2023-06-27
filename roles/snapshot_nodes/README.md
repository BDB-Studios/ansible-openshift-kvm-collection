bdbstudios.openshift_kvm.snapshot_nodes
=========

Role to create a snapshot of specified nodes before configuration is applied.

Requirements
------------

Requires the `community.libvirt` collection to be installed

Role Variables
--------------

```yaml
---

snapshot_workers: false # Boolean to toggle snapshotting of worker nodes
snapshot_controlplanes: false # Boolean to toggle snapshotting of control plane nodes

create_snapshots: true# #  Boolean to toggle snapshotting

...

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Snapshot KVM Nodes
  hosts: kvm_host
  become: true
  gather_facts: true

  roles:
    - role: bdbstudios.openshift_kvm.snapshot_nodes
      when: take_snapshot | default(true) | bool
      vars:
        snapshot_workers: true
        snapshot_controlplanes: true
      tags:
        - always
```
License
-------

MIT
