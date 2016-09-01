# OpenVPN Ansible Playbook

[![CircleCI](https://circleci.com/gh/verygood-ops/verygood.openvpn.svg?style=svg)](https://circleci.com/gh/verygood-ops/verygood.openvpn)

## Development

```
cd vagrant
vagrant up
cd ..
ssh-add vagrant/.vagrant/machines/default/virtualbox/private_key
ansible-playbook test.yml
```

## Production

- Uses Google 2FA


## Generating Client Keys

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
