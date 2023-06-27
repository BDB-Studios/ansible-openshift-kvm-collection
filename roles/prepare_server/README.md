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
# Our basic network configurations for the kvm network
networking_defaults:
  network: 192.168.0.0
  gateway: 192.168.0.254

# Our cluster name, internal dns domain and DNS forwarders, ideally we should use the host/kvm helper as forwarder one as we'll
# be running bind/named on it
dns:
  cluster_id: test
  forwarder1: 8.8.8.8
  forwarder2: 1.1.1.1
  domain: openshift.local

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

# This is to try and create a public bridge, it is buggy
configure_networking: false

...

```

Exported Facts
--------------

```yaml
    prepare_server_vm_subnet_ipv4: "{{ networking_defaults.network.split('.')[:3] | join('.') }}"
    prepare_server_ipv4_listen_private:
      - "{{ networking_defaults.network.split('.')[:3] | join('.') }}.1"
    prepare_server_ipv4_listen_public:
      - "{{ listen_address }}"
    prepare_server_virt_net_facts: {} # A dictionary of facts returned by community.libvirt.virt_net facts

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
