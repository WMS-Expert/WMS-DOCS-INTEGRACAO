# View EXPERT_PEDIDOSITEM

Definição da view responsável pelo gerenciamento dos itens dos Pedidos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PEDIDOSITEM (
  "ID",
  "CODIGOPEDIDO",
  "ITEMPEDIDO",
  "CODIGOFILIAL",
  "CODIGOPRODUTO",
  "QUANTIDADE",
  "QTDSEPARADA",
  "QTDCONFERIDA",
  "QTDCORTADA"
) AS 
SELECT 
  ROWNUM AS id,
  CAST(PCPEDI.NUMPED AS varchar(30)) codigopedido,
  CAST(PCPEDI.NUMSEQ AS varchar(30)) itempedido,  
  CAST(PCPEDC.CODFILIAL AS varchar(30)) codigofilial,
  CAST(PCPEDI.CODPROD AS varchar(30)) codigoproduto,
  CAST(PCPEDI.QT AS numeric(10,4)) quantidade,
  CAST(0 AS numeric(10,4)) qtdseparada,
  CAST(0 AS numeric(10,4)) qtdconferida,
  CAST(0 AS numeric(10,4)) qtdcortada
FROM PCPEDI 
JOIN PCPEDC ON PCPEDC.NUMPED = PCPEDI.NUMPED
WHERE PCPEDC.LOCALIZACAOPEDIDO IS NULL
  AND PCPEDC."DATA" >= (SYSDATE - 90)
  AND PCPEDC.CODCLI <> 3;

```


Exemplo de json:

```json
{
  "ID": 1,
  "CODIGOPEDIDO": "456789",
  "ITEMPEDIDO": "001",
  "CODIGOFILIAL": "1",
  "CODIGOPRODUTO": "987654",
  "QUANTIDADE": 10.0000,
  "QTDSEPARADA": 0.0000,
  "QTDCONFERIDA": 0.0000,
  "QTDCORTADA": 0.0000
}

```

| Campo             | Tipo            | Descrição                                                        |
| ----------------- | --------------- | ---------------------------------------------------------------- |
| **ID**            | `INTEGER`       | Identificador da linha (gerado com `ROWNUM`). 🔴 **Obrigatório** |
| **CODIGOPEDIDO**  | `VARCHAR(30)`   | Código do pedido ao qual o item pertence. 🔴 **Obrigatório**     |
| **ITEMPEDIDO**    | `VARCHAR(50)`   | Sequencial do item no pedido. 🔴 **Obrigatório**                 |
| **CODIGOFILIAL**  | `VARCHAR(30)`   | Código da filial de origem do pedido. 🔴 **Obrigatório**         |
| **CODIGOPRODUTO** | `VARCHAR(30)`   | Código do produto. 🔴 **Obrigatório**                            |
| **QUANTIDADE**    | `NUMERIC(10,4)` | Quantidade total solicitada do produto. 🔴 **Obrigatório**       |
| **QTDSEPARADA**   | `NUMERIC(10,4)` | Quantidade separada.                                             |
| **QTDCONFERIDA**  | `NUMERIC(10,4)` | Quantidade conferida.                                            |
| **QTDCORTADA**    | `NUMERIC(10,4)` | Quantidade cortada.                                              |

