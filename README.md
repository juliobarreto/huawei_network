# Huawei Ansible Automation

Este projeto contém roles do Ansible para backup, restauração e aplicação de ACLs em switches Huawei.

## Uso

Defina no inventário os hosts do grupo **huawei** e utilize as seguintes variáveis para controlar a execução:

- **backup_enabled**:  para ativar o backup (padrão: true)
- **restore_enabled**:  para restaurar o último backup (padrão: false)
- **apply_acls_enabled**:  para aplicar ACLs (padrão: false)

