# Role: apply_acls

Esta role executa a função de **apply_acls** em switches Huawei.

## Exemplo de ACL esperada

```yaml
acl_rules:
  - acl_number: 3001
    entries:
      - rule_id: 5
        action: permit
        source: 192.168.1.0 0.0.0.255
      - rule_id: 10
        action: deny
        source: any
```
