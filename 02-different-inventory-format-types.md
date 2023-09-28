## Different Inventory Format Types

### INI

```ini
[web_servers]
web1.example.com
web2.example.com

[db_servers]
db1.example.com
db2.example.com
```

### YAML

```yaml
all:
  children:
    web_servers:
      hosts:
        web1.example.com:
        web2.example.com:
    db_servers:
      hosts:
        db1.example.com:
        db2.example.com:
```

