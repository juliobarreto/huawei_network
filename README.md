# Automação de switches Huawei para Red Hat AAP

Este projeto contém roles do Ansible para backup, restauração e aplicação de ACLs em switches Huawei.


## Passo a Passo

1. **Fazer upload do projeto para o AAP**:
   - No painel do AAP, vá até "Projects" e adicione um novo projeto.
   - Escolha "Git" como fonte e informe a URL do repositório contendo este projeto.
   - Sincronize o projeto para carregar os arquivos.

2. **Configurar as credenciais**:
   - Vá até "Credentials" e adicione credenciais para acessar os switches Huawei.
   - Se necessário, adicione credenciais para acessar o repositório Git.

3. **Criar um Inventory**:
   - Vá até "Inventories" e crie um novo inventário.
   - Adicione um grupo chamado `huawei` e cadastre os switches Huawei.
   - Configure as variáveis globais conforme necessário.

4. **Criar um Job Template**:
   - Vá até "Templates" e crie um novo template de job.
   - Escolha o inventário e o projeto criados anteriormente.
   - Selecione o playbook `playbook.yml`.
   - Defina as variáveis extras conforme necessário, como:
     ```yaml
     backup_enabled: true
     restore_enabled: false
     apply_acls_enabled: true
     ```

5. **Criar Formulários no AAP**:
   - Vá até "Templates" e edite o template criado.
   - Habilite a opção "Prompt on launch" para permitir a entrada dinâmica de variáveis.
   - Adicione os seguintes campos no formulário:
     - `backup_enabled` (booleano): Define se o backup deve ser realizado antes das mudanças.
     - `restore_enabled` (booleano): Define se a restauração do último backup deve ser feita.
     - `apply_acls_enabled` (booleano): Define se as ACLs devem ser aplicadas.
     - `acl_rules` (JSON/YAML): Permite a entrada manual de regras ACL no momento da execução.
   - Salve as alterações e teste a execução do job template.

6. **Executar o template**:
   - Salve e inicie o job template.
   - Monitore os logs de execução para verificar possíveis erros.

Ao executar o job template, um exemplo de preenchimento das variáveis pode ser:

```yaml
backup_enabled: true
restore_enabled: false
apply_acls_enabled: true
acl_rules:
  - acl_number: 3001
    entries:
      - rule_id: 5
        action: permit
        source: 192.168.1.0 0.0.0.255
      - rule_id: 10
        action: deny
        source: any
  - acl_number: 3002
    entries:
      - rule_id: 5
        action: permit
        source: 10.0.0.0 0.255.255.255
      - rule_id: 15
        action: deny
        source: any
  - acl_number: 3003
    entries:
      - rule_id: 7
        action: permit
        source: 172.16.0.0 0.0.255.255
      - rule_id: 20
        action: deny
        source: any
```

## Definição de ACLs

Para definir ACLs personalizadas para os switches Huawei, adicione a variável `acl_rules` no inventário ou na execução do template. Segue um exemplo de configuração:

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

- `acl_number`: Define o número da ACL a ser configurada no switch.
- `rule_id`: Identificador da regra dentro da ACL.
- `action`: Ação a ser tomada (`permit` ou `deny`).
- `source`: Endereço IP ou máscara correspondente à regra.

Essa configuração será aplicada aos switches pertencentes ao grupo `huawei` no inventário.
