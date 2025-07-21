# View EXPERT_NOTAITEM

Definição da view responsável pelo gerenciamento dos itens das Notas Fiscais no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_NOTAITEM (
  "ID",
  "CODIGONOTA",
  "ITEM",
  "CODIGOPRODUTO",
  "CODIGOFILIAL",
  "TIPO",
  "CODIGOFORNECEDOR",
  "QUANTIDADE",
  "VALUNITARIO",
  "VALIDADE",
  "FABRICACAO",
  "LOTE",
  "NUMSERIE",
  "EMBALAGEM"
) AS 
SELECT DISTINCT
  ROWNUM AS id,
  CAST(pcmov.numnota AS VARCHAR(30)) codigonota,
  pcmov.SEQMOV item,
  CAST(pcmov.CODPROD AS VARCHAR(30)) codigoproduto,
  CAST(pcnfent.CODFILIAL AS VARCHAR(30)) codigofilial,
  CASE 
    WHEN pcmov.codoper = 'ED' THEN 2
    WHEN pcmov.codoper = 'ET' THEN 3
    ELSE 1 
  END AS tipo,
  CAST(CASE 
    WHEN pcmov.codoper = 'ED' THEN TO_NUMBER('900' || TO_CHAR(pcnfent.CODFORNEC))
    ELSE pcnfent.CODFORNEC
  END AS VARCHAR(30)) codigofornecedor,
  CAST(SUM(pcmov.qt) AS NUMERIC(10,4)) quantidade,
  CAST(COALESCE(pcmov.valorultent, 0) AS NUMERIC(10,4)) valunitario,
  pcmov.DATAVALIDADE validade,
  pcmov.DATAFABRICACAO fabricacao,
  pcmov.NUMLOTE lote,
  NULL numserie,
  NULL embalagem
FROM pcmov
JOIN pcnfent 
  ON pcmov.numtransent = pcnfent.numtransent 
 AND pcmov.numnota = pcnfent.numnota
WHERE pcmov.codoper IN ('E', 'EB', 'ET', 'EF', 'ED')
  AND pcnfent.especie <> 'OE'
  AND pcnfent.dtent >= TO_DATE('01/10/2024', 'dd/mm/yyyy')
GROUP BY 
  ROWNUM, pcmov.SEQMOV, pcmov.numnota, pcnfent.CODFILIAL, pcnfent.CODFORNEC,
  pcmov.CODPROD, pcmov.valorultent, pcmov.DATAVALIDADE, pcmov.DATAFABRICACAO,
  pcmov.NUMLOTE, pcmov.codoper;


```

Exemplo de json:

```json
{
  "ID": 1,
  "CODIGONOTA": "123456",
  "ITEM": 1,
  "CODIGOPRODUTO": "PROD001",
  "CODIGOFILIAL": "01",
  "TIPO": 1,
  "CODIGOFORNECEDOR": "FORN001",
  "QUANTIDADE": 25.5,
  "VALUNITARIO": 3.75,
  "VALIDADE": "2024-12-31",
  "FABRICACAO": "2024-10-01",
  "LOTE": "L123ABC",
  "NUMSERIE": null,
  "EMBALAGEM": null
}

```


| Campo                | Tipo            | Descrição                                                                             |
| -------------------- | --------------- | ------------------------------------------------------------------------------------- |
| **ID**               | `INTEGER`       | Identificador do item. <br/>🔴 **Obrigatório**.                                       |
| **CODIGONOTA**       | `VARCHAR(30)`   | Código da nota fiscal. <br/>🔴 **Obrigatório**.                                       |
| **ITEM**             | `VARCHAR(50)`   | Sequência do item na nota. <br/>🔴 **Obrigatório**.                                   |
| **CODIGOPRODUTO**    | `VARCHAR(30)`   | Código do produto. <br/>🔴 **Obrigatório**.                                           |
| **CODIGOFILIAL**     | `VARCHAR(30)`   | Código da filial. <br/>🔴 **Obrigatório**.                                            |
| **TIPO**             | `INTEGER`       | Tipo da nota (1 = padrão, 2 = devolução, 3 = transferência). <br/>🔴 **Obrigatório**. |
| **CODIGOFORNECEDOR** | `VARCHAR(30)`   | Código do fornecedor. <br/>🔴 **Obrigatório**.                                        |
| **QUANTIDADE**       | `NUMERIC(10,4)` | Quantidade do item. <br/>🔴 **Obrigatório**.                                          |
| **VALUNITARIO**      | `NUMERIC(10,4)` | Valor unitário do item. <br/>🔴 **Obrigatório**.                                      |
| **VALIDADE**         | `DATE`          | Data de validade do produto.                                                          |
| **FABRICACAO**       | `DATE`          | Data de fabricação do produto.                                                        |
| **LOTE**             | `VARCHAR(50)`   | Código do lote do produto.                                                            |
| **NUMSERIE**         | `VARCHAR(50)`   | Número de série do item.                                                              |
| **EMBALAGEM**        | `VARCHAR(20)`   | Tipo ou código da embalagem.                                                          |

