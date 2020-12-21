# OpenVPN Ansible Playbook

[![CircleCI](https://circleci.com/gh/verygood-ops/verygood.openvpn.svg?style=svg)](https://circleci.com/gh/verygood-ops/verygood.openvpn)


## Authentication

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

Please check https://verygoodsecurity.atlassian.net/wiki/spaces/FOUN/pages/35489057/How+to+Generate+VPN+Credentials

```
ansible-playbook -i ${HOST}, plays/vpn.yml -s -v -u ${USER} \
     -e 'clients=["${USER}"]' \
     -e 'infra_env=prod' \
     -e 'aws_account=vg' \
     -e 'aws_region=us-east-1' \
     -e 'client_config_dest=/tmp/client_certs/ansible'\
     -e "openvpn_2fa_google='yes'"
     -e "cname=${VPN_CNAME}"
```

## Vars

#TODO: fill in
