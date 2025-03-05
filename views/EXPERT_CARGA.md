# View EXPERT_CARGA

Definição da view responsável pelo gerenciamento de cargas no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_CARGA (
  "CODIGOCARGA",
  "NOMECARGA",
  "DATAGERACAO",
  "CODIGOFILIAL"
) AS 
  SELECT 
    CAST(PCBONUSC.NUMBONUS AS VARCHAR(20)) codigocarga,
    NULL AS nomecarga,
    CAST (PCBONUSC.DATABONUS AS TIMESTAMP) datageracao,
    CAST(PCBONUSC.CODFILIAL AS VARCHAR(20)) codigofilial
  FROM PCBONUSC  
  WHERE pcbonusc.DATABONUS >= TO_DATE('01/10/2024', 'dd/mm/yyyy');
```

**CODIGOCARGA** : *O campo deve ser **VARCHAR(20)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>*** <br/>
**NOMECARGA** : *O campo deve ser **VARCHAR(100)**, contendo o nome da carga.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOFILIAL** : *O campo deve ser **VARCHAR(20)**, contem o codigo da filial do cliente, sendo um campo.* ***<font color="red"> - obrigartorio</font>***<br/>
**DATAGERACAO** : *O campo deve ser **DATE**, trazendo a data de geração da carga.* <br/>
