=========================================
Panzer1119 Linux Collection Release Notes
=========================================

.. contents:: Topics

v0.3.2
======

Minor Changes
-------------

- zfs_yubikey_autounlock role: change ``zfs_yubikey_systemd_before_extra`` default to a list (still rendered space-separated in the systemd unit).
- zfs_yubikey_autounlock role: keep pools as a list of pool name strings (``zfs_yubikey_pools``).
- zfs_yubikey_autounlock role: move Pushover notification settings to dedicated variables (``zfs_yubikey_pushover_user``, ``zfs_yubikey_pushover_token``, ``zfs_yubikey_pushover_title``).
- zfs_yubikey_autounlock role: refactor configuration to use separate role variables again (``zfs_yubikey_pools``, ``zfs_yubikey_slot``, ``zfs_yubikey_challenge_dir``, ``zfs_yubikey_systemd_before_extra``, ``zfs_yubikey_notify_success``).

v0.3.1
======

Minor Changes
-------------

- zfs_yubikey_autounlock role: move Pushover notification settings to a dedicated top-level dict (``zfs_yubikey_autounlock_pushover``).
- zfs_yubikey_autounlock role: refactor configuration variables to a single config dict (``zfs_yubikey_autounlock``) with ``pools`` as a list of pool name strings, and remove legacy variable support.

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
