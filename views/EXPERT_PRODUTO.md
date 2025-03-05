# View EXPERT_PRODUTO

Definição da view responsável pelo gerenciamento dos Produtos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PRODUTO ("CODIGO", "CODIGOREF", "NOME", "UNIDADEPADRAO", "CODIGOGRUPO", "CODIGOFORNECEDOR", "ESTOQUEMINIMO", "ESTOQUEMAXIMO", "CONTROLAVALIDADE", "CONTROLALOTE", "CONTROLANUMSERIE", "PESOBRUTO", "PESOLIQUIDO", "SHELFLIFE", "LASTRO", "ALTURA", "QTDCX", "QTDPL", "CHECKOUT", "SEPCX", "SEPPL") AS 
  SELECT
    CAST (pcprodut.codprod AS VARCHAR(255)) codigo, 
    CAST (pcprodut.codprod AS VARCHAR(255)) codigoref,
    (pcprodut.descricao || ' ' || pcmarca.marca) nome,
    NULL unidadepadrao,
    CAST (pcprodut.codepto AS VARCHAR(255)) codigogrupo,
    CAST (pcprodut.CODFORNEC AS VARCHAR(255)) codigofornecedor,
    NULL estoqueminimo,
    NULL estoquemaximo,
    CASE WHEN COALESCE(PCPRODUT.ESTOQUEPORDTVALIDADE,'N') = 'N' THEN 0 ELSE 1 END controlavalidade,
    CASE WHEN COALESCE(PCPRODUT.ESTOQUEPORLOTE ,'N') = 'N' THEN 0 ELSE 1 END controlalote,
    0 controlanumserie,
    pcprodut.PESOBRUTO  pesobruto,
    pcprodut.PESOLIQ  pesoliquido,
    pcprodut.NUMDIASVALIDADEMIN shelflife,
    pcprodut.LASTROPAL lastro,
    pcprodut.ALTURAPAL altura,
    pcprodut.QTUNITCX qtdcx,
    pcprodut.QTTOTPAL qtdpl,
    0 checkout,
    0 sepcx,
    0 seppl
  FROM pcprodut 
  left join pcmarca on pcmarca.codmarca = pcprodut.codmarca

```
**CODIGO** : *O campo deve ser **VARCHAR(30)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOREF** : *O campo deve ser **VARCHAR(30)**, contendo o codigo ref.*<br/>
**NOME** : *O campo deve ser **VARCHAR(100)**, contendo o nome.****<font color="red"> - obrigartorio</font>***<br/>
**UNIDADEPADRAO** : *O campo deve ser **VARCHAR(100)**, contendo o unidade padrão.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOGRUPO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do grupo.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOFORNECEDOR** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do fornecedor.****<font color="red"> - obrigartorio</font>***<br/>
**ESTOQUEMINIMO** : *O campo deve ser **NUMERIC(10,4)**, contendo o estoque minimo.*<br/>
**ESTOQUEMAXIMO** : *O campo deve ser **NUMERIC(10,4)**, contendo o estoque maximo.*<br/>
**CONTROLAVALIDADE** : *O campo deve ser **INTEGER**, trazendo se controla ou não validade.*<br/>
**CONTROLALOTE** : *O campo deve ser **INTEGER**, trazendo se controla ou não lote.*<br>
**CONTROLANUMSERIE** : *O campo deve ser **INTEGER**, trazendo se controla ou não numserie.*<br/>
**PESOBRUTO** : *O campo deve ser **NUMERIC(12,6)**, contendo o peso bruto.*<br/>
**PESOLIQUIDO** : *O campo deve ser **NUMERIC(12,6)**, contendo o peso liquido.*<br/>
**SHELFLIFE** : *O campo deve ser **INTEIRO**, contendo a shelflife.*<br/> 
**LASTRO** : *O campo deve ser **NUMERIC(10,4)**, contendo a lastro.*<br/>
**ALTURA** : *O campo deve ser **NUMERIC(10,4)**, contendo a alturaa.*<br/>
**QTDCX** : *O campo deve ser **NUMERIC(8,2)**, contendo a qtdcx.*<br/>
**QTDPL** : *O campo deve ser **NUMERIC(8,2)**, contendo a qtdpl.*<br/>
**CHECKOUT** : *O campo deve ser **INTEIRO**, trazendo checkout.*<br/>
**SEPCX** : *O campo deve ser **INTEIRO**, trazendo se separa caixa.*<br/>
**SEPPL** : *O campo deve ser **INTEIRO**, trazendo se separa palete.*<br/>
