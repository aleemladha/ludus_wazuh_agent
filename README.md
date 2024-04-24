# Ansible Role: Wazuh Agent Deployment

An Ansible role that deploys Wazuh Agents to Windows, Debian, and Ubuntu systems.

## Description

- The role checks if the Wazuh Agents have been downloaded to the Ludus host. If not, it will attempt to download the agents.
- Agent versions can be [found here](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)
- The role is designed to work with Windows, Debian, Ubuntu systems.
- This role compliments the [ludus_wazuh_siem](https://github.com/aleemladha/wazuh_server_install)


## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):


    # `ludus range status` will provide you with the IP address
    ludus_wazuh_siem_server: ""

## Dependencies

None.

## Example Playbook

```yaml
- hosts: wazuh-agent
  roles:
    - aleemladha.ludus_wazuh_agent
  role_vars:
    - ludus_wazuh_siem_server: "wazuh siem server ip" #10.x.x.x
```

## Example Ludus Range Config

```yaml
ludus:
  - vm_name: "{{ range_id }}-jumpbox01"
    hostname: "{{ range_id }}-jumpbox01"
    template: win2019-server-x64-template
    vlan: 20
    ip_last_octet: 25
    ram_gb: 4
    cpus: 2
    linux: true
    testing:
      snapshot: false
      block_internet: false
    roles:
      - aleemladha.ludus_wazuh_agent # role_vars are not required when using ludus
```

## Ludus setup

```
# Add the role to your ludus host
ludus ansible roles add aleemladha.ludus_wazuh_agent

# Get your config into a file so you can assign to your VMs
ludus range config get > config.yml

# Edit config to add the role to the VMs you wish to make an wazuh server
ludus range config set -f config.yml

# Deploy the range with the user-defined-roles ONLY :)
ludus range deploy -t user-defined-roles
```

## License

GPLv3

## Author Information

This role was created by [Aleem Ladha](https://twitter.com/LadhaAleem), for [Ludus](https://ludus.cloud/).
