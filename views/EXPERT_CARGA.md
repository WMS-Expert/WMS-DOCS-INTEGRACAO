# View EXPERT_CARGA

Definição da view responsável pelo gerenciamento de cargas no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_CARGA (
  "CODIGOCARGA",
  "CODIGOFILIAL",
  "DATAINICIOCONF",
  "DATAFIMCONF",
  "DATAEXPORTACAOERP",
  "QTDVOLUMES"
) AS 
  SELECT 
    CAST(PCBONUSC.NUMBONUS AS VARCHAR(20)) codigocarga,
    CAST(PCBONUSC.CODFILIAL AS VARCHAR(20)) codigofilial,
    CAST(PCBONUSC.DATAINICIO AS TIMESTAMP) datainicioconf,
    CAST(PCBONUSC.DATAFIM AS TIMESTAMP) datafimconf,
    CAST(PCBONUSC.DATABONUS AS TIMESTAMP) dataexportacaoerp,
    CAST(0 AS integer) qtdvolumes
  FROM PCBONUSC  
  WHERE pcbonusc.DATABONUS >= TO_DATE('01/10/2024', 'dd/mm/yyyy');
```

Json de exemplo:

```json
{
  "CODIGOCARGA": "123456",
  "CODIGOFILIAL": "01",
  "DATAINICIOCONF": "2024-10-05T08:00:00",
  "DATAFIMCONF": "2024-10-05T10:30:00",
  "DATAEXPORTACAOERP": "2024-10-06T00:00:00",
  "QTDVOLUMES": 0
}
```



| Campo                 | Tipo          | Descrição                                                                                   |
| --------------------- | ------------- | ------------------------------------------------------------------------------------------- |
| **CODIGOCARGA**       | `VARCHAR(20)` | Código identificador da carga. 🔴 **Obrigatório**.         |
| **CODIGOFILIAL**      | `VARCHAR(20)` | Código da filial da carga. 🔴 **Obrigatório**.            |
| **DATAINICIOCONF**    | `TIMESTAMP`   | Data e hora de início da conferência da carga.                |
| **DATAFIMCONF**       | `TIMESTAMP`   | Data e hora de fim da conferência da carga.                      |
| **DATAEXPORTACAOERP** | `TIMESTAMP`   | Data de exportação para o ERP.                                 |
| **QTDVOLUMES**        | `INTEGER`     | Quantidade de volumes. 🔴 **Obrigatório**. |

