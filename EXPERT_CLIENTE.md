# View EXPERT_CLIENTE

Definição da view responsável pelo gerenciamento dos clientes no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_CLIENTE ("CODIGO", "CPFCNPJ", "NOME", "CODCIDADE", "NOMECIDADE", "ENDERECO", "COMPLEMENTO", "NUMERO", "BAIRRO", "CEP", "TELEFONE", "EMAIL", "ATIVO", "LATITUDE", "LONGETUDE", "SHELFLIFE", "CODROTA", "NOMEROTA") AS 
  SELECT
    CAST(pcclient.codcli AS VARCHAR(255)) codigo,
    pcclient.cgcent cpfcnpj,
    pcclient.cliente nome,
    pcclient.codmunicipio codcidade,
    PCCIDADE.NOMECIDADE nomecidade,
    pcclient.enderent endereco,
    pcclient.complementoent complemento,
    pcclient.NUMEROENT numero,
    pcclient.bairroent bairro,
    pcclient.cepent cep,
    pcclient.TELCOB telefone,
    pcclient.EMAIL email,
    1 ativo,
    NULL latitude,
    NULL longetude,
    pcclient.QTDIASAVENCERPRODUTO shelflife ,
    PCROTAEXP.codrota as CodRota,
    PCROTAEXP.descricao as NomeRota
  FROM pcclient
  left join pcpraca on pcpraca.codpraca = pcclient.codpraca
  left join PCROTAEXP on PCROTAEXP.codrota = pcpraca.rota
  LEFT JOIN PCCIDADE ON pcclient.CODCIDADE = PCCIDADE.CODCIDADE
  where pcclient.dtexclusao is null
```

**CODIGO** : *O campo deve ser **VARCHAR(255)**, o mesmo e a chave primaria.****<font color="red"> - obrigartorio</font>***<br/>
**CPFCNPJ** : *O campo deve ser **VARCHAR(18)**, contendo os dados de cpf ou cnpj do cliente.****<font color="red"> - obrigartorio</font>***<br/>
**NOME** : *O campo deve ser **VARCHAR(60)**, contendo o nome do cliente.****<font color="red"> - obrigartorio</font>***<br/>
**CODCIDADE** : *O campo deve ser **NUMERIC(10,0)**, contendo o codigo da cidade.*<br/>
**NOMECIDADE** : *O campo deve ser **VARCHAR(80)**, contendo o nome da cidade.*<br/>
**ENDERECO** : *O campo deve ser **VARCHAR(40)**, contendo o endereço.*<br/>
**COMPLEMENTO** : *O campo deve ser **VARCHAR(80)**, contendo o complemento do endereço.*<br/>
**NUMERO** : *O campo deve ser **VARCHAR(6)**, contendo o numero do endereço.*<br/>
**BAIRRO** : *O campo deve ser **VARCHAR(40)**, contendo o bairro.*<br/>
**CEP** : *O campo deve ser **VARCHAR(9)**, contendo o cep.*<br/>
**TELEFONE** : *O campo deve ser **VARCHAR(13)**, contendo o telefone.*<br/>
**EMAIL** : *O campo deve ser **VARCHAR(100)**, contendo o email.*<br/>
**ATIVO** : *O campo deve ser **INTEIRO**, trazendo dados se e ativo.*<br/>
**LATITUDE** : *O campo deve ser **VARCHAR(100)**, contendo a latitude.*<br/>
**LONGETUDE** : *O campo deve ser **VARCHAR(100)**, contendo a longetude.*<br/>
**SHELFLIFE** : *O campo deve ser **INTEIRO**, contendo a shelflife.*<br/> 
**CODROTA** : *O campo deve ser **VARCHAR(30)**, contendo o codigo da rota.*<br/> 
**NOMEROTA** : *O campo deve ser **VARCHAR(100)**, contendo o nome da rota.*<br/> 
