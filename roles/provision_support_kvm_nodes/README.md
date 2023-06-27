bdbstudios.openshift_kvm.provision_kvm_support_nodes
=========

Creates a support KVM node from the RHEL kvm image stored in s3

Requirements
------------

Requires the `community.libvirt` collection to be installed
Requires the `amazon.aws` collection to be installed

Role Variables
--------------

Defaults in the module
```yaml

nodes:
  image:
    bucket: "kvm-qemu-images"
  bastion:
    name: "{{ bastion_kvm_image | default('AlmaLinux-8-GenericCloud-latest.x86_64.qcow2') }}"
    url: "https://repo.almalinux.org/almalinux/8/cloud/x86_64/images/"
    checksum: "c0ad09255d91288dac590d99c95197d83a2846f1bcbec3f4222fb04265a2a4d7"
    os_variant: "rhel8-unknown"
    ram: "{{ bastion_kvm_ram | default(16384) }}"
    cpus: "{{ bastion_kvm_cpus | default(8) }}"

```

Required from outside

```yaml
###
kvm:
  bastion:
    name: "bastion"
    ipaddr: "192.168.160.10"
    vm_mac_address: "84:47:09:0e:f7:f0"
    vm_vcpu: 4
    vm_memory_size: "8192"
    vm_memory_unit: "MiB"
    vm_root_disk_size: "120G"
    vm_instance_name: "bastion.{{ dns.clusterid}}.{{ dns.domain  }}"
    vm_network: "{{ dns.clusterid }}"
    vm_extra_disks:
      - device: "sdb" # Device identifier ie vdb
        size: "240G"

image_from_s3: true
s3_access_key_id: "username"
s3_secret_access_key: "password"
s3_region: "setme"
s3_url: "http://localhost:9090"
s3_validate_certs: false

using_rhel: false
admin_password: "secret"
root_password: "super_secret"
admin_user: "admin"
admin_gecos: "Default admin user"

networking_defaults:
  gateway: "192.168.0.254"
  prefix: "24"
  ns1: "8.8.8.8"
  ns2: "1.1.1.1"

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Configure our KVM host in the HQ
  hosts: kvm_host
  become: true

  roles:
    - role: bdbstudios.openshift_kvm.prepare_server
      when: kvm.bastion is defined
      tags:
        - always
    - role: bdbstudios.openshift_kvm.provision_support_kvm_nodes
      when: kvm.bastion is defined
      tags:
        - always
```

License
-------

MIT
