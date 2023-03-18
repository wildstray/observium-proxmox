![ProxmoxVE_PHP_API](https://upload.wikimedia.org/wikipedia/en/thumb/2/25/Proxmox-VE-logo.svg/600px-Proxmox-VE-logo.svg.png)
# Observium CE support for Proxmox VE

This is an Observium CE changeset to add functionalities for Proxmox nodes VMs detection and to display additional storage (eg. CEPH, LVM, LVM thin, etc.).

## Table of Contents
- [Observium CE support for Proxmox VE](#observium-ce-support-for-proxmox-ve)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Caveat](#caveat)
  - [Credits](#credits)

## Installation
Apply the observium-proxmox.diff to add support for Proxmox VE VM discovery to Observium CE NMS. Add ProxmoxVE_PHP_API library to Observium CE with PHP Composer.

```
cd /opt/observium
patch -p2 -N < observium-proxmox.diff
mkdir -p /opt/observium/lib/Proxmox
php composer.phar -n require saleh7/proxmox-ve_php_api

```
## Usage

You have to add these configuration parameters to config.php:

```
$config['enable_proxmox']      = 1;  # Enable Proxmox VM support
$config['proxmox_username']    = 'observium';
$config['proxmox_password']    = 'mysecret';
$config['proxmox_realm']       = 'pve';
$config['proxmox_token_name']  = 'mytoken';
$config['proxmox_token_value'] = '27801b8a-7227-4e2e-900d-1a62e01ccfac';
```
The hostname for API login will be the hostname of the Observium configured device.

Parameter **proxmox_password** is optional if you intend to use API tokens, otherwise **proxmox_token_name** and **proxmox_token_value** are optional if you intend to user username and password. We recommens to use tokens, so you can avoid to flood syslog of Proxmox VE nodes of authentications. 

The username must be able to authenticate to Proxmox and it must have the minimal role of **PVEAuditor**.

If you use tokens, turn off Priviled Separation.

## Caveat

For now username, password or tokens and realms will be unique for all devices (for simplicity, because is not provided a web UI configuration for Proxmox API login for device, obviously any contribution is welcome!). This could be useful and acceptable for a standalone Proxmox nodes or a single cluster with multiple nodes; it could not work as desired for multiple clusters with different realms, usernames, tokens. 

## Credits

Andrea Tuccia "wildstray"

Thanks to https://github.com/Saleh7/ProxmoxVE_PHP_API
