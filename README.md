# README: OpenVPN Installation and Configuration Playbook

This Ansible playbook automates the process of installing and configuring OpenVPN with Easy-RSA. It handles certificate generation, file placement, and server configuration. Below is a detailed guide on its purpose, prerequisites, and how to use it.

---

## Features
- Installs OpenVPN and Easy-RSA packages.
- Initializes and manages the Public Key Infrastructure (PKI).
- Creates necessary certificates (CA, server, Diffie-Hellman parameters, and TLS key).
- Configures OpenVPN server and generates client configuration files.
- Ensures idempotency by checking the existence of files and directories before executing tasks.

---

## Prerequisites

### 1. Target System Requirements
- Supported OS: Ubuntu/Debian-based systems.
- Sudo privileges for the Ansible user.

### 2. Ansible Control Node Requirements
- Ansible version 2.9+ installed on the control node.

### 3. Variables to Configure
Set the following variables in your playbook or inventory file:
- **`openvpn_packages`**: List of packages to install (e.g., `['openvpn', 'easy-rsa']`).
- **`easy_rsa_path`**: Path where the Easy-RSA directory should be created.
- **`easy_rsa_pki_path`**: Path to the PKI directory.
- **`openvpn_config_path`**: Path to the OpenVPN configuration directory (e.g., `/etc/openvpn`).

---

## Playbook Structure
This playbook consists of the following tasks:

1. **Update apt cache**: Ensures the package index is updated.
2. **Install OpenVPN and Easy-RSA**: Installs necessary software.
3. **Manage PKI and Certificates**:
   - Creates the Easy-RSA directory.
   - Initializes the PKI directory.
   - Generates CA, server certificates, and Diffie-Hellman parameters.
   - Generates a TLS key for added security.
4. **Copy Certificate Files**: Moves necessary files to the OpenVPN directory.
5. **Apply OpenVPN Configuration**: Deploys the server configuration using a Jinja2 template.
6. **Manage OpenVPN Process**: Ensures OpenVPN is running.
7. **Generate Client Configuration**: Creates a client `.ovpn` configuration file.

---

## Usage

### 1. Clone or Copy the Playbook
Clone this repository or copy the playbook file to your Ansible control node.

### 2. Customize Variables
Edit the variables to match your environment. You can set them in:
- The inventory file.
- A separate variable file (e.g., `vars.yml`).

Example inventory configuration:
```ini
[openvpn_servers]
your.server.ip ansible_user=your_user ansible_ssh_private_key_file=~/.ssh/id_rsa

[openvpn_servers:vars]
openvpn_packages=['openvpn', 'easy-rsa']
easy_rsa_path=/etc/easy-rsa
easy_rsa_pki_path=/etc/easy-rsa/pki
openvpn_config_path=/etc/openvpn
```

### 3. Create Templates
Ensure the following Jinja2 templates are present:
- **`server.conf.j2`**: OpenVPN server configuration template.
- **`client.ovpn.j2`**: Client configuration template.

### 4. Run the Playbook
Run the playbook using the following command:
```bash
ansible-playbook -i inventory_file playbook.yml
```

---

## Notes
- **Server Configuration**: Customize the `server.conf.j2` template to match your OpenVPN server requirements.
- **Client Configuration**: Adjust the `client.ovpn.j2` template to include the correct server address and required certificates.

---

## Troubleshooting
- If OpenVPN fails to start, check the server logs at `/var/log/syslog` or `/var/log/openvpn.log`.
- Ensure all required dependencies are installed on the target system.

---

