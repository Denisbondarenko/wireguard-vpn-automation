# Installs wireguard into a machine and generates peer config to connect

## Requierements

- UDP port must be opened
- Some reboot must be done to the machine to activate the internet access

## Usage

```bash
ansible-playbook -i hosts playbook.yaml
```

Were hosts is the inventory file.

An example of that inventory file can be:

```
[all:vars]
ansible_connection=ssh
ansible_user=username
# ansible_port=22

[wireguard]
myserverhost.wireguard
```

After execution a new peer.conf file will be created in the current directory with the wireguard connection settings to the client.

# TODO

- [ ] Add more than one client
- [ ] Add parametrization and options


# Helpers

Use in conjunction with terraform instant-ec2-instance
