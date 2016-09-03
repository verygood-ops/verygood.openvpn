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

### clients
* clients: array of client names (array of strings)
* client_config_dest: name of the configuration file to write to the client

### google_2fa
* openvpn_2fa_google: enable google 2fa (string 'yes' | 'no)

## Generating Client Keys

copy the configuration from the host at /etc/openvpn/vault-admins-ip-10-8-2-146.ovpn to your client. Copy the keys vault-admins.key, ca.crt, vault-admins.crt, ta.key to the client. Load configuration into tunnelblick by naming a folder containing all these files with the extension .tblk
