# View EXPERT_NOTA

Definição da view responsável pelo gerenciamento de Notas Fiscais no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_NOTA (
  "ID",
  "CODIGO",
  "CODFILIALERP",
  "TIPO",
  "CARGA",
  "DATAEMISSAO",
  "QUANTIDADE",
  "NUMERONOTAFISCAL",
  "SERIE",
  "QUANTIDADEVOLUME",
  "VALTOTALPRODUTO",
  "VALTOTALNOTA",
  "PESOBRUTO",
  "PESOLIQUIDO",
  "SITUACAO",
  "QTDVOLUMECONF",
  "CODIGOFORNECEDOR"
) AS 
SELECT 
  DISTINCT 
  CAST(pcnfent.NUMNOTA AS VARCHAR(30)) || 
  CAST(pcnfent.CODFILIAL AS VARCHAR(30)) || 
  CAST(CASE 
         WHEN pcmov.codoper = 'ED' 
         THEN TO_NUMBER('900' || TO_CHAR(pcnfent.CODFORNEC)) 
         ELSE pcnfent.CODFORNEC 
       END AS VARCHAR(30)) || 
  pcnfent.SERIE AS id,

  CAST(pcnfent.NUMNOTA AS VARCHAR(30)) codigo,
  CAST(pcnfent.CODFILIAL AS VARCHAR(30)) codfilial,
  CASE 
    WHEN pcmov.codoper = 'ED' THEN 2
    WHEN pcmov.codoper = 'ET' THEN 3
    ELSE 1
  END AS tipo,
  CAST(pcnfent.NUMBONUS AS VARCHAR(20)) carga,
  pcnfent.dtent dataemissao,
  CAST(0 AS NUMERIC(10,2)) quantidade,
  CAST(pcnfent.NUMNOTA AS VARCHAR(30)) numeronotafiscal,
  pcnfent.serie,
  pcnfent.numvol quantidadevolume,
  pcnfent.vltotal valtotalproduto,
  pcnfent.vltotal valtotalnota,
  pcnfent.totpeso pesobruto,
  pcnfent.totpesoliq pesoliquido,
  1 situacao,
  CAST(0 AS NUMERIC(10,2)) qtdvolumeconf,
  CAST(CASE 
         WHEN pcmov.codoper = 'ED' 
         THEN TO_NUMBER('900' || TO_CHAR(pcnfent.CODFORNEC)) 
         ELSE pcnfent.CODFORNEC 
       END AS VARCHAR(30)) codigofornecedor

FROM pcnfent
JOIN pcmov 
  ON pcmov.numtransent = pcnfent.numtransent 
 AND pcmov.numnota = pcnfent.numnota
WHERE pcmov.codoper IN ('E', 'EB', 'ET', 'EF', 'ED')
  AND pcnfent.especie <> 'OE'
  AND pcnfent.dtent >= TO_DATE('01/10/2024', 'dd/mm/yyyy');

```


Exemplo de json:

```json
{
  "ID": "1234501FORN001S1",
  "CODIGO": "12345",
  "CODFILIALERP": "01",
  "TIPO": 1,
  "CARGA": "BONUS123",
  "DATAEMISSAO": "2024-10-10",
  "QUANTIDADE": 0,
  "NUMERONOTAFISCAL": "12345",
  "SERIE": "S1",
  "QUANTIDADEVOLUME": 10,
  "VALTOTALPRODUTO": 1500.00,
  "VALTOTALNOTA": 1500.00,
  "PESOBRUTO": 120.50,
  "PESOLIQUIDO": 100.25,
  "SITUACAO": 1,
  "QTDVOLUMECONF": 0,
  "CODIGOFORNECEDOR": "FORN001"
}

```


| Campo                | Tipo            | Descrição                                                                            |
| -------------------- | --------------- | ------------------------------------------------------------------------------------ |
| **ID**               | `VARCHAR(100)`  | Identificador único da nota fiscal. <br/>🔴 **Obrigatório**.                         |
| **CODIGO**           | `VARCHAR(30)`   | Código da nota fiscal. <br/>🔴 **Obrigatório**.                                      |
| **CODFILIALERP**        | `VARCHAR(30)`   | Código da filial emissora. <br/>🔴 **Obrigatório**.                                  |
| **TIPO**             | `INTEGER`       | Tipo da nota: 1 = padrão, 2 = devolução, 3 = transferência. <br/>🔴 **Obrigatório**. |
| **CARGA**            | `VARCHAR(20)`   | Código da carga associada. <br/>🔴 **Obrigatório**.                                  |
| **DATAEMISSAO**      | `DATE`          | Data de emissão da nota fiscal. <br/>🔴 **Obrigatório**.                             |
| **QUANTIDADE**       | `NUMERIC(10,4)` | Quantidade de itens. <br/>🔴 **Obrigatório**.                                        |
| **NUMERONOTAFISCAL** | `VARCHAR(50)`   | Número da nota fiscal. <br/>🔴 **Obrigatório**.                                      |
| **SERIE**            | `VARCHAR(50)`   | Série da nota fiscal. <br/>🔴 **Obrigatório**.                                       |
| **QUANTIDADEVOLUME** | `NUMERIC(10,2)` | Quantidade de volumes. <br/>🔴 **Obrigatório**.                                      |
| **VALTOTALPRODUTO**  | `NUMERIC(12,2)` | Valor total dos produtos. <br/>🔴 **Obrigatório**.                                   |
| **VALTOTALNOTA**     | `NUMERIC(12,2)` | Valor total da nota. <br/>🔴 **Obrigatório**.                                        |
| **PESOBRUTO**        | `NUMERIC(18,6)` | Peso bruto da carga.                                                                 |
| **PESOLIQUIDO**      | `NUMERIC(18,6)` | Peso líquido da carga.                                                               |
| **SITUACAO**         | `INTEGER`       | Situação da nota (1 = ativa).                                                        |
| **QTDVOLUMECONF**    | `NUMERIC(10,2)` | Quantidade de volumes confirmados.                                                   |
| **CODIGOFORNECEDOR** | `VARCHAR(30)`   | Código do fornecedor. <br/>🔴 **Obrigatório**.                                       |

