## Variables Precedence

| Order | Variable                                                                 | Applied |
|:-----:|--------------------------------------------------------------------------|:-------:|
|   1   | command line values (for example, `-u my_user`, these are not variables) |    X    |
|   2   | role defaults (defined in role/defaults/main.yml)                        |    X    |
|   3   | inventory file or script group vars                                      |    X    |
|   4   | inventory group_vars/all                                                 |    X    |
|   5   | playbook group_vars/all                                                  |    X    |
|   6   | inventory group_vars/*                                                   |    X    |
|   7   | playbook group_vars/*                                                    |    X    |
|   8   | inventory file or script host vars                                       |    X    |
|   9   | inventory host_vars/*                                                    |    X    |
|  10   | playbook host_vars/*                                                     |    X    |
|  11   | host facts / cached set_facts                                            |    X    |
|  12   | play vars                                                                |    X    |
|  13   | play vars_prompt                                                         |    X    |
|  14   | play vars_files                                                          |    X    |
|  15   | role vars (defined in role/vars/main.yml)                                |    X    |
|  16   | block vars (only for tasks in block)                                     |    X    |
|  17   | task vars (only for the task)                                            |    X    |
|  18   | include_vars                                                             |    X    |
|  19   | set_facts / registered vars                                              |    X    |
|  20   | role (and include_role) params                                           |    X    |
|  21   | include params                                                           |    X    |
|  22   | extra vars (for example, `-e "user=my_user"`)(always win precedence)     |    O    |

