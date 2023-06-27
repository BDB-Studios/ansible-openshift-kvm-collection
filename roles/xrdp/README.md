microlise_collections.kvm.xrdp
=========

Auxillary role called by `microlise_collections.kvm.prepare_server` can be consumed in other playbooks.
Installs TigerVNC and XRDP server on an EL host. Will ensure we have a default desktop environment as well and
if required install it. Configures the XRDP.ini file to allow windows RDP to connect.


Requirements
------------

We need to be running an Enterprise Linux version

Role Variables
--------------

```yaml
---

xrdp: # What packages we're installing
  packages:
    fedora:
      desktop:
        - "@kde-desktop-environment"
      xrdp:
        - xrdp
    redhat:
      desktop:
        - "@Server with GUI"
      xrdp:
        - "tigervnc-server"
        - xrdp
  config: # Config options for XRDP and Win10/11 RDP to play nicely
    - search: "^#delay_ms=2000$"
      replace: "delay_ms=2000"
    - search: "^#disabled_encodings_mask=0$"
      replace: "disabled_encodings_mask=0"
    - search: "^#xserverbpp=24$"
      replace: "xserverbpp=24"

...

```

Dependencies
------------

None

Example Playbook
----------------

```yaml

- name: Install XRPD server
  hosts: kvm
  become: true

  roles:
    - role: microlise_collections.kvm.xrdp
      when: using_xrdp | default(false) | bool
      tags:
        - xrdp

```
License
-------

MIT
