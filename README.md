# Install Cisco VIRL on packet.net
This project was borne out of a need to quickly spin up and tear down a Cisco VIRL server for study purposes.

The `deploy-virl-packet-server` playbook will:
- Provision a Cisco VIRL server on https://packet.net/
 - Install the latest version of Cisco VIRL

 The `teardown-virl-packet-server` playbook will:
 - Destroy the Cisco VIRL server on https://packet.net/


## NOTES
- This project will NOT install a licensed VIRL; you still have to register this instance with Cisco.
- You will also have to download the VMaestro client in order to run your topologies.

## Packet.net setup requirements
  Visit https://packet.net/:
  - Create an account and add a form of payment
  - Create a project called cisco_virl
   - Add a personal or project api key
	 - create an ssh key pair and upload the public key to Packet.net

##  On your ansible control machine
#### Install packet-python
```pip install packet-python```

#### Update ansible.cfg to point to your vault password file*
```
[defaults]
inventory      = inventory
host_key_checking = False
vault_password_file = </path/to/your/vault/password/file>
retry_files_enabled = False
[persistent_connection]
command_timeout = 100
```
_*or remove the 'vault_password_file' line if you prefer to supply the password some other way_

#### Update roles/virl-packet-net-provision/vars/vault.yml with your api key
```
---
# roles/packet-net-provision/vars/vault.yml
vaulted_api_token: <your_api_key>
```
#### Update roles/virl-packet-net-teardown/vars/vault.yml with your api key
```
---
# roles/packet-net-provision/vars/vault.yml
vaulted_api_token: <your_api_key>

#### Vault your api key
```ansible-vault encrypt roles/packet-net-provision/vars/vault.yml```

```

## Run it
#### Provision instance and install Cisco VIRL
```ansible-playbook deploy-virl-packet-server.yml```


#### Tear down Cisco VIRL instance
```ansible-playbook teardown-virl-packet-server.yml```
