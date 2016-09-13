# OpenVPN Ansible Playbook

[![CircleCI](https://circleci.com/gh/verygood-ops/verygood.openvpn.svg?style=svg)](https://circleci.com/gh/verygood-ops/verygood.openvpn)

## Development

```
export CITADEL_BUCKET=
export AWS_ACCOUNT=
export AWS_KEY=
vagrant up
vagrant provision
```

requires a default password for vagrant user. it should be `changeme` by default

## Production

- Uses Google 2FA

Two Factor auth is implemented via a combination of two PAM modules. 

1. openvpn
1. google-authenticator

The first module passes local UNIX user/password auth info via `/etc/passwd` to OpenVPN as a challenge for the client application. The second module appends a TOTP substring to the client password string. This effectively enables two factors in a single authentication string. The pattern for the password is

`username, unix-pass + TOTP`

This mode is disabled in dev, though if you would like to enable it for testing, set the variable `openvpn_2fa_google = 'yes'`

To generate a QR code for 2fa, this must be done manually for each user, since this access is infrequent and should be audited by a person prior to provisioning. The process to generate this code is simple. Log into the VPN server as root, create the new user, set the password, switch to the new user and generate the QR code.

```
useradd -m -s /bin/bash $username
passwd $username
su - username
google-authenticator
```

A QR code will print out to your shell. Take a screen shot and save it.

## Generating Client Keys

The playbook will run by default without generating any client configuration or keys. To generate keys, update the variable `clients: []` with the names of each client you wish to enable. Copy the keys $client.key, ca.crt, $client.crt, ta.key to the client system. Load configuration into tunnelblick by naming a folder containing all these files with the extension .tblk.

## vars

### service defaults
* aws_region: region for KMS and S3 services (string)
* openvpn_port: port (number)
* openvpn_proto: protocol (tcp| udp)
* openvpn_service_name: service name (string)
* openvpn_config_file: name of the configuration file to write to the server (string)
* openvpn_cname: name of the host endpoint (string)

### backup section
* openvpn_backup_keys: copy keys to a S3 bucket (bool)
* openvpn_backup_bucket: S3 bucket name, accessed via citadel (string)
* openvpn_backup_server_keys_path: path to the bucket location with server keys (string)
* openvpn_backup_clients_keys_path: path to the bucket location with the client keys (string)
* openvpn_backup_clients_configs_path: path to location on disk to store the client config (string)
* openvpn_force_gen_keys: generate new keys for the server and client (bool)

### aws vpc, lan, etc
* virtual_private_cidr: aws private LAN subnet (ipaddr)
* openvpn_iptables_enabled: add iptables rules to server (string "yes" | "no")
* openvpn_server_subnet_seed: ???
* openvpn_server_subnet_netmask: server subnet mask (ipaddr)
* block_size: block_size for cipher. must be higher than defaults to mitigate sweet32 attack. (string)

### clients
* clients: array of client names (array of strings)
* client_config_dest: name of the configuration file to write to the client

### google_2fa
* openvpn_2fa_google: enable google 2fa (string 'yes' | 'no')

