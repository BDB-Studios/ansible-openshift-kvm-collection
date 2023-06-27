# Ansible Collection - bdbstudios.openshift_kvm

Currently only tested on EL 8/9 and Fedora 36+

Roles to ensure the host environment can run KVM.

Will provision a virtual helper node (bastion)
Will provision workers,controlplanes and bootstrap node
Can snapshot workers and control planes
Can destroy a kvm cluster that is running and remove all snapshots
