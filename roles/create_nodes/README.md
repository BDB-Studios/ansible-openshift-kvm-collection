bdbstudios.openshift_kvm.create_nodes
=========

Opinionated role to create kvm vms to use as openshift machines. Will configure optional extra drives if passed through.
Supports network bonding as well, so we can test out our dual nic scenario. The primary drive is more
optimised for IOPs than the extra drives for a more performant VM.

Requirements
------------

Requires the `community.libvirt` collection to be installed

Role Variables
--------------

Global variables
```yaml
qcow_cluster_size: "2M" # Default size for our qcow2 cluster size on our disks
iothread_type: "threads" # iothread type for our primary disk
```

Per instance configuration vars, hosted in the following lists:
 - bootstrap
 - control_plane
 - workers

```yaml
vm_instance_name: "string" # Composed as follows "{{ instance.name }}.{{ dns.clusterid }}.{{ dns.domain }}"
vm_network: "string" # "{{ dns.clusterid }}"
vm_mac_address: "string" # Generated with the command `date +%s | md5sum | head -c 6 | sed -e 's/\([0-9A-Fa-f]\{2\}\)/\1:/g' -e 's/\(.*\):$/\1/' | sed -e 's/^/52:54:00:/'`
vm_vcpu: "int" # Defaults to 4 "{{ instance.vm_cpu | default(4) }}"
vm_memory_size: "int"# MiB defaults to 16384"{{ instance.vm_memory | default(16384) }}"
vm_memory_unit: "string" # Defaults to MiB"{{ instance.vm_memory_unit | default('MiB') }}"
vm_root_disk_size: "string" # Defaults to 120G"{{ instance.vm_disk_size | default('120G') }}"
use_bonded_network: "bool" # Defaults to {{ instance.bond is defined }}"
vm_extra_disks: [] # List of dictionaries, described below, default empty
```

Optional per_instance vars as a dictionary in each node

```yaml
vm_extra_disks:
  - device: "string" # Device identifier ie vdb
    size: "string" # Disk size identifier as a string ie 20G
```


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
    - name: Create our VMS
      hosts: kvm_host
      become: true

      roles:
        - role: bdbstudios.openshift_kvm.create_nodes
          tags:
            - kvm-instances
```

License
-------

MIT
