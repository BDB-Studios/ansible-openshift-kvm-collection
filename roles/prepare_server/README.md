bdbstudios.openshift_kvm.prepare_server
=========

Installs and configures pre-reqs to run KVM on a EL machine. This includes a graphical environment,
virtualization software, XRDP for remote desktop access and configures the initial KVM network.

Requirements
------------

Requires the `community.libvirt` collection to be installed

Role Variables
--------------

```yaml
---

virtualization_packages: # Packages required to run the KVM server, we have Rocky and Debian declared here as they have been used for testing
  fedora:
    - "@virtualization"
    - python3-lxml
    - python3-pip
    - python3-requests-oauthlib
    - python3-openshift
    - cronie
    - bridge-utils
  redhat:
    - "@virtualization-hypervisor"
    - "@virtualization-client"
    - "@virtualization-platform"
    - "@virtualization-tools"
    - python3-lxml
    - python3-pip
    - python3-requests-oauthlib
    - python3-openshift
    - cronie
    - bridge-utils
  rocky:
    - "@virtualization-hypervisor"
    - "@virtualization-client"
    - "@virtualization-platform"
    - "@virtualization-tools"
    - python3-lxml
    - python3-pip
    - python3-requests-oauthlib
    - python3-openshift
    - cronie
    - bridge-utils

listen_address: "{{ hostvars['localhost']['ansible_default_ipv4']['address'] | default('') }}"

configure_networking: false
is_hq_kvm: false

...

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Install KVM server
  hosts: kvm_host
  become: true
  roles:
    - role: bdbstudios.openshift_kvm.prepare_server
      tags:
        - kvm
        - network

```
License
-------

MIT
