Role Name
=========

Install Hashicorp Vault and run as a Service

Requirements
------------

NA

Role Variables
--------------

Refer `default/main.yml`
```
vault_user: vault
vault_group: vault
vault_root: /apps/vault
vault_tarball_url: https://releases.hashicorp.com/vault/0.6.4/vault_0.6.4_linux_amd64.zip

vault_configuration:
  disable_mlock: false
  cluster_name: vault-dev
  listener: 
    - tcp:
        address: "localhost:8200"
        tls_disable: 1
  backend:
    - file:
        path: "{{ vault_root }}/data"    

vault_policies:
  superadmin: 
    path: 
      '*': 
        capabilities: ["sudo" ]  
  'superreader': 
    path: 
      '*':
        capabilities:  ["read", "list" ]

vault_auth_backends:
  userpass: 
    type: userpass

vault_secret_backends:
  buildusers:
    type: generic
    config: 
      default_lease_ttl: 0
      max_lease_ttl: 0

```

Dependencies
------------

NA

Technical Debt
------------

Handle Initial Secrets better
Works idempotently for adding config like auth/ backends etc, however cannot reconfigure  existing mounts, auth backends etc. Fails silently without altering the system in such cases

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - ansible-role-hashicorp-vault

License
-------

Apache License 2.0

Author Information
------------------

Abhay Chrungoo <abhay@ziraffe.io> 