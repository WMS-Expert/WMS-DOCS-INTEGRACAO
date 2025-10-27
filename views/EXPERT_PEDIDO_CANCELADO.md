# View EXPERT_PEDIDO_CANCELADO

Responsável por listar os pedidos cancelados que não devem ser processados ou executados pelo WMS. 

## Código SQL

```sql
CREATE VIEW EXPERT_PEDIDO_CANCELADO ("CODIGOPEDIDO", "CODFILIALERP") AS
SELECT
  CAST(p.numped AS VARCHAR(30)) codigopedido,
  CAST(p.codfilial AS VARCHAR(30)) codfilialerp
FROM pcpedc p
WHERE p.posicao = 'C'


```


Exemplo de json:

```json
[
  {
    "codigopedido": "456789",
    "codfilialerp": "01"
  },
  {
    "codigopedido": "123456",
    "codfilialerp": "01"
  },
  {
    "codigopedido": "789123",
    "codfilialerp": "01"
  }
]


```

| Campo            | Tipo          | Descrição                                                  |
| ---------------- | ------------- | ---------------------------------------------------------- |
| **CODIGOPEDIDO** | `VARCHAR(30)` | Código do pedido cancelado. 🔴 **Obrigatório**             |
| **CODFILIALERP**    | `VARCHAR(30)` | Código da filial que originou o pedido. 🔴 **Obrigatório** |



