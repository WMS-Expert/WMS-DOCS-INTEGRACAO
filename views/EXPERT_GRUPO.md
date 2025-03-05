# View EXPERT_GRUPO

Definição da view responsável pelo gerenciamento de Grupos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_GRUPO ("CODIGO", "NOME", "ATIVO") AS 
  SELECT
   CAST (pcdepto.codepto AS VARCHAR(255)) codigo,
   pcdepto.descricao nome,
   1 ativo
  from pcdepto
  ;

```
**CODIGO** : *O campo deve ser **VARCHAR(255)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**NOME** : *O campo deve ser **VARCHAR(25)**, contendo o nome do grupo.****<font color="red"> - obrigartorio</font>***<br/>
**ATIVO** : *O campo deve ser **INTEIRO**, trazendo se o grupo esta ativo.*<br/>
