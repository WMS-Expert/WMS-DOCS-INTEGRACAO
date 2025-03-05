# View EXPERT_PEDIDO

Definição da view responsável pelo gerenciamento dos Pedidos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PEDIDO ("ID", "CODIGO", "VENDEDOR", "TIPO", "DATAEMISSAO", "DATAIMPORTACAO", "CODIGOCLIENTE", "CODFILIAL", "TOTALPEDIDO", "QTDITENS", "ORDEMPEDIDO", "STATUS", "DTEXPORTACAO", "CODIGOREF", "OBS", "CODIGOCARREGAMENTO", "DESCRICAOCONSUMIDOR") AS 
  SELECT 
  	ROWNUM AS id,
    CAST(pcpedc.numped AS varchar(30)) codigo,
    'Teste' vendedor,
    1 tipo,
    Coalesce(pcpedc.DTEMISSAOMAPA, pcpedc.Data) dataemissao,
    Coalesce(pcpedc.DTEMISSAOMAPA, pcpedc.Data) dataimportacao,
    CAST(pcpedc.codcli AS varchar(30)) codigocliente,
    CAST(pcpedc.codfilial AS varchar(30)) AS codigofilial,
    CAST(pcpedc.vltotal AS numeric(10,4)) totalpedido,
    (SELECT count(CODPROD) FROM PCPEDI WHERE pcpedi.numped = pcpedc.numped) qtditens,
    PCPEDC.NUMSEQMONTAGEM ordempedido,
    1 status,
    NULL dtexportacao,
    CAST(pcpedc.numped AS varchar(30)) codigoref,
    CASE 
    	WHEN (pcpedc.OBS1 IS NULL OR pcpedc.OBS1 = '') AND (pcpedc.OBS IS NULL OR pcpedc.OBS = '') AND (pcpedc.OBS2 IS NULL OR pcpedc.OBS2 = '') THEN NULL
    	WHEN (pcpedc.OBS1 IS NOT NULL ) AND (pcpedc.OBS IS NOT NULL ) AND (pcpedc.OBS2 IS NULL OR pcpedc.OBS2 = '') THEN 'OBS01 - ' || pcpedc.OBS || ' | OBS2 - ' || pcpedc.OBS1
    	WHEN (pcpedc.OBS1 IS NULL OR pcpedc.OBS1 = '' ) AND (pcpedc.OBS IS NOT NULL ) AND (pcpedc.OBS2 IS NOT NULL ) THEN 'OBS01 - ' || pcpedc.OBS || ' | OBS3 - ' || pcpedc.OBS2
    	WHEN (pcpedc.OBS1 IS NOT NULL ) AND (pcpedc.OBS IS NULL OR pcpedc.OBS = '' ) AND (pcpedc.OBS2 IS NOT NULL ) THEN 'OBS02 - ' || pcpedc.OBS1 || ' | OBS3 - ' || pcpedc.OBS2
    	WHEN (pcpedc.OBS1 IS NULL OR pcpedc.OBS1 = '') AND (pcpedc.OBS IS NOT NULL ) AND (pcpedc.OBS2 IS NULL OR pcpedc.OBS2 = '') THEN 'OBS01 - ' || pcpedc.OBS
    	WHEN (pcpedc.OBS1 IS NOT NULL ) AND (pcpedc.OBS IS NULL OR pcpedc.OBS = '') AND (pcpedc.OBS2 IS NULL OR pcpedc.OBS2 = '') THEN 'OBS02 - ' || pcpedc.OBS1
    	WHEN (pcpedc.OBS1 IS NULL OR pcpedc.OBS1 = '') AND (pcpedc.OBS IS NULL OR pcpedc.OBS = '') AND (pcpedc.OBS2 IS NOT NULL ) THEN 'OBS03 - ' || pcpedc.OBS2
    ELSE 'OBS01 - ' || pcpedc.OBS || ' | OBS2 - ' || pcpedc.OBS1 || ' | OBS3 - ' || pcpedc.OBS2 END AS OBS,
    CAST(pcpedc.numcar AS VARCHAR(30)) AS codigocarregamento,
    PCCLIENT.CLIENT as descricaoconsumidor
  from pcpedc
  left join PCCLIENT on PCCLIENT.codcli = pcpedc.CODCLI
  left join PCPRACA on PCPRACA.codpraca = PCCLIENT.codpraca
  left join PCROTAEXP on PCROTAEXP.CODROTA = pcpraca.rota
  where (pcpedc.posicao ='L' or pcpedc.posicao ='M')
    and pcpedc.LOCALIZACAOPEDIDO is NULL
   -- AND pcpedc.DTEMISSAOMAPA IS NOT NULL
    AND pcpedc."DATA" >= (sysdate - 90)
    AND PCPEDC.CODCLI <> 3
    --and PCPEDC.ORIGEMPED = 'R'
    --and Coalesce(pcpedc.DTEMISSAOMAPA, pcpedc.Data) >= to_date('20240111', 'yyyymmdd')

```
**ID** : *O campo deve ser **INTEIRO**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do pedido.****<font color="red"> - obrigartorio</font>***<br/>
**VENDEDOR** : *O campo deve ser **VARCHAR(100)**, contendo o nome do vendedor.****<font color="red"> - obrigartorio</font>***<br/>
**TIPO** : *O campo deve ser **INTEIRO**, trazendo o tipo.****<font color="red"> - obrigartorio</font>***<br/>
**DATAEMISSAO** : *O campo deve ser **DATE**, trazendo a data de importação.*<br/>
**DATAIMPORTACAO** : *O campo deve ser **DATE**, trazendo a data de importação.*<br/>
**CODIGOCLIENTE** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do cliente.****<font color="red"> - obrigartorio</font>***<br/>
**CODFILIAL** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da filial.****<font color="red"> - obrigartorio</font>***<br/>
**TOTALPEDIDO** : *O campo deve ser **NUMERIC(10,4)**, contendo o total do pedido.****<font color="red"> - obrigartorio</font>***<br/>
**QTDITENS** : *O campo deve ser **INTEGER**, contendo o quantidade de itens.*<br/>
**ORDEMPEDIDO** : *O campo deve ser **INTEGER**, contendo a ordem do pedido.*<br/>
**SITUACAO** : *O campo deve ser **INTEIRO**, contendo a situação da nota.*<br/>
**DTEXPORTACAO** : *O campo deve ser **DATE**, trazendo a data de exportação.*<br/>
**CODIGOREF** : *O campo deve ser **VARCHAR(30)**, contendo o codigoref.*<br/>
**OBS** : *O campo deve ser **VARCHAR(30)**, contendo uma obs.*<br/>
**CODIGOCARREGAMENTO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do carregamento.****<font color="red"> - 
obrigartorio</font>***<br/>
**DESCRICAOCONSUMIDOR** : *O campo deve ser **VARCHAR(150)**, contendo o nome do consumidor.****
