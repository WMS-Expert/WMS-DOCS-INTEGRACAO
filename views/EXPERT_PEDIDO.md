# View EXPERT_PEDIDO

Definição da view responsável pelo gerenciamento dos Pedidos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PEDIDO (
  "ID",
  "CODIGO",
  "VENDEDOR",
  "TIPO",
  "DATAEMISSAO",
  "DATAIMPORTACAO",
  "CODIGOCLIENTE",
  "CODFILIALERP",
  "TOTALPEDIDO",
  "QTDITENS",
  "ORDEMPEDIDO",
  "STATUS",
  "DTEXPORTACAO",
  "CODIGOREF",
  "OBS",
  "CODIGOCARREGAMENTO",
  "DESCRICAOCONSUMIDOR"
) AS 
SELECT 
  ROWNUM AS id,
  CAST(pcpedc.numped AS VARCHAR(30)) codigo,
  'Teste' vendedor,
  1 tipo,
  COALESCE(pcpedc.DTEMISSAOMAPA, pcpedc.Data) dataemissao,
  COALESCE(pcpedc.DTEMISSAOMAPA, pcpedc.Data) dataimportacao,
  CAST(pcpedc.codcli AS VARCHAR(30)) codigocliente,
  CAST(pcpedc.codfilial AS VARCHAR(30)) codigofilial,
  CAST(pcpedc.vltotal AS NUMERIC(10,4)) totalpedido,
  (SELECT COUNT(CODPROD) FROM PCPEDI WHERE pcpedi.numped = pcpedc.numped) qtditens,
  pcpedc.NUMSEQMONTAGEM ordempedido,
  1 status,
  NULL dtexportacao,
  CAST(pcpedc.numped AS VARCHAR(30)) codigoref,
  CASE 
    WHEN NVL(pcpedc.OBS1, '') = '' AND NVL(pcpedc.OBS, '') = '' AND NVL(pcpedc.OBS2, '') = '' THEN NULL
    WHEN NVL(pcpedc.OBS1, '') <> '' AND NVL(pcpedc.OBS2, '') = '' THEN 'OBS01 - ' || pcpedc.OBS || ' | OBS2 - ' || pcpedc.OBS1
    WHEN NVL(pcpedc.OBS2, '') <> '' AND NVL(pcpedc.OBS1, '') = '' THEN 'OBS01 - ' || pcpedc.OBS || ' | OBS3 - ' || pcpedc.OBS2
    WHEN NVL(pcpedc.OBS, '') = '' THEN 'OBS02 - ' || pcpedc.OBS1 || ' | OBS3 - ' || pcpedc.OBS2
    ELSE 'OBS01 - ' || pcpedc.OBS || ' | OBS2 - ' || pcpedc.OBS1 || ' | OBS3 - ' || pcpedc.OBS2
  END AS obs,
  CAST(pcpedc.numcar AS VARCHAR(30)) codigocarregamento,
  pcclient.CLIENT descricaoconsumidor
FROM pcpedc
LEFT JOIN pcclient ON pcclient.codcli = pcpedc.codcli
LEFT JOIN pcpraca ON pcpraca.codpraca = pcclient.codpraca
LEFT JOIN pcrotaexp ON pcrotaexp.codrota = pcpraca.rota
WHERE (pcpedc.posicao = 'L' OR pcpedc.posicao = 'M')
  AND pcpedc.localizacaopedido IS NULL
  AND pcpedc.DATA >= (SYSDATE - 90)
  AND pcpedc.codcli <> 3;


```



Exemplo de json:

```json
{
  "ID": 1,
  "CODIGO": "456789",
  "VENDEDOR": "Teste",
  "TIPO": 1,
  "DATAEMISSAO": "2024-07-21",
  "DATAIMPORTACAO": "2024-07-21",
  "CODIGOCLIENTE": "12345",
  "CODFILIALERP": "1",
  "TOTALPEDIDO": 1250.75,
  "QTDITENS": 4,
  "ORDEMPEDIDO": 789,
  "STATUS": 1,
  "DTEXPORTACAO": null,
  "CODIGOREF": "456789",
  "OBS": "OBS01 - Pedido urgente | OBS2 - Entregar pela manhã",
  "CODIGOCARREGAMENTO": "998",
  "DESCRICAOCONSUMIDOR": "JOÃO DA SILVA"
}

```


| Campo                   | Tipo            | Descrição                                            |
| ----------------------- | --------------- | ---------------------------------------------------- |
| **ID**                  | `INTEGER`       | Identificador único da linha. 🔴 **Obrigatório**.    |
| **CODIGO**              | `VARCHAR(30)`   | Código do pedido. 🔴 **Obrigatório**.                |
| **VENDEDOR**            | `VARCHAR(100)`  | Nome do vendedor. 🔴 **Obrigatório**.                |
| **TIPO**                | `INTEGER`       | Tipo do pedido. (fixo: 1) 🔴 **Obrigatório**.        |
| **DATAEMISSAO**         | `DATE`          | Data da emissão do pedido.                           |
| **DATAIMPORTACAO**      | `DATE`          | Data de importação do pedido.                        |
| **CODIGOCLIENTE**       | `VARCHAR(30)`   | Código do cliente. 🔴 **Obrigatório**.               |
| **CODFILIALERP**           | `VARCHAR(30)`   | Código da filial. 🔴 **Obrigatório**.                |
| **TOTALPEDIDO**         | `NUMERIC(10,4)` | Valor total do pedido. 🔴 **Obrigatório**.           |
| **QTDITENS**            | `INTEGER`       | Quantidade de itens no pedido.                       |
| **ORDEMPEDIDO**         | `INTEGER`       | Número sequencial da montagem do pedido.             |
| **STATUS**              | `INTEGER`       | Status do pedido (fixo: 1).                          |
| **DTEXPORTACAO**        | `DATE`          | Data de exportação do pedido.                        |
| **CODIGOREF**           | `VARCHAR(30)`   | Código de referência do pedido.                      |
| **OBS**                 | `VARCHAR(500)`  | Observações complementares do pedido.                |
| **CODIGOCARREGAMENTO**  | `VARCHAR(30)`   | Código de carregamento. 🔴 **Obrigatório**.          |
| **DESCRICAOCONSUMIDOR** | `VARCHAR(150)`  | Nome ou descrição do consumidor. 🔴 **Obrigatório**. |

