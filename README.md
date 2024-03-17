# Installs wireguard into a machine and generates peer config to connect

## Playbook_Install_Server
Initial Setup and WireGuard Installation

    Update apt package cache: Ensures the package lists on the target hosts are up to date. This is important for installing the latest version of packages.

    Install WireGuard and other tools: Installs the WireGuard package along with net-tools. net-tools is often used for networking tasks, such as checking network interfaces.

WireGuard Configuration and Key Management

    Stop WireGuard service: Ensures the WireGuard service is stopped before making changes. This is necessary to apply configuration changes or to ensure a clean setup process.

    Create /etc/wireguard directory: Ensures the directory for WireGuard configuration files exists. This is where WireGuard stores its configuration and key files.

    Generate WireGuard key pair: Generates a private key for the WireGuard server. This key is essential for securing VPN connections.

    Save private key to /etc/wireguard/privatekey: Saves the generated private key to a file. This key is used by the WireGuard service to authenticate itself to clients.

    Generate public key: Generates a public key from the private key. The public key is shared with clients to allow them to connect to the server.

    Save public key to /etc/wireguard/publickey: Saves the generated public key to a file. This key will be used in the client configuration.

Reading and Using Key Information

    Read private and public keys from file: Reads the previously saved private and public keys from their respective files. This is done to use these keys in configurations or to share with clients.

    Decode base64 readed file and store in a variable: Decodes the base64-encoded content of the key files and stores the result in variables. This makes the keys usable in the playbook.

Network Configuration and Service Management

    Get default interface: Determines the default network interface used for internet connectivity. This is important for configuring WireGuard to use the correct interface.

    Create WireGuard configuration file: Creates the main WireGuard configuration file (wg0.conf) based on a template. This file contains the server's settings, including its private key and network configurations.

    Enable and start WireGuard service: Ensures the WireGuard service is enabled to start on boot and starts the service. This activates the VPN server.

Enabling IP Forwarding

    Enable IPv4 forwarding in sysctl.conf: Modifies the system configuration to allow IP forwarding. This is crucial for the VPN server to route traffic between clients and the internet.

    Apply sysctl settings: Applies the changed system settings to enable IP forwarding immediately.

Client Configuration Generation

    Generate WireGuard key pair for the client: Similar to the server, generates a key pair for the client. This is for creating a secure connection from the client to the server.

    Generate public key for the client: Generates the public key from the client's private key. This public key is added to the server's configuration to recognize the client.

    Copy client public key to file & Fetch public key file: Saves the client's public key to a file and fetches it to the Ansible controller. This is useful for manually configuring other clients or for record-keeping.

    Get default DNS server: Retrieves the current DNS server setting. This can be used in the client configuration to ensure DNS queries are routed through the VPN.

    Create WireGuard configuration file for the client (peer.conf): Generates a client configuration file based on a template. This file includes the client's private key and the server's public key, among other settings.

    Add client to the server peers: Adds the client as a peer to the server's WireGuard configuration. This step is crucial for allowing the client to connect to the server.

    Fetch file (peer.conf): Fetches the generated peer.conf file to the Ansible controller. This file can then be distributed to the client for use in connecting to the VPN.

Reboot Handler

    Reboot: A handler to reboot the server if required. This ensures all changes, especially those requiring a reboot (like sysctl configurations), are applied.

## Requierements

- UDP port must be opened
- Some reboot must be done to the machine to activate the internet access

## Usage

```bash
ansible-playbook -i hosts.ini playbook.yaml
```

Were hosts is the inventory file.

An example of that inventory file can be:

```
[all:vars]
ansible_connection=ssh
ansible_user=username
# ansible_port=22

[wireguard]
 IP Address

After execution a new peer.conf file will be created in the current directory with the wireguard connection settings to the client.

# TODO

- [ ] Add more than one client
- [ ] Add parametrization and options
  [ ] molecule testing



