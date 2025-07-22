# View EXPERT_PRODUTO

Definição da view responsável pelo gerenciamento dos Produtos no sistema.  

## Código SQL

```sql
CREATE VIEW EXPERT_PRODUTO (
  "CODIGO",
  "CODIGOREF",
  "NOME",
  "UNIDADEPADRAO",
  "CODIGOGRUPO",
  "CODIGOFORNECEDOR",
  "ESTOQUEMINIMO",
  "ESTOQUEMAXIMO",
  "CONTROLAVALIDADE",
  "CONTROLALOTE",
  "CONTROLANUMSERIE",
  "PESOBRUTO",
  "PESOLIQUIDO",
  "SHELFLIFE",
  "LASTRO",
  "ALTURA",
  "QTDCX",
  "QTDPL",
  "CHECKOUT",
  "SEPCX",
  "SEPPL"
) AS 
SELECT
  CAST(pcprodut.codprod AS VARCHAR(255)) codigo, 
  CAST(pcprodut.codprod AS VARCHAR(255)) codigoref,
  (pcprodut.descricao || ' ' || pcmarca.marca) nome,
  NULL unidadepadrao,
  CAST(pcprodut.codepto AS VARCHAR(255)) codigogrupo,
  CAST(pcprodut.CODFORNEC AS VARCHAR(255)) codigofornecedor,
  NULL estoqueminimo,
  NULL estoquemaximo,
  CASE WHEN COALESCE(pcprodut.ESTOQUEPORDTVALIDADE,'N') = 'N' THEN 0 ELSE 1 END controlavalidade,
  CASE WHEN COALESCE(pcprodut.ESTOQUEPORLOTE,'N') = 'N' THEN 0 ELSE 1 END controlalote,
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
LEFT JOIN pcmarca ON pcmarca.codmarca = pcprodut.codmarca;


```


Exemplo de json:

```json
{
  "CODIGO": "12345",
  "CODIGOREF": "12345",
  "NOME": "Arroz Integral Camil 1kg",
  "UNIDADEPADRAO": null,
  "CODIGOGRUPO": "001",
  "CODIGOFORNECEDOR": "500",
  "ESTOQUEMINIMO": null,
  "ESTOQUEMAXIMO": null,
  "CONTROLAVALIDADE": 1,
  "CONTROLALOTE": 1,
  "CONTROLANUMSERIE": 0,
  "PESOBRUTO": 1.200000,
  "PESOLIQUIDO": 1.000000,
  "SHELFLIFE": 180,
  "LASTRO": 10.00,
  "ALTURA": 1.80,
  "QTDCX": 12.00,
  "QTDPL": 120.00,
  "CHECKOUT": 0,
  "SEPCX": 0,
  "SEPPL": 0
}

```

| Campo                | Tipo            | Descrição                                          |
| -------------------- | --------------- | -------------------------------------------------- |
| **CODIGO**           | `VARCHAR(30)`   | Código do produto. 🔴 **Obrigatório**              |
| **CODIGOREF**        | `VARCHAR(30)`   | Código de referência (espelho do código).          |
| **NOME**             | `VARCHAR(100)`  | Nome do produto + marca. 🔴 **Obrigatório**        |
| **UNIDADEPADRAO**    | `VARCHAR(100)`  | Unidade padrão de medida. 🔴 **Obrigatório**       |
| **CODIGOGRUPO**      | `VARCHAR(30)`   | Código do grupo/departamento. 🔴 **Obrigatório**   |
| **CODIGOFORNECEDOR** | `VARCHAR(30)`   | Código do fornecedor principal. 🔴 **Obrigatório** |
| **ESTOQUEMINIMO**    | `NUMERIC(10,4)` | Quantidade mínima em estoque.                      |
| **ESTOQUEMAXIMO**    | `NUMERIC(10,4)` | Quantidade máxima em estoque.                      |
| **CONTROLAVALIDADE** | `INTEGER`       | 1 se controla validade, 0 caso contrário.          |
| **CONTROLALOTE**     | `INTEGER`       | 1 se controla lote, 0 caso contrário.              |
| **CONTROLANUMSERIE** | `INTEGER`       | 1 se controla número de série, 0 caso contrário.   |
| **PESOBRUTO**        | `NUMERIC(12,6)` | Peso bruto do produto.                             |
| **PESOLIQUIDO**      | `NUMERIC(12,6)` | Peso líquido do produto.                           |
| **SHELFLIFE**        | `INTEGER`       | Número mínimo de dias de validade (shelf life).    |
| **LASTRO**           | `NUMERIC(10,4)` | Quantidade por lastro na palete.                   |
| **ALTURA**           | `NUMERIC(10,4)` | Altura em palete.                                  |
| **QTDCX**            | `NUMERIC(8,2)`  | Quantidade por caixa.                              |
| **QTDPL**            | `NUMERIC(8,2)`  | Quantidade por palete.                             |
| **CHECKOUT**         | `INTEGER`       | 1 se possui checkout especial, 0 caso contrário.   |
| **SEPCX**            | `INTEGER`       | 1 se permite separação por caixa.                  |
| **SEPPL**            | `INTEGER`       | 1 se permite separação por palete.                 |

