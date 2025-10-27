# View EXPERT_CARGA

Definição da view responsável pelo gerenciamento de cargas no sistema.  
View responsável por representar os dados de cargas.  Filtra os registros e converte os campos relevantes para os tipos utilizados pela integração.

## Código SQL

```sql
CREATE VIEW EXPERT_CARGA (
  "CODIGOCARGA",
  "CODIGOFILIAL",
  "NOME",
  "DATAGERACAO"
) AS 
  SELECT 
    CAST(PCBONUSC.NUMBONUS AS VARCHAR(20)) codigocarga,
    CAST(PCBONUSC.CODFILIAL AS VARCHAR(20)) codfilialerp,
    CAST(PCBONUSC.NOME AS VARCHAR(100)) datainicioconf,
    CAST(PCBONUSC.DATAGERACAO AS DATE) datageracao
  FROM PCBONUSC  
  WHERE pcbonusc.DATABONUS >= TO_DATE('01/10/2024', 'dd/mm/yyyy');
```

Json de exemplo:

```json
{
  "CODIGOCARGA": "123456",
  "CODIGOFILIAL": "01",
  "NOME": "Carga A",
  "DATAGERACAO": "01/01/2025"
}
```



| Campo                 | Tipo          | Descrição                                                                                   |
| --------------------- | ------------- | ------------------------------------------------------------------------------------------- |
| **CODIGOCARGA**       | `VARCHAR(20)` | Código identificador da carga. 🔴 **Obrigatório**.         |
| **CODFILIALERP**      | `VARCHAR(20)` | Código da filial da carga. 🔴 **Obrigatório**.            |
| **NOME**    | `VARCHAR(100)`   | Nome da carga.
| **DATAGERACAO**       | `DATE`   | Data de geração da carga. 🔴 **Obrigatório**.                    |

