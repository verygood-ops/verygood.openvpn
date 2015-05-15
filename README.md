# OpenVPN

```
ansible-playbook -i inventories/lendup-pci vpn.yml -s -vvvv -u mahmoudimus -e virtual_private_cidr="10.4.0.0/16" -e 'clients=["mahmoudimus"]'
```