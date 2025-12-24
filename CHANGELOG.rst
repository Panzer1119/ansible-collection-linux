=========================================
Panzer1119 Linux Collection Release Notes
=========================================

.. contents:: Topics

v0.3.0
======

Minor Changes
-------------

- zfs_yubikey_autounlock - Add role to automatically unlock encrypted ZFS pools at boot using a pre-programmed YubiKey (HMAC-SHA1 challenge-response), with optional Pushover notifications.

v0.2.1
======

Bugfixes
--------

- set_up_docker_registry_mirror_vm - Fix setting of Docker container labels.

v0.2.0
======

Major Changes
-------------

- set_up_docker_registry_mirror_vm - Remove orphaned containers.

Minor Changes
-------------

- set_up_docker_registry_mirror_vm - Add variable to disable usage of Let's Encrypt.

v0.1.1
======

Bugfixes
--------

- set_up_docker_registry_mirror_vm - Fixed an issue creating the data directory.

v0.1.0
======

Release Summary
---------------

Create the role set_up_docker_registry_mirror_vm.

New Roles
---------

- panzer1119.linux.set_up_docker_registry_mirror_vm - Sets up a Docker registry mirror VM on Proxmox VE.
