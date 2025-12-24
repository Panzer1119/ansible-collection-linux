# Panzer1119 Linux Collection Release Notes

**Topics**

- <a href="#v0-3-0">v0\.3\.0</a>
    - <a href="#minor-changes">Minor Changes</a>
- <a href="#v0-2-1">v0\.2\.1</a>
    - <a href="#bugfixes">Bugfixes</a>
- <a href="#v0-2-0">v0\.2\.0</a>
    - <a href="#major-changes">Major Changes</a>
    - <a href="#minor-changes-1">Minor Changes</a>
- <a href="#v0-1-1">v0\.1\.1</a>
    - <a href="#bugfixes-1">Bugfixes</a>
- <a href="#v0-1-0">v0\.1\.0</a>
    - <a href="#release-summary">Release Summary</a>
    - <a href="#new-roles">New Roles</a>

<a id="v0-3-0"></a>
## v0\.3\.0

<a id="minor-changes"></a>
### Minor Changes

* zfs\_yubikey\_autounlock \- Add role to automatically unlock encrypted ZFS pools at boot using a pre\-programmed YubiKey \(HMAC\-SHA1 challenge\-response\)\, with optional Pushover notifications\.

<a id="v0-2-1"></a>
## v0\.2\.1

<a id="bugfixes"></a>
### Bugfixes

* set\_up\_docker\_registry\_mirror\_vm \- Fix setting of Docker container labels\.

<a id="v0-2-0"></a>
## v0\.2\.0

<a id="major-changes"></a>
### Major Changes

* set\_up\_docker\_registry\_mirror\_vm \- Remove orphaned containers\.

<a id="minor-changes-1"></a>
### Minor Changes

* set\_up\_docker\_registry\_mirror\_vm \- Add variable to disable usage of Let\'s Encrypt\.

<a id="v0-1-1"></a>
## v0\.1\.1

<a id="bugfixes-1"></a>
### Bugfixes

* set\_up\_docker\_registry\_mirror\_vm \- Fixed an issue creating the data directory\.

<a id="v0-1-0"></a>
## v0\.1\.0

<a id="release-summary"></a>
### Release Summary

Create the role set\_up\_docker\_registry\_mirror\_vm\.

<a id="new-roles"></a>
### New Roles

* panzer1119\.linux\.set\_up\_docker\_registry\_mirror\_vm \- Sets up a Docker registry mirror VM on Proxmox VE\.
