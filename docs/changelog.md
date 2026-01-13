# Panzer1119 Linux Collection Release Notes

<a id="v0-4-4"></a>
## v0\.4\.4

<a id="v0-4-3"></a>
## v0\.4\.3

<a id="minor-changes"></a>
### Minor Changes

* zbm\_mint\_provision \- Add APT Proxy support for LIVE ISO\.

<a id="v0-4-2"></a>
## v0\.4\.2

<a id="bugfixes"></a>
### Bugfixes

* zbm\_mint\_provision \- Fix fact setting\.

<a id="v0-4-1"></a>
## v0\.4\.1

<a id="minor-changes-1"></a>
### Minor Changes

* zbm\_mint\_provision \- Add README\.md file\.

<a id="v0-4-0"></a>
## v0\.4\.0

<a id="release-summary"></a>
### Release Summary

Create the role zbm\_mint\_provision\.

<a id="new-roles"></a>
### New Roles

* panzer1119\.linux\.zbm\_mint\_provision \- Sets up a Linux Mint system with ZBM\.

<a id="v0-3-10"></a>
## v0\.3\.10

<a id="minor-changes-2"></a>
### Minor Changes

* zfs\_yubikey\_autounlock \- Add optional <code>zfs\_yubikey\_serial</code> to pin ykman/ykchalresp to a specific YubiKey device when multiple are connected\.

<a id="v0-3-9"></a>
## v0\.3\.9

<a id="v0-3-8"></a>
## v0\.3\.8

<a id="v0-3-7"></a>
## v0\.3\.7

<a id="v0-3-6"></a>
## v0\.3\.6

<a id="v0-3-5"></a>
## v0\.3\.5

<a id="v0-3-4"></a>
## v0\.3\.4

<a id="v0-3-3"></a>
## v0\.3\.3

<a id="bugfixes-1"></a>
### Bugfixes

* zfs\_yubikey\_autounlock role\: add a systemd daemon\-reload handler to ensure newly installed/updated unit files are picked up by systemd\.

<a id="v0-3-2"></a>
## v0\.3\.2

<a id="minor-changes-3"></a>
### Minor Changes

* zfs\_yubikey\_autounlock role\: change <code>zfs\_yubikey\_systemd\_before\_extra</code> default to a list \(still rendered space\-separated in the systemd unit\)\.
* zfs\_yubikey\_autounlock role\: keep pools as a list of pool name strings \(<code>zfs\_yubikey\_pools</code>\)\.
* zfs\_yubikey\_autounlock role\: move Pushover notification settings to dedicated variables \(<code>zfs\_yubikey\_pushover\_user</code>\, <code>zfs\_yubikey\_pushover\_token</code>\, <code>zfs\_yubikey\_pushover\_title</code>\)\.
* zfs\_yubikey\_autounlock role\: refactor configuration to use separate role variables again \(<code>zfs\_yubikey\_pools</code>\, <code>zfs\_yubikey\_slot</code>\, <code>zfs\_yubikey\_challenge\_dir</code>\, <code>zfs\_yubikey\_systemd\_before\_extra</code>\, <code>zfs\_yubikey\_notify\_success</code>\)\.

<a id="v0-3-1"></a>
## v0\.3\.1

<a id="minor-changes-4"></a>
### Minor Changes

* zfs\_yubikey\_autounlock role\: move Pushover notification settings to a dedicated top\-level dict \(<code>zfs\_yubikey\_autounlock\_pushover</code>\)\.
* zfs\_yubikey\_autounlock role\: refactor configuration variables to a single config dict \(<code>zfs\_yubikey\_autounlock</code>\) with <code>pools</code> as a list of pool name strings\, and remove legacy variable support\.

<a id="v0-3-0"></a>
## v0\.3\.0

<a id="minor-changes-5"></a>
### Minor Changes

* zfs\_yubikey\_autounlock \- Add role to automatically unlock encrypted ZFS pools at boot using a pre\-programmed YubiKey \(HMAC\-SHA1 challenge\-response\)\, with optional Pushover notifications\.

<a id="v0-2-1"></a>
## v0\.2\.1

<a id="bugfixes-2"></a>
### Bugfixes

* set\_up\_docker\_registry\_mirror\_vm \- Fix setting of Docker container labels\.

<a id="v0-2-0"></a>
## v0\.2\.0

<a id="major-changes"></a>
### Major Changes

* set\_up\_docker\_registry\_mirror\_vm \- Remove orphaned containers\.

<a id="minor-changes-6"></a>
### Minor Changes

* set\_up\_docker\_registry\_mirror\_vm \- Add variable to disable usage of Let\'s Encrypt\.

<a id="v0-1-1"></a>
## v0\.1\.1

<a id="bugfixes-3"></a>
### Bugfixes

* set\_up\_docker\_registry\_mirror\_vm \- Fixed an issue creating the data directory\.

<a id="v0-1-0"></a>
## v0\.1\.0

<a id="release-summary-1"></a>
### Release Summary

Create the role set\_up\_docker\_registry\_mirror\_vm\.

<a id="new-roles-1"></a>
### New Roles

* panzer1119\.linux\.set\_up\_docker\_registry\_mirror\_vm \- Sets up a Docker registry mirror VM on Proxmox VE\.
