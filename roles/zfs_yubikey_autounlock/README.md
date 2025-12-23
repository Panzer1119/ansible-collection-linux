# zfs_yubikey_autounlock

Ansible role to configure automatic unlocking of encrypted ZFS pools using a **pre-programmed YubiKey**.  

**Important:** This role **does not** program YubiKeys, create ZFS pools, or re-key existing pools. It only prepares the host, generates challenge files (if missing), installs the unlock script, and integrates with systemd.  

Optional Pushover notifications can alert on failures and/or successful unlocks.

—

## Requirements

- Server (Debian/Ubuntu based)
- Pre-programmed YubiKey (slot configured for HMAC-SHA1 challenge-response)
- Root privileges on the host
- Optional: Pushover account + API token for notifications

—

## Role Variables

| Variable | Default | Description |
| :--- | :--- | :--- |
| `zfs_yubikey_pools` | `[]` | List of ZFS pool names to unlock. **Required.** |
| `zfs_yubikey_slot` | `2` | YubiKey slot number used for challenge-response (1 or 2). |
| `zfs_yubikey_challenge_dir` | `/etc/zfs/yubikey` | Directory to store challenge files. |
| `zfs_yubikey_pushover_user` | `""` | Pushover user key. If empty, no notifications are sent. |
| `zfs_yubikey_pushover_token` | `""` | Pushover API token. If empty, no notifications are sent. |
| `zfs_yubikey_notify_success` | `true` | Whether to send Pushover notifications on successful unlocks (priority 0). |
| `zfs_yubikey_systemd_before_extra` | `""` | Additional systemd units to append to the `Before=` directive. `zfs-mount.service` is always included. |

—

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: zfs_yubikey_autounlock
      vars:
        zfs_yubikey_pools:
          - tank
          - backup
        zfs_yubikey_slot: 2
        zfs_yubikey_pushover_user: "uXXXXXXXXXXXX"
        zfs_yubikey_pushover_token: "aXXXXXXXXXXXX"
        zfs_yubikey_notify_success: true
        zfs_yubikey_systemd_before_extra: "pve-storage.service"
```
