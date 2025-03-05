# View EXPERT_CARREGAMENTO

Definição da view responsável pelo gerenciamento de Carregamento no sistema.  

## Código SQL

```sql
CREATE OR REPLACE VIEW EXPERT_CARREGAMENTO AS
 
 SELECT
    CAST(PCPEDC.NUMCAR AS VARCHAR(255)) AS CODIGO,
    CAST(PCCARREG.DATAMON AS DATE) AS DATAGERACAO,
    CAST(PCPEDC.codfilial AS VARCHAR(30)) AS CODFILIAL,
    CAST(PCPEDC.NUMPED AS VARCHAR(30)) AS CODIGOPEDIDO
FROM
    PCPEDC
JOIN
    PCCARREG ON PCCARREG.numcar = PCPEDC.numcar
LEFT JOIN
    PCVEICUL ON PCVEICUL.CODVEICULO = PCCARREG.CODVEICULO
WHERE
    PCPEDC.codfilial = 1
```
**CODIGO** : *O campo deve ser **VARCHAR(255)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**DATAGERACAO** : *O campo deve ser **DATE**, trazendo a data de geração do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
**CODFILIAL** : *O campo deve ser **VARHCAR(30)**, trazendo o destino do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOPEDIDO** : *O campo deve ser **VARHCAR(30)**, trazendo o destino do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
