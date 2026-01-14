# panzer1119.linux.zbm_mint_provision Role

Provision a fresh Linux Mint installation on ZFS using **ZFSBootMenu** (UEFI).

This role is intended to be run from a **live environment** (e.g. Linux Mint live ISO) and will:

- Wipe and partition disks (EFI + ZFS)
- Create a ZFS pool and datasets
- Bootstrap a base Ubuntu system into `/mnt` via `debootstrap`
- Install Linux Mint packages (Cinnamon) into the chroot
- Install ZFSBootMenu into the EFI System Partition and create an EFI boot entry
- Create **recursive ZFS snapshots** of `{{ pool_name }}/ROOT` at major milestones

## Requirements / Assumptions

- **UEFI boot** is required (`/sys/firmware/efi/efivars` must exist).
- Run as `root` (disk wipe, mounting, chroot, ZFS operations).
- The live system must have working networking/DNS.
- Target disks specified by `boot_disk` / `pool_disk` must exist and be correct.
- The role installs tooling on the live system via APT: `debootstrap`, `gdisk`, `zfsutils-linux`, `curl`.

> Warning: This role is destructive. It wipes the given disks.

## Role Variables

All variables live in `defaults/main.yml`.

### Disk configuration

- `boot_disk` (default: `/dev/sda`)
  - Disk that contains the EFI System Partition.
- `boot_part_num` (default: `"1"`)
  - Partition number for the EFI System Partition.
- `pool_disk` (default: `/dev/sda`)
  - Disk that contains the ZFS partition.
- `pool_part_num` (default: `"2"`)
  - Partition number for the ZFS partition.
- `is_nvme` (default: `false`)
  - Controls how partition device paths are rendered.
  - `false`: `/dev/sda` + `1` → `/dev/sda1`
  - `true`:  `/dev/nvme0n1` + `1` → `/dev/nvme0n1p1`

### APT proxy (optional)

- `apt_proxy` (default: empty)
  - When set, writes `/mnt/etc/apt/apt.conf.d/90proxy` with `Acquire::http::Proxy` and `Acquire::https::Proxy`.
  - Example: `http://aptproxy.local:3142`

### ZFS configuration

- `pool_name` (default: `zroot`)
  - Name of the ZFS pool.
- `zfs_id` (default: `linuxmint`)
  - Backwards compatible name used by snapshots and (by default) as boot environment name.
- `zfs_boot_environment` (default: `{{ zfs_id }}`)
  - Boot environment dataset name under `{{ pool_name }}/ROOT/<name>`.

### ZFS dataset layout (Ubuntu-installer-inspired)

The role creates a root dataset plus optional sub-datasets to keep snapshots/rollbacks cleaner (similar to Ubuntu’s ZFS installer).

Created by default (in addition to the root datasets):

- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/srv`  → `/srv`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/usr` (container, `canmount=off`)
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/usr/local` → `/usr/local`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var` (container, `canmount=off`)
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/log` → `/var/log`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/cache` → `/var/cache`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/tmp` → `/var/tmp`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/lib` → `/var/lib`
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/lib/<sub>` (optional list)
- `{{ pool_name }}/ROOT/{{ zfs_boot_environment }}/var/<sub>` (optional list)

All of the above are controlled via `zfs_dataset_layout` in `defaults/main.yml`.

Home dataset options:

- `zfs_legacy_home_dataset` (default: `true`): create `{{ pool_name }}/home` mounted at `/home`.
- `zfs_use_userdata_container` (default: `false`): create `{{ pool_name }}/USERDATA` and place `/root` and `/home` datasets there.
- `zfs_per_user_home_datasets` (default: `false`): create one dataset per user from `zfs_users` when USERDATA is enabled.

Recommended things **not** to put on ZFS datasets:

- `/tmp`, `/run` (usually `tmpfs`)

### System configuration

- `hostname` (default: `linux-mint-zbm-test-vm`)
- `root_password` (default: `password`)
  - Set a proper (preferably hashed) password for real use.
- `timezone` (default: `Europe/Berlin`)
  - Currently not applied by tasks (documented for completeness).

### OS versions

- `ubuntu_codename` (default: `noble`)
  - Used for debootstrap.
- `mint_codename` (default: `zara`)
  - Used to render the chroot APT sources in `/mnt/etc/apt/sources.list`.
- `mint_keyring_url`
  - URL to the Linux Mint keyring `.deb` installed inside the chroot.

## Snapshots

The role creates **recursive** snapshots of `{{ pool_name }}/ROOT` at major milestones:

- `{{ pool_name }}/ROOT@post-debootstrap`
- `{{ pool_name }}/ROOT@pre-mint-install`
- `{{ pool_name }}/ROOT@post-mint-install`
- `{{ pool_name }}/ROOT@pre-export`

These snapshots are useful as rollback points while iterating on provisioning.

## Example Playbook

```yaml
- name: Provision Linux Mint on ZFS with ZFSBootMenu
  hosts: live_iso
  become: true
  gather_facts: false
  roles:
    - role: panzer1119.linux.zbm_mint_provision
      vars:
        boot_disk: /dev/nvme0n1
        pool_disk: /dev/nvme0n1
        is_nvme: true
        hostname: mint-zbm
        root_password: "changeme"
        apt_proxy: "http://aptproxy.local:3142"
        pool_name: zroot
        zfs_id: mint
```

## Notes

- The role currently renders `/mnt/etc/apt/sources.list` from a template (`templates/sources.list.j2`).
  If you prefer copying the live ISO APT sources instead, adjust the tasks accordingly.
- `apt_proxy` is applied inside the chroot by writing `/mnt/etc/apt/apt.conf.d/90proxy`.

## Idempotency / Atomicity

- **Idempotency:** **False**. The role wipes disks, creates partitions/pools, mounts filesystems, and appends to files (e.g. `/mnt/etc/fstab`). Re-running without cleanup will likely fail or produce duplicates.
- **Atomicity:** **False**. This is a procedural provisioning flow.

### Roll-back capabilities

Rollback is supported via the created ZFS snapshots (see “Snapshots” section). To roll back, boot a live system, import the pool, and `zfs rollback -r` to a previous snapshot.

## License

Apache-2.0

## Author Information

Paul.H / panzer1119
