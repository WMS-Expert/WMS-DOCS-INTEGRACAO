# View EXPERT_GRUPO

Definição da view responsável pelo gerenciamento de Grupos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_GRUPO (
  "CODIGO", 
  "NOME", 
  "ATIVO"
) AS 
SELECT
  CAST(pcdepto.codepto AS VARCHAR(255)) codigo,
  pcdepto.descricao nome,
  1 ativo
FROM pcdepto;

```

Exemplo de json:

```json
{
  "CODIGO": "001",
  "NOME": "Grupo de Bebidas",
  "ATIVO": 1
}

```

| Campo      | Tipo           | Descrição                                   |
| ---------- | -------------- | ------------------------------------------- |
| **CODIGO** | `VARCHAR(30)`  | Código do grupo. 🔴 **Obrigatório**.        |
| **NOME**   | `VARCHAR(100)` | Nome do grupo. 🔴 **Obrigatório**.          |
| **ATIVO**  | `INTEGER`      | Indica se o grupo está ativo (`1` = ativo). |

