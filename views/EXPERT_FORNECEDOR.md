# View EXPERT_FORNECEDOR

Definição da view responsável pelo gerenciamento dos -- no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_FORNECEDOR (
  "CODIGO",
  "NOME",
  "NOMEFANTASIA",
  "CPF",
  "IE",
  "ESTADO",
  "CODCIDADE",
  "BAIRRO",
  "ENDERECO",
  "NUMERO",
  "TELEFONE"
) AS 
  SELECT
    CAST(pcfornec.codfornec AS VARCHAR(255)) codigo,
    pcfornec.fornecedor nome,
    pcfornec.fantasia nomefantasia,
    pcfornec.cgc cpf,
    pcfornec.IE ie,
    pcfornec.estado estado,
    NULL codcidade,
    pcfornec.bairro bairro,
    pcfornec.ender endereco,
    NULL numero,
    NULL telefone
  FROM pcfornec
  WHERE pcfornec.dtexclusao IS NULL

UNION

  SELECT
    CAST(TO_NUMBER('900' || TO_CHAR(pcclient.codcli)) AS VARCHAR(255)) codigo,
    pcclient.cliente nome,
    pcclient.fantasia nomefantasia,
    pcclient.cgcent cpf,
    pcclient.IEENT ie,
    pccidade.UF estado,
    pcclient.codmunicipio codcidade,
    pcclient.bairroent bairro,
    pcclient.enderent endereco,
    pcclient.numeroent numero,
    pcclient.telcob telefone
  FROM pcclient
  LEFT JOIN pcpraca ON pcpraca.codpraca = pcclient.codpraca
  LEFT JOIN PCROTAEXP ON PCROTAEXP.codrota = pcpraca.rota
  LEFT JOIN PCCIDADE ON pcclient.codcidade = PCCIDADE.codcidade
  WHERE pcclient.dtexclusao IS NULL;


```

Exemplo de json:

```json
{
  "CODIGO": "900123",
  "NOME": "Fornecedor Exemplo LTDA",
  "NOMEFANTASIA": "Fornecedor XP",
  "CPF": "12345678901234",
  "IE": "1234567890",
  "ESTADO": "SP",
  "CODCIDADE": 3550308,
  "BAIRRO": "Centro",
  "ENDERECO": "Av. das Nações Unidas",
  "NUMERO": "1500",
  "TELEFONE": "11999998888"
}

```
| Campo            | Tipo           | Descrição                                            |
| ---------------- | -------------- | ---------------------------------------------------- |
| **CODIGO**       | `VARCHAR(30)`  | Código do fornecedor.🔴 **Obrigatório**.             |
| **NOME**         | `VARCHAR(100)` | Nome do fornecedor.🔴 **Obrigatório**.               |
| **NOMEFANTASIA** | `VARCHAR(100)` | Nome fantasia do fornecedor.🔴 **Obrigatório**.      |
| **CPF**          | `VARCHAR(14)`  | CPF ou CNPJ do fornecedor.🔴 **Obrigatório**.        |
| **IE**           | `VARCHAR(10)`  | Inscrição estadual.                                  |
| **ESTADO**       | `VARCHAR(2)`   | Unidade federativa (UF).                             |
| **CODCIDADE**    | `INTEGER`      | Código da cidade.                                    |
| **BAIRRO**       | `VARCHAR(100)` | Bairro do endereço.                                  |
| **ENDERECO**     | `VARCHAR(150)` | Endereço do fornecedor.                              |
| **NUMERO**       | `VARCHAR(10)`  | Número do endereço.                                  |
| **TELEFONE**     | `VARCHAR(12)`  | Telefone de contato.                                 |

