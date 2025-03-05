# View EXPERT_NOTAITEM

Definição da view responsável pelo gerenciamento dos itens das Notas Fiscais no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_NOTAITEM ("ID", "CODIGONOTA", "ITEM", "CODIGOPRODUTO", "CODIGOFILIAL", "TIPO", "CODIGOFORNECEDOR", "QUANTIDADE", "VALUNITARIO", "VALIDADE", "FABRICACAO", "LOTE", "NUMSERIE", "EMBALAGEM") AS 
  select DISTINCT
  ROWNUM AS id,
  CAST(pcmov.numnota AS varchar(30)) codigonota,
  pcmov.SEQMOV item,
  CAST(pcmov.CODPROD AS varchar(30)) codigoproduto,
  CAST(pcnfent.CODFILIAL AS varchar(30)) codigofilial,
  case when pcmov.codoper = 'ED' then 2
    	 WHEN pcmov.CODOPER = 'ET' THEN 3
    else 1 
  end as tipo,
  CAST(case when pcmov.codoper = 'ED' then to_number('900'||to_char(pcnfent.CODFORNEC))
       else pcnfent.CODFORNEC
  END AS varchar(30)) codigofornecedor,
  CAST(sum(pcmov.qt) AS numeric(10,4)) quantidade,
  CAST(coalesce(pcmov.valorultent,0) AS numeric(10,4)) valunitario,
  pcmov.DATAVALIDADE validade,
  pcmov.DATAFABRICACAO fabricacao,
  pcmov.NUMLOTE lote,
  NULL numserie,
  NULL embalagem
  from pcmov
  Join pcnfent on pcmov.numtransent=pcnfent.numtransent and pcmov.numnota=pcnfent.NUMNOTA
  where ((pcmov.codoper='E') or (pcmov.codoper='EB') or (pcmov.codoper='ET') or (pcmov.codoper='EF') or (pcmov.codoper='ED'))
    and (pcnfent.especie <> 'OE')  and pcnfent.dtent >= to_date('01/10/2024','dd/mm/yyyy')
  GROUP BY ROWNUM , pcmov.SEQMOV, pcmov.numnota, pcnfent.CODFILIAL,  pcnfent.CODFORNEC, pcmov.CODPROD, pcmov.valorultent,
  pcmov.DATAVALIDADE, pcmov.DATAFABRICACAO, pcmov.NUMLOTE, pcmov.codoper

```
**ID** : *O campo deve ser **INTEIRO**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGONOTA** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da nota.****<font color="red"> - obrigartorio</font>***<br/>
**ITEM** : *O campo deve ser **VARCHAR(50)**, contendo o item.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOPRODUTO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da produto.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOFILIAL** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da filial.****<font color="red"> - obrigartorio</font>***<br/>
**TIPO** : *O campo deve ser **INTEIRO**, trazendo o tipo.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOFORNECEDOR** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do fornecedor.****<font color="red"> - obrigartorio</font>***<br/>
**QUANTIDADE** : *O campo deve ser **NUMERIC(10,4)**, contendo a quantidade.****<font color="red"> - obrigartorio</font>***<br/>
**VALUNITARIO** : *O campo deve ser **NUMERIC(10,4)**, contendo o valunitario.****<font color="red"> - obrigartorio</font>***<br/>
**VALIDADE** : *O campo deve ser **DATE**, contendo a validade.*<br/>
**FABRICACAO** : *O campo deve ser **DATE**, contendo a fabricacao.*<br/>
**LOTE** : *O campo deve ser **VARCHAR(50)**, contendo o lote.*<br/>
**NUMSERIE** : *O campo deve ser **VARCHAR(50)**, contendo o numserie.*<br/>
**EMBALAGEM** : *O campo deve ser **VARCHAR(20)**, contendo o embalagem.*<br/>
