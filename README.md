![Vault](https://www.vaultproject.io/assets/images/mega-nav/logo-vault-0f83e3d2.svg) <h1>Vault</h1>
A no nonsense Ansible role to deploy and configure [Vault](https://vaultproject.io)

## Features
### Configure Vault with YAML
Configuration for the Vault service is done using YAML to JSON conversion, so you can express your Vault configuration like so:  
```yaml
vault_config:
  cluster_name: dc1
  storage:
    consul:
      address: 127.0.0.1:8500
      token: someToken
      path: vault
  listener:
    tcp:
      address: 127.0.0.1:8200
      tls_disable: 1
```
This is done using Jinja2 filters. This role implements no preconfigured entries for configuration, so rather than write your Vault's configuration in JSON or HCL; you simply express it in YAML's equivilent syntax, which means it can sit anywhere in your Ansible configuration.  
This can be quite powerful, as it allows you to leverage other filters available in Ansible to construct arbitrary data and use it in the configuration. Anything you can express with Ansible's templating (including fetching data/host information from the inventory, etc.) can be used in the configuration.  

If you don't know how to express your Vault's JSON configuration as YAML, see [here for a handy converter](https://www.json2yaml.com/). Or, if you're using HCL, check out [hcl-to-json](https://www.npmjs.com/package/hcl-to-json).  

### Packer provisioning support
A very simple, but handy feature of this role is the ability to set `vault_packer_provision` to `true` (`false` by default). When this is `true`, during the Ansible run, it won't start the Vault service. This exists so that you can place values into your configuration that may not be valid, intended to be used when producing machine images of some sort with Packer, intended to be substituted later on by way of some sort of launch configuration/user script/user data.  

## Default Role Variables

```yaml
vault_packer_provision: false
vault_group_name: vault
vault_group_gid: 3000
vault_user_name: vault
vault_user_uid: 3000
vault_user_home: /opt/vault
vault_config_dir: "{{ vault_user_home }}/etc"
vault_version: 0.7.3
vault_uri: "https://releases.hashicorp.com/vault/{{ vault_version }}/vault_{{ vault_version }}_linux_amd64.zip"
vault_config_src: main.json.j2
vault_service_file:
  src: vault.service.j2
  dest: /etc/systemd/system/vault.service

vault_config:
  cluster_name: dc1
  storage:
    consul:
      address: 127.0.0.1:8500
      token: someToken
      path: vault
  listener:
    tcp:
      address: 127.0.0.1:8200
      tls_disable: 1
```

## License
MIT

## Author Information
Created by Calum MacRae

Feel free to:  
- Contact me: [@calumacrae](https://twitter.com/calumacrae), [calum0macrae@gmail.com](mailto:calum0macrae@gmail.com)  
- [Raise an issue](https://github.com/cmacrae/ansible-vault/issues)  
- [Contribute](https://github.com/cmacrae/ansible-vault/pulls)  
