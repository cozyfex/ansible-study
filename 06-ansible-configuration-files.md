## Ansible Configuration Files

### /etc/ansible/ansible.cfg

```ini
[defaults]

[inventory]

[privilege_escalation]

[paramiko_connection]

[ssh_connection]

[persistent_connection]

[colors]

```

### Ansible Config Priority

| Order | Location                              |    Explain     | Applied |
|:-----:|---------------------------------------|:--------------:|:-------:|
|   1   | `/etc/ansible/ansible.cfg`            | Ansible System |    X    |
|   2   | `~/.ansible.cfg`                      |   User Home    |    X    |
|   3   | `playbook/ansible.cfg`                |    Playbook    |    X    |
|   4   | `ANSIBLE_CONFIG=/opt/ansible-web.cfg` |  Environment   |    O    |

### View Configuration

#### List all configurations

```shell
ansible-config list
```

#### Show the current config file

```shell
ansible-config view
```

#### Show the current settings

```shell
ansible-config dump
```
