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


## Generating Client Keys

copy the configuration from the host at /etc/openvpn/vault-admins-ip-10-8-2-146.ovpn to your client. Copy the keys vault-admins.key, ca.crt, vault-admins.crt, ta.key to the client. Load configuration into tunnelblick by naming a folder containing all these files with the extension .tblk
