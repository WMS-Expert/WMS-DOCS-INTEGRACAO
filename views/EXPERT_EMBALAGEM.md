# View EXPERT_EMBALAGEM

Definição da view responsável pelo gerenciamento de Embalagens no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_EMBALAGEM ("ID", "CODIGO", "CODIGOPRODUTO", "CODBARRA", "QUANTIDADE", "ATIVO", "TIPO", "LARGURA", "ALTURA", "PROFUNDIDADE", "M3", "PESOBRUTO", "PESOLIQUIDO")
AS SELECT 
  DISTINCT "ID", "CODIGO", "CODIGOPRODUTO", "CODBARRA", "QUANTIDADE", "ATIVO", "TIPO", "LARGURA", "ALTURA", "PROFUNDIDADE", "M3", "PESOBRUTO", "PESOLIQUIDO" 
FROM 
  (
    SELECT 
      DISTINCT ROWNUM AS id, 
      null AS codigo, 
      CAST(pcembalagem.CODPROD AS varchar(30)) AS codigoproduto, 
      CAST(codauxiliar AS varchar(20)) AS codbarra, 
      CAST(qtunit AS numeric(10, 4)) AS quantidade, 
      1 AS ativo, 
      CASE WHEN qtunit = 1 THEN 1 ELSE 2 END AS tipo, 
      CAST(pcembalagem.LARGURA AS numeric(10, 4)) AS largura, 
      CAST(pcembalagem.ALTURA AS numeric(10, 4)) AS altura, 
      CAST(pcembalagem.COMPRIMENTO AS numeric(10, 4)) AS profundidade, 
      CAST((pcembalagem.LARGURA * pcembalagem.ALTURA * pcembalagem.COMPRIMENTO) AS numeric(10, 4)) AS m3, 
      CAST(pcembalagem.PESOBRUTO AS numeric(10, 4)) AS pesobruto, 
      CAST(pcembalagem.PESOLIQ AS numeric(10, 4)) AS pesoliquido 
    FROM 
      pcembalagem 
    WHERE 
      codauxiliar IS NOT null
  );

```
**ID** : *O campo deve ser **INTEIRO**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da embalagem.****<font color="red"> - obrigartorio</font>***<br/>
**CODIGOPRODUTO** : *O campo deve ser **VARCHAR(30)**, contendo o codigo do produto.****<font color="red"> - obrigartorio</font>***<br/>
**CODBARRA** : *O campo deve ser **VARCHAR(20)**, contendo o codigo de barra.****<font color="red"> - obrigartorio</font>***<br/>
**QUANTIDADE** : *O campo deve ser **NUMERIC(10,4)**, contendo a quantidade.****<font color="red"> - obrigartorio</font>***<br/>
**ATIVO** : *O campo deve ser **INTEIRO**, trazendo se a embalagem esta ativo.****<font color="red"> - obrigartorio</font>***<br/>
**TIPO** : *O campo deve ser **INTEIRO**, trazendo o tipo.****<font color="red"> - obrigartorio</font>***<br/>
**LARGURA** : *O campo deve ser **NUMERIC(10,4)**, contendo a largura.*<br/>
**ALTURA** : *O campo deve ser **NUMERIC(10,4)**, contendo a Altura.*<br/>
**PROFUNDIDADE** : *O campo deve ser **NUMERIC(10,4)**, contendo a Profundidade.*<br/>
**M3** : *O campo deve ser **NUMERIC(10,4)**, contendo a M3.*<br/>
**PESOBRUTO** : *O campo deve ser **NUMERIC(10,4)**, contendo a Peso Bruto.*<br/>
**PESOLIQUIDO** : *O campo deve ser **NUMERIC(10,4)**, contendo a Peso Liquido.*<br/>
