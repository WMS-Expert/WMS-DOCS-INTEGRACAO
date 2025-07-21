# View EXPERT_EMBALAGEM

Definição da view responsável pelo gerenciamento de Embalagens no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_EMBALAGEM (
  "ID", 
  "CODIGO", 
  "CODIGOPRODUTO", 
  "CODBARRA", 
  "QUANTIDADE", 
  "ATIVO", 
  "TIPO", 
  "LARGURA", 
  "ALTURA", 
  "PROFUNDIDADE", 
  "M3", 
  "PESOBRUTO", 
  "PESOLIQUIDO"
)
AS 
SELECT 
  DISTINCT "ID", "CODIGO", "CODIGOPRODUTO", "CODBARRA", "QUANTIDADE", "ATIVO", "TIPO", "LARGURA", "ALTURA", "PROFUNDIDADE", "M3", "PESOBRUTO", "PESOLIQUIDO" 
FROM (
  SELECT 
    DISTINCT ROWNUM AS id, 
    NULL AS codigo, 
    CAST(pcembalagem.CODPROD AS VARCHAR(30)) AS codigoproduto, 
    CAST(codauxiliar AS VARCHAR(20)) AS codbarra, 
    CAST(qtunit AS NUMERIC(10, 4)) AS quantidade, 
    1 AS ativo, 
    CASE WHEN qtunit = 1 THEN 1 ELSE 2 END AS tipo, 
    CAST(pcembalagem.LARGURA AS NUMERIC(10, 4)) AS largura, 
    CAST(pcembalagem.ALTURA AS NUMERIC(10, 4)) AS altura, 
    CAST(pcembalagem.COMPRIMENTO AS NUMERIC(10, 4)) AS profundidade, 
    CAST((pcembalagem.LARGURA * pcembalagem.ALTURA * pcembalagem.COMPRIMENTO) AS NUMERIC(10, 4)) AS m3, 
    CAST(pcembalagem.PESOBRUTO AS NUMERIC(10, 4)) AS pesobruto, 
    CAST(pcembalagem.PESOLIQ AS NUMERIC(10, 4)) AS pesoliquido 
  FROM 
    pcembalagem 
  WHERE 
    codauxiliar IS NOT NULL
);

```

Exemplo de json:

```json
{
  "ID": 1,
  "CODIGO": null,
  "CODIGOPRODUTO": "PROD123",
  "CODBARRA": "7891234567890",
  "QUANTIDADE": 12.0000,
  "ATIVO": 1,
  "TIPO": 2,
  "LARGURA": 0.3000,
  "ALTURA": 0.2000,
  "PROFUNDIDADE": 0.1500,
  "M3": 0.0090,
  "PESOBRUTO": 5.0000,
  "PESOLIQUIDO": 4.5000
}

```
| Campo             | Tipo            | Descrição                                                               |
| ----------------- | --------------- | ----------------------------------------------------------------------- |
| **ID**            | `INTEGER`       | Identificador da embalagem na view. 🔴 **Obrigatório**.            |
| **CODIGO**        | `VARCHAR(30)`   | Código da embalagem (atualmente `NULL`). 🔴 **Obrigatório**.       |
| **CODIGOPRODUTO** | `VARCHAR(30)`   | Código do produto vinculado à embalagem. 🔴 **Obrigatório**.       |
| **CODBARRA**      | `VARCHAR(20)`   | Código de barras da embalagem. 🔴 **Obrigatório**.                 |
| **QUANTIDADE**    | `NUMERIC(10,4)` | Quantidade contida na embalagem. 🔴 **Obrigatório**.               |
| **ATIVO**         | `INTEGER`       | Indica se a embalagem está ativa (1 = sim). 🔴 **Obrigatório**.    |
| **TIPO**          | `INTEGER`       | Tipo da embalagem: 1 = unitária, 2 = múltipla. 🔴 **Obrigatório**. |
| **LARGURA**       | `NUMERIC(10,4)` | Largura da embalagem.                                                   |
| **ALTURA**        | `NUMERIC(10,4)` | Altura da embalagem.                                                    |
| **PROFUNDIDADE**  | `NUMERIC(10,4)` | Profundidade (comprimento) da embalagem.                                |
| **M3**            | `NUMERIC(10,4)` | Volume cúbico (largura × altura × profundidade).                        |
| **PESOBRUTO**     | `NUMERIC(10,4)` | Peso bruto da embalagem.                                                |
| **PESOLIQUIDO**   | `NUMERIC(10,4)` | Peso líquido da embalagem.                                              |

