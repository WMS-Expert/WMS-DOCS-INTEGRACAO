# View EXPERT_CARREGAMENTO

Definição da view responsável pelo gerenciamento de Carregamento no sistema.  

## Código SQL

```sql
CREATE OR REPLACE VIEW EXPERT_CARREGAMENTO AS
 
 SELECT
    CAST(PCPEDC.NUMCAR AS VARCHAR(255)) AS CODIGO,
    CAST(NULL AS VARCHAR(100)) AS MOTORISTA,
    CAST(PCCARREG.CODFUNCAJUD AS VARCHAR(100)) AS AJUDANTE1,
    CAST(PCCARREG.CODFUNCAJUD2 AS VARCHAR(100)) AS AJUDANTE2,
    CAST(PCCARREG.CODFUNCAJUD3 AS VARCHAR(100)) AS AJUDANTE3,
    CAST(PCCARREG.DATAMON AS DATE) AS DATAGERACAO,
    CAST(PCCARREG.DTSAIDA AS DATE) AS DATASAIDA,
    CAST(NULL AS DATE) AS DATACHEGADA,
    CAST(PCCARREG.DESTINO AS VARCHAR(20)) AS DESTINO,
    CAST(PCCARREG.KMINICIAL AS VARCHAR(10)) AS KMINICIAL,
    CAST(PCCARREG.KMFINAL AS VARCHAR(10)) AS KMFIM,
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
**MOTORISTA** : *O campo deve ser **VARCHAR(100)**, contendo o nome do motorista.*<br/>
**AJUDANTE1** : *O campo deve ser **VARCHAR(100)**, contendo o nome do ajudante.*<br/>
**AJUDANTE2** : *O campo deve ser **VARCHAR(100)**, contendo o nome do ajudante.*<br/>
**AJUDANTE3** : *O campo deve ser **VARCHAR(100)**, contendo o nome do ajudante.*<br/>
**DATAGERACAO** : *O campo deve ser **DATE**, trazendo a data de geração do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
**DATASAIDA** : *O campo deve ser **DATE**, trazendo a data de saida do carregamento.*<br/>
**DATACHEGADA** : *O campo deve ser **DATE**, trazendo a data de chegada do carregamento.*<br/>
**DESTINO** : *O campo deve ser **VARHCAR(20)**, trazendo o destino do carregamento.*<br/>
**KMINICIAL** : *O campo deve ser **VARHCAR(10)**, trazendo o destino do carregamento.*<br/>
**KMFIM** : *O campo deve ser **VARHCAR(10)**, trazendo o destino do carregamento.* <br/> 
**CODFILIAL** : *O campo deve ser **VARHCAR(30)**, trazendo o destino do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOPEDIDO** : *O campo deve ser **VARHCAR(30)**, trazendo o destino do carregamento.****<font color="red"> - obrigartorio</font>***<br/>
