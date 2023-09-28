## Jinja2

### String Manipulation

```
The name is {{ my_name }} => The name is Bond

The name is {{ my_name | upper }} => The name is BOND

The name is {{ my_name | lower }} => The name is bond

The name is {{ my_name | title }} => The name is Bond

The name is {{ my_name | replace("Bond", "Bourne") }} => The name is Bourne

The name is {{ first_name | default("James") }} {{ my_name }} => The name is James Bond
```

### Filters - List and Set

```
{{ [1, 2, 3] | min }} => 1
{{ [1, 2, 3] | max }} => 3
{{ [1, 2, 3, 2] | unique }} => 1, 2, 3
{{ [1, 2, 3, 4] | union([4, 5]) }} => 1, 2, 3, 4, 5
{{ [1, 2, 3, 4] | intersect([4, 5]) }} => 4
{{ 100 | ramdom }} => Random number
{{ ["The", "name", "is", "Bond"] | join("") }} => The name is Bond
```

### Loops

```
{% for number in [0, 1, 2, 3, 4] %}
    {{ number }}
{% endfor %}
```

```shell
0
1
2
3
4
```

### Conditions

```
{% for number in [0, 1, 2, 3, 4] %}
    {% if number == 2 %}
        {{ number }}
    {% endif %}
{% endfor %}
```

```shell
2
```

### Filter - file

```
{{ "/etc/hosts" | basename }} => hosts
{{ "c:\windows\hosts" | win_basename }} => hosts
{{ "c:\windows\hosts" | win_splitdrive }} => ["c:", "\windows\hosts"]
{{ "c:\windows\hosts" | win_splitdrive | first }} => "c:"
{{ "c:\windows\hosts" | win_splitdrive | last }} => "\windows\hosts"
```

### Jinja2 in Playbooks(templates)

#### /etc/ansible/hosts

```
[web_servers]
web1 ansible_host=172.20.1.100 dns_server=10.5.5.4 inventory_hostname=web1
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4 inventory_hostname=web2
web3 ansible_host=172.20.1.102 dns_server=10.5.5.4 inventory_hostname=web3
```

#### Playbook Example 1

- playbook.yaml

```yaml
---
- name: Update dns server
  hosts: all
  tasks:
    - nsupdate:
        server: "{{ dns_server }}"
```

- Result

```yaml
---
- name: Update dns server
  hosts: all
  tasks:
    - nsupdate:
        server: 10.5.5.4
```

#### Playbook Example 2

- index.html

```html
<!DOCTYPE html>
<html lang="kr">
<body>
This is Web Server
</body>
</html>
```

- playbook.yaml

```yaml
- name: Web Server
  hosts: web_servers
  tasks:
    - name: Copy index.html to remote servers
      copy:
        src: index.html
        dest: /var/www/nginx-default/index.html
```

- Result

```
This is Web Server
```

#### Playbook Example 3

- index.html.j2

```html
<!DOCTYPE html>
<html lang="kr">
<body>
This is {{ inventory_hostname }} Server
</body>
</html>
```

- playbook.yaml

```yaml
- name: Web Server
  hosts: web_servers
  tasks:
    - name: Copy index.html to remote servers
      template:
        src: index.html.j2
        dest: /var/www/nginx-default/index.html
```

- Result web1

```
This is web1 Server
```

- Result web2

```
This is web2 Server
```

- Result web3

```
This is web3 Server
```

### Template Example 1

- nginx.conf.j2

```
server {
    location / {
        fastcgi_pass {{ host }}:{{ port }};
        fastcgi_param QUERY_STRING $query_string;
    }
    
    location ~ \ gif|jpg|png $ {
        root {{ image_path }};
    }
}
```

- Result nginx.conf

```
server {
    location / {
        fastcgi_pass localhost:9000;
        fastcgi_param QUERY_STRING $query_string;
    }
    
    location ~ \ gif|jpg|png $ {
        root /data/images;
    }
}
```

### Template Example 2

- redis.conf.j2

```
bind {{ ip_address }}

protected-mode yes

port {{ redis_port | default(6379) }}

tcp-backlog 511

# Unix socket
timeout 0

# TCP keepalive
tcp-keepalive {{ tcp_keepalive | default(300) }}

daemonize no

supervised no
```

- redis.conf

```
bind 192.168.1.100

protected-mode yes

port 6379

tcp-backlog 511

# Unix socket
timeout 0

# TCP keepalive
tcp-keepalive 300

daemonize no

supervised no
```

### Template Example 3

- Variables

```yaml
name_servers:
  - 10.1.1.2
  - 10.1.1.3
  - 8.8.8.8
```

- resolve.conf.j2

```
{% for name_server in name_servers %}
nameserver {{ name_server }}
{% endfor %}
```

- /etc/resolve.conf

```ini
nameserver 10.1.1.2
nameserver 10.1.1.3
nameserver 8.8.8.8
```

