# View EXPERT_PEDIDOSITEM

Definição da view responsável pelo gerenciamento dos itens dos Pedidos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PEDIDOSITEM ("ID", "CODIGOPEDIDO", "ITEMPEDIDO", "CODIGOFILIAL", "CODIGOPRODUTO", "QUANTIDADE", "QTDSEPARADA", "QTDCONFERIDA", "QTDCORTADA") AS 
  SELECT 
  ROWNUM AS id,
  CAST(PCPEDI.NUMPED AS varchar(30)) codigopedido,
  CAST(PCPEDI.NUMSEQ AS varchar(30)) itempedido,  
  CAST(pcpedc.CODFILIAL  AS varchar(30)) codigofilial,
  CAST(PCPEDI.CODPROD  AS varchar(30)) codigoproduto,
  CAST(PCPEDI.QT AS numeric(10,4)) quantidade,
  CAST(0 AS numeric(10,4)) qtdseparada,
  CAST(0 as numeric(10,4)) qtdconferida,
  CAST(0 as numeric(10,4)) qtdcortada
 FROM PCPEDI 
 JOIN PCPEDC ON PCPEDC.NUMPED = PCPEDI.NUMPED
 WHERE pcpedc.LOCALIZACAOPEDIDO is NULL
   -- AND pcpedc.DTEMISSAOMAPA IS NOT NULL
    AND pcpedc."DATA" >= (sysdate - 90)
    AND PCPEDC.CODCLI <> 3;

```
**ID** : *O campo deve ser **INTEIRO**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do pedido.****<font color="red"> - obrigartorio</font>***<br/>
**ITEMPEDIDO** : *O campo deve ser **VARCHAR(50)**, contendo o item.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOFILIAL** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da filial.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOPRODUTO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da produto.****<font color="red"> - obrigartorio</font>***<br/>
**QUANTIDADE** : *O campo deve ser **NUMERIC(10,4)**, contendo a quantidade.****<font color="red"> - obrigartorio</font>***<br/>
**QTDSEPARADA** : *O campo deve ser **NUMERIC(10,4)**, contendo a qtdseparada.*<br/>
**QTDCONFERIDA** : *O campo deve ser **NUMERIC(10,4)**, contendo a qtdconferida.*<br/>
**QTDCORTADA** : *O campo deve ser **NUMERIC(10,4)**, contendo a qtdcortada.*<br/>
