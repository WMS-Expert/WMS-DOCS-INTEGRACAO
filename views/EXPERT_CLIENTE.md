# View EXPERT_CLIENTE

Definição da view responsável pelo gerenciamento dos clientes no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_CLIENTE (
  "CODIGO", 
  "CPFCNPJ", 
  "NOME", 
  "CODCIDADE", 
  "NOMECIDADE", 
  "ENDERECO", 
  "COMPLEMENTO", 
  "NUMERO", 
  "BAIRRO", 
  "CEP", 
  "TELEFONE", 
  "EMAIL", 
  "ATIVO", 
  "LATITUDE", 
  "LONGITUDE", 
  "SHELFLIFE", 
  "CODROTA", 
  "NOMEROTA"
) AS 
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
  NULL longitude,
  pcclient.QTDIASAVENCERPRODUTO shelflife,
  PCROTAEXP.codrota codrota,
  PCROTAEXP.descricao nomerota
FROM pcclient
LEFT JOIN pcpraca ON pcpraca.codpraca = pcclient.codpraca
LEFT JOIN PCROTAEXP ON PCROTAEXP.codrota = pcpraca.rota
LEFT JOIN PCCIDADE ON pcclient.CODCIDADE = PCCIDADE.CODCIDADE
WHERE pcclient.dtexclusao IS NULL;
```

Exemplo de json:

```json
{
  "CODIGO": "CLT12345",
  "CPFCNPJ": "12345678901234",
  "NOME": "Cliente Teste Ltda",
  "CODCIDADE": 3543402,
  "NOMECIDADE": "São Paulo",
  "ENDERECO": "Rua das Américas",
  "COMPLEMENTO": "Sala 5",
  "NUMERO": "100",
  "BAIRRO": "Centro",
  "CEP": "01001000",
  "TELEFONE": "11999998888",
  "EMAIL": "cliente@teste.com",
  "ATIVO": 1,
  "LATITUDE": null,
  "LONGITUDE": null,
  "SHELFLIFE": 60,
  "CODROTA": "R001",
  "NOMEROTA": "Rota Principal"
}
```

| Campo           | Tipo            | Descrição                                                 |
| --------------- | --------------- | --------------------------------------------------------- |
| **CODIGO**      | `VARCHAR(30)`   | Código identificador do cliente. 🔴 **Obrigatório**. |
| **CPFCNPJ**     | `VARCHAR(14)`   | CPF ou CNPJ do cliente. 🔴 **Obrigatório**.          |
| **NOME**        | `VARCHAR(100)`  | Nome ou razão social do cliente. 🔴 **Obrigatório**. |
| **CODCIDADE**   | `NUMERIC(10,0)` | Código da cidade.                                         |
| **NOMECIDADE**  | `VARCHAR(100)`  | Nome da cidade.                                           |
| **ENDERECO**    | `VARCHAR(150)`  | Endereço principal.                                       |
| **COMPLEMENTO** | `VARCHAR(150)`  | Complemento do endereço.                                  |
| **NUMERO**      | `VARCHAR(20)`   | Número do endereço.                                       |
| **BAIRRO**      | `VARCHAR(120)`  | Bairro.                                                   |
| **CEP**         | `VARCHAR(8)`    | Código de endereçamento postal (CEP).                     |
| **TELEFONE**    | `VARCHAR(12)`   | Telefone de contato. 🔴 **Obrigatório**.                                    |
| **EMAIL**       | `VARCHAR(100)`  | Endereço de e-mail.                                       |
| **ATIVO**       | `INTEGER`       | Indicador de cliente ativo (1 = ativo).                   |
| **LATITUDE**    | `VARCHAR(100)`  | Latitude geográfica (atualmente `NULL`).                  |
| **LONGITUDE**   | `VARCHAR(100)`  | Longitude geográfica (atualmente `NULL`).                 |
| **SHELFLIFE**   | `INTEGER`       | Prazo de validade sugerido em dias para produtos.         |
| **CODROTA**     | `VARCHAR(30)`   | Código da rota de entrega.                                |
| **NOMEROTA**    | `VARCHAR(100)`  | Descrição da rota de entrega.                             |

