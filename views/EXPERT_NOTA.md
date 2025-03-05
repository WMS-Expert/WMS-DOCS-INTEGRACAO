# View EXPERT_NOTA

Definição da view responsável pelo gerenciamento de Notas Fiscais no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_NOTA ("ID", "CODIGO", "CODFILIAL", "TIPO", "CARGA", "DATAEMISSAO", "QUANTIDADE", "NUMERONOTAFISCAL", "SERIE", "QUANTIDADEVOLUME", "VALTOTALPRODUTO", "VALTOTALNOTA", "PESOBRUTO", "PESOLIQUIDO", "SITUACAO", "QTDVOLUMECONF", "CODIGOFORNECEDOR") AS 
  select 
  	distinct 
  	CAST (pcnfent.NUMNOTA AS VARCHAR(30)) || CAST(pcnfent.CODFILIAL AS VARCHAR(30))
  	|| CAST (case when pcmov.codoper = 'ED' then to_number('900'||to_char(pcnfent.CODFORNEC))
    else pcnfent.CODFORNEC
    END AS VARCHAR(30)) || pcnfent.SERIE AS Id,
    CAST (pcnfent.NUMNOTA AS VARCHAR(30)) codigo,
    CAST(pcnfent.CODFILIAL AS VARCHAR(30)) codfilial,
    case when pcmov.codoper = 'ED' then 2
    	 WHEN pcmov.CODOPER = 'ET' THEN 3
    else 1 
    end as tipo,
    CAST(pcnfent.NUMBONUS AS varchar(20)) carga,
    pcnfent.dtent dataemissao,
    CAST (0 AS numeric(10,2)) quantidade,
    CAST (pcnfent.NUMNOTA AS VARCHAR(30)) numeronotafiscal,
    pcnfent.SERIE serie,
    pcnfent.NUMVOL quantidadevolume,
    pcnfent.VLTOTAL valtotalproduto,
    pcnfent.VLTOTAL valtotalnota,
    pcnfent.TOTPESO pesobruto,
    pcnfent.TOTPESOLIQ pesoliquido,
    1 situacao,
    CAST (0 AS numeric(10,2)) qtdvolumeconf,    
    CAST (case when pcmov.codoper = 'ED' then to_number('900'||to_char(pcnfent.CODFORNEC))
    else pcnfent.CODFORNEC
    END AS VARCHAR(30)) codigofornecedor  
  from pcnfent
  Join pcmov on pcmov.numtransent=pcnfent.numtransent and pcmov.numnota=pcnfent.NUMNOTA
  where ((pcmov.codoper='E') or (pcmov.codoper='EB') or (pcmov.codoper='ET')  or (pcmov.codoper='EF') or (pcmov.codoper='ED'))    and (pcnfent.especie <> 'OE')
  and pcnfent.dtent >= to_date('01/10/2024','dd/mm/yyyy') ;

```
**ID** : *O campo deve ser **INTEIRO**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da nota.****<font color="red"> - obrigartorio</font>***<br/>
**CODFILIAL** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da filial.****<font color="red"> - obrigartorio</font>***<br/>
**TIPO** : *O campo deve ser **INTEIRO**, trazendo o tipo.****<font color="red"> - obrigartorio</font>***<br/>
**CARGA** : *O campo deve ser **VARCHAR(20)**, contendo o codigo da carga.****<font color="red"> - obrigartorio</font>***<br/>
**DATAEMISSAO** : *O campo deve ser **DATE**, trazendo a data de geração.****<font color="red"> - obrigartorio</font>***<br/>
**QUANTIDADE** : *O campo deve ser **NUMERIC(10,4)**, contendo a quantidade.****<font color="red"> - obrigartorio</font>***<br/>
**NUMERONOTAFISCAL** : *O campo deve ser **VARCHAR(30)**, contendo o numero da nota fiscal.****<font color="red"> - obrigartorio</font>***<br/>
**SERIE** : *O campo deve ser **VARCHAR(3)**, contendo o numero da nota fiscal.****<font color="red"> - obrigartorio</font>***<br/>
**QUANTIDADEVOLUME** : *O campo deve ser **NUMERIC(10,2)**, contendo a quantidade volume.****<font color="red"> - obrigartorio</font>***<br/>
**VALTOTALPRODUTO** : *O campo deve ser **NUMERIC(12,2)**, contendo a valtotalproduto.****<font color="red"> - obrigartorio</font>***<br/>
**VALTOTALNOTA** : *O campo deve ser **NUMERIC(12,2)**, contendo a valtotalnota.****<font color="red"> - obrigartorio</font>***<br/>
**PESOBRUTO** : *O campo deve ser **NUMERIC(18,6)**, contendo a Peso Bruto.*<br/>
**PESOLIQUIDO** : *O campo deve ser **NUMERIC(18,6)**, contendo a Peso Liquido.*<br/>
**SITUACAO** : *O campo deve ser **INTEIRO**, contendo a situação da nota.*<br/>
**QTDVOLUMECONF** : *O campo deve ser **NUMERIC(10,2)**, contendo a quantidade volume conf.*<br/>
**CODIGOFORNECEDOR** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do fornecedor.****<font color="red"> - obrigartorio</font>***<br/>
