# View EXPERT_CARREGAMENTO

View responsável por representar os dados de carregamentos no sistema, integrando informações de pedidos e dados operacionais de carregamento.

## Código SQL

```sql
CREATE OR REPLACE VIEW EXPERT_CARREGAMENTO AS
 
SELECT
  CAST(PCPEDC.NUMCAR AS VARCHAR(255)) AS CODIGO,
  CAST(NULL AS VARCHAR(100)) AS MOTORISTA,
  CAST(PCCARREG.CODFUNCAJUD AS VARCHAR(100)) AS AJUDANTE1,
  CAST(PCCARREG.CODFUNCAJUD2 AS VARCHAR(100)) AS AJUDANTE2,
  CAST(PCCARREG.CODFUNCAJUD3 AS VARCHAR(100)) AS AJUDANTE3,
  CAST(PCCARREG.DATAMON AS DATE) AS DATAGERACAO,
  CAST(PCCARREG.DTSAIDA AS DATE) AS DATASAIDA,
  CAST(NULL AS DATE) AS DATACHEGADA,
  CAST(PCCARREG.DESTINO AS VARCHAR(20)) AS DESTINO,
  CAST(PCCARREG.KMINICIAL AS VARCHAR(10)) AS KMINICIAL,
  CAST(PCCARREG.KMFINAL AS VARCHAR(10)) AS KMFIM,
  CAST(PCPEDC.codfilial AS VARCHAR(30)) AS CODFILIAL,
  CAST(PCPEDC.NUMPED AS VARCHAR(30)) AS CODIGOPEDIDO
FROM
  PCPEDC
JOIN
  PCCARREG ON PCCARREG.numcar = PCPEDC.numcar
LEFT JOIN
  PCVEICUL ON PCVEICUL.CODVEICULO = PCCARREG.CODVEICULO
WHERE
  PCPEDC.codfilial = 1;

```

Exemplo de json:

```json
{
  "CODIGO": "100200",
  "MOTORISTA": null,
  "AJUDANTE1": "A123",
  "AJUDANTE2": "A456",
  "AJUDANTE3": "A789",
  "DATAGERACAO": "2024-10-05",
  "DATASAIDA": "2024-10-06",
  "DATACHEGADA": null,
  "DESTINO": "FILIAL02",
  "KMINICIAL": "1000",
  "KMFIM": "1250",
  "CODFILIAL": "1",
  "CODIGOPEDIDO": "PED789456"
}

```
| Campo            | Tipo           | Descrição                                                |
| ---------------- | -------------- | -------------------------------------------------------- |
| **CODIGO**       | `VARCHAR(255)` | Código do carregamento.🔴 **Obrigatório**.          |
| **MOTORISTA**    | `VARCHAR(100)` | Nome do motorista.                   |
| **AJUDANTE1**    | `VARCHAR(100)` | Código ou nome do ajudante 1.                            |
| **AJUDANTE2**    | `VARCHAR(100)` | Código ou nome do ajudante 2.                            |
| **AJUDANTE3**    | `VARCHAR(100)` | Código ou nome do ajudante 3.                            |
| **DATAGERACAO**  | `DATE`         | Data de geração do carregamento.🔴 **Obrigatório**. |
| **DATASAIDA**    | `DATE`         | Data de saída do carregamento.                           |
| **DATACHEGADA**  | `DATE`         | Data de chegada do carregamento (atualmente `NULL`).     |
| **DESTINO**      | `VARCHAR(20)`  | Destino do carregamento.                                 |
| **KMINICIAL**    | `VARCHAR(10)`  | Quilometragem inicial.                                   |
| **KMFIM**        | `VARCHAR(10)`  | Quilometragem final.                                     |
| **CODFILIAL**    | `VARCHAR(30)`  | Código da filial.🔴 **Obrigatório**.                |
| **CODIGOPEDIDO** | `VARCHAR(30)`  | Código do pedido associado.🔴 **Obrigatório**.      |


