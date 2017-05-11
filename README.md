Role Name
=========

Create ansible inventory-related environment variable structure (http://docs.ansible.com/ansible/intro_inventory.html#splitting-out-host-and-group-specific-data)

Requirements
------------

None

Role Variables
--------------

```yaml
# an environment setup directory
env_bootstrap_env_dir: "environments/default"

# a list of dynamic/static inventory scripts w/ jinja2 template config parameters
env_bootstrap_dynamic_inventories:
  -
    script: "https://raw.githubusercontent.com/ansible/ansible/stable-2.3/contrib/inventory/vmware_inventory.py"
    config: "vmware_inventory.ini"
    params:
      vmware:
        server: "vcenterserver.vsphere.local"
        port: 443
        username: "administrator@vsphere.local"
        password:
        validate_certs: "False"
        cache_max_age: 1
        max_object_level: 1
  -
    script: "https://raw.githubusercontent.com/ansible/ansible/stable-2.3/contrib/inventory/ec2.py"
    config: "ec2.ini"
    params:
      ec2:
        regions: "all"
        regions_exclude: "us-gov-west-1, cn-north-1"
        destination_variable: "public_dns_name"
        vpc_destination_variable: "ip_address"
        route53: "False"
        cache_path: "~/.ansible/tmp"
        cache_max_age: 300
      credentials:
        aws_access_key_id: "AXXXXXXXXXXXXXX"
        aws_secret_access_key: "XXXXXXXXXXXXXXXXXXX"
        aws_security_token: "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
```

Dependencies
------------

none

Example Playbook
----------------

    - hosts: servers
      vars:
         - my_environment: "environments/myenv"
         - my_scripts:
           -
             script: "https://localhost/my_inventory_script.py"
             config: "my_inventory.ini"
             params:
               foo: "bar"

      roles:
         - { role: amaczuga.env-bootstrap, env_bootstrap_env_dir: "{{ my_environment }}", env_bootstrap_dynamic_inventories: "{{ my_scripts }}" }

License
-------

Apache 2.0

Author Information
------------------

Andrzej Maczuga
