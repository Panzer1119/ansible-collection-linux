# zfs_yubikey_autounlock

Ansible role to configure **automatic unlocking of encrypted ZFS pools at boot** using a **pre-programmed YubiKey** (HMAC-SHA1 challenge-response).

## Scope

This role **does not**:

- program YubiKeys,
- create or modify ZFS pools,
- change ZFS key formats or re-key existing pools.

It **does**:

- install required packages,
- create per-pool challenge files (only if missing),
- install an unlock script,
- install and enable a `systemd` oneshot service.

Optional Pushover notifications can alert on failures and (optionally) on successful unlocks.

## Requirements

- Debian/Ubuntu (or derivatives) with `apt` and `systemd`
- ZFS installed and pools already exist
- Pools are configured so `zfs load-key` can unlock them (e.g., passphrase/keylocation as appropriate)
- YubiKey is pre-configured in slot **1** or **2** for HMAC-SHA1 challenge-response
- Root privileges on the host
- Optional: Pushover account + API token

## Security notes

- Challenge files in `{{ zfs_yubikey_challenge_dir }}` are **sensitive**. The role creates them with restrictive permissions.
- Do **not** store Pushover credentials in plaintext inventories. Prefer Ansible Vault or a secrets backend.
- The unlock script logs to journald/syslog via `logger` and does not print the derived passphrase.

## Role variables

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `zfs_yubikey_pools` | `list[str]` | `[]` | Pools to unlock, e.g. `[tank, backup]`. **Required.** |
| `zfs_yubikey_slot` | `int` | `2` | YubiKey slot used for challenge-response (1 or 2). |
| `zfs_yubikey_challenge_dir` | `str` | `/etc/zfs/yubikey` | Directory to store challenge files (`<pool>.challenge`). |
| `zfs_yubikey_systemd_before_extra` | `list[str]` | `[]` | Additional units appended to `Before=`. `zfs-mount.service` is always included. |
| `zfs_yubikey_notify_success` | `bool` | `true` | Send Pushover notifications on successful unlocks (priority 0). |
| `zfs_yubikey_pushover_user` | `str` | `""` | Pushover user key. Empty = no notifications. |
| `zfs_yubikey_pushover_token` | `str` | `""` | Pushover API token. Empty = no notifications. |
| `zfs_yubikey_pushover_title` | `str` | `"ZFS YubiKey Auto-Unlock"` | Pushover notification title. |

## Example playbook

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
        zfs_yubikey_challenge_dir: /etc/zfs/yubikey
        zfs_yubikey_systemd_before_extra:
          - pve-storage.service
        zfs_yubikey_notify_success: true
        zfs_yubikey_pushover_user: "uXXXXXXXXXXXX"
        zfs_yubikey_pushover_token: "aXXXXXXXXXXXX"
        zfs_yubikey_pushover_title: "ZFS YubiKey Auto-Unlock"
```

## systemd integration

The role installs and enables:

- `zfs-yubikey-unlock.service` (Type=oneshot)

Logs:

- `journalctl -u zfs-yubikey-unlock.service -b`

## Limitations

- Currently targets `apt`-based systems.
- Only pools listed in `zfs_yubikey_pools` are processed.
- If the YubiKey is missing or a challenge file is missing, the affected pool(s) will not be unlocked.

## Troubleshooting

- Check service status:
  - `systemctl status zfs-yubikey-unlock.service`
  - `journalctl -u zfs-yubikey-unlock.service -b`
- Verify YubiKey detection:
  - `ykman info`
- Verify challenge files:
  - `ls -l {{ zfs_yubikey_challenge_dir }}`
  - files are named `<pool>.challenge`
