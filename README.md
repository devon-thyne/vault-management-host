vault-management-host
=====================

This role configures a host machine to trust Vault as certificate authority which enables clients to connect to this host via ssh with a Vault signed ssh key pairs

Requirements
------------

* Ansible 2.7+
* An initialized Hashicorp Vault server
* Supported Operating Systems:
    * RHEL 6.10 & 7.6
    * Ubuntu 14 & 16
    * Mac OS

Role Variables
--------------

| variable name                  | default value | description                                                                                         | value options                                                                                                                                                                                                                                                       |
| ------------------------------ | ------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| vault_ssh_host_vault_url       |               | (**mandatory**) URL to the Vault wished to configure the host to trust as a certificate authority   | examples: <br/> `https://vault.internal:8200` or `https://REDACTED` ...                                                                                                                                                                                          |
| vault_ssh_host_ssh_engine      |               | (**mandatory**) name of the specific ssh secrets engine to trust to sign ssh key pairs within Vault | examples: <br/> `ssh_development` or `ssh_management` ...                                                                                                                                                                                               |
| vault_ssh_host_config_behavior | configure     | behavior of the configure task                                                                      | `configure` - apply host configuration <br/><br/> `remove` host confiurations will be removed from machine                                                                                                                                                          |
| vault_ssh_host_public_key      | present       | ssh secrets engine public key state in list of trusted CA keys                                      | `present` - engine public key will be added to host's ssh configuration as a trusted key to sign client ssh key pairs with <br/><br/> `absent` - engine public key will be removed from host's ssh configuration as a trusted key to sign client ssh key pairs with |


Example Playbook
----------------

The standard location for our playbook to simply include this `vault-management-host` role is located in `playbooks/vault/vault-configure-host.yml`
```    
- hosts: localhost
  roles:
    - vault-management-host
```

Example Playbook Execution
--------------------------

Assuming the executed playbook includes the `vault-management-host` as depicted in the **Example Playbook** section above. The below examples show the use of the playbook that comes out of the box installed in the default `/etc/ansible/` directory, but this value can be substituted for the path to any other playbook that includes this role.

1. Running playbook with all default values
```
ansible-playbook -i localhost, -c local /etc/ansible/playbooks/vault/vault-configure-host.yml -e "vault_ssh_host_vault_url=https://URL_TO_VAULT:8200 vault_ssh_host_ssh_engine=SSH_ENGINE_NAME"
```

2. Running the playbook with any number of specified non-mandatory variables in addition to mandatory ones. For documentation on the passable variables to this role and their possible values please see the **Role Variables** section above.
```
ansible-playbook -i localhost, -c local /etc/ansible/playbooks/vault/vault-configure-host.yml -e "vault_ssh_host_vault_url=https://URL_TO_VAULT:8200 vault_ssh_host_ssh_engine=SSH_ENGINE_NAME VARIABLE1=VALUE VARIABLE2=VALUE"
```

Author Information
------------------

* Devon Thyne (devon-thyne)
