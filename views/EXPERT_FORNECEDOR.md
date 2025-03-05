Definição da view responsável pelo gerenciamento dos -- no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_FORNECEDOR ("CODIGO", "NOME", "NOMEFANTASIA", "CPF", "IE", "ESTADO", "CODCIDADE", "BAIRRO", "ENDERECO", "NUMERO", "TELEFONE") AS 
  SELECT
   CAST (pcfornec.codfornec AS VARCHAR(255)) codigo,
   pcfornec.fornecedor nome,
   pcfornec.fantasia nomefantasia,
   pcfornec.cgc cpf,
   pcfornec.IE ie,
   pcfornec.estado estado,
   NULL codcidade,
   pcfornec.bairro bairro,
   pcfornec.ender endereco,
   null numero,
   NULL telefone
  from pcfornec
  WHERE pcfornec.dtexclusao IS NULL
UNION
SELECT
    CAST (to_number('900' || to_char(pcclient.codcli)) AS varchar(255))codigo,
    pcclient.cliente nome,
    pcclient.FANTASIA nomefantasia,
    pcclient.cgcent cpf,
    pcclient.IEENT ie ,
    pccidade.UF estado,
    pcclient.codmunicipio codcidade,
    pcclient.bairroent bairro,
    pcclient.enderent endereco,
    pcclient.NUMEROENT numero,
    pcclient.TELCOB telefone
  FROM pcclient
  left join pcpraca on pcpraca.codpraca = pcclient.codpraca
  left join PCROTAEXP on PCROTAEXP.codrota = pcpraca.rota
  LEFT JOIN PCCIDADE ON pcclient.CODCIDADE = PCCIDADE.CODCIDADE
  where pcclient.dtexclusao is null

```
**CODIGO** : *O campo deve ser **VARCHAR(30)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**NOME** : *O campo deve ser **VARCHAR(100)**, contendo o nome do fornecedor.****<font color="red"> - obrigartorio</font>***<br/>
**NOMEFANTASIA** : *O campo deve ser **VARCHAR(100)**, contendo o nome fantasia do fornecedor.****<font color="red"> - obrigartorio</font>***<br/>
**CPF** : *O campo deve ser **VARCHAR(14)**, contem o cpf.****<font color="red"> - obrigartorio</font>***<br/>
**IE** : *O campo deve ser **VARCHAR(10)**, contem o ie.*<br/>
**ESTADO** : *O campo deve ser **VARCHAR(2)**, contem o estado.*<br/>
**CODCIDADE** : *O campo deve ser **INTEIRO**, contem o codigo da cidade.*<br/>
**BAIRRO** : *O campo deve ser **VARCHAR(100)**, contendo o bairro.*<br/>
**ENDERECO** : *O campo deve ser **VARCHAR(150)**, contendo o endereço.*<br/>
**NUMERO** : *O campo deve ser **VARCHAR(10)**, contendo o numero do endereço.*<br/>
**TELEFONE** : *O campo deve ser **VARCHAR(12)**, contendo o telefone.*<br/>
