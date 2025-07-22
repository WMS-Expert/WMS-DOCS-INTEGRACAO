# 📚 Retorno de Movimentos

Os retornos de movimentos no Wms Expert são fundamentais para garantir a sincronia com o ERP, evitando ações manuais. Eles estão divididos em três tipos:  
- **Retorno após a importação**  
- **Retorno após a exclusão**  
- **Retorno após a finalização do processo**  

---

## Retorno após a Importação de Movimentos (Entradas)

### Descrição
Após importar um movimento, o Wms notifica o ERP que o movimento foi recebido. Isso é feito via **stored procedure**, que valida o movimento e remove-o da view correspondente.

#### Movimentos de Entrada
```sql
CREATE procedure [dbo].[sp_RetornoImpEntrada] 
@codFiliaErp varchar(20), @codNotaFiscalErp varchar(20), 
@codFornecedorErp varchar(20), @serie int as

begin
  update NOTAFISCALENTRADA SET WMS = 1 
  WHERE 
    codFilialERP = @codFiliaErp and 
    codNotaFiscalErp = @codNotaFiscalErp and 
    codFornecedorErp = @codFornecedorErp and
    serie = @serie
end

```

---

## Retorno após a Exclusão de Movimentos

### Descrição
Quando um movimento é excluído, o Wms notifica o ERP que ele está novamente disponível. Isso é feito via stored procedure, que o retorna para a view correspondente.

#### Movimentos de Entrada
```sql
CREATE procedure [dbo].[sp_RetornoLiberaEntrada] 
@codFiliaErp varchar(20), @codNotaFiscalErp varchar(20), 
@codFornecedorErp varchar(20), @serie int as

begin
  update NOTAFISCALENTRADA SET WMS = 0 
  WHERE 
    codFilialERP = @codFiliaErp and 
    codNotaFiscalErp = @codNotaFiscalErp and 
    codFornecedorErp = @codFornecedorErp and
    serie = @serie
end

```

---

## Retorno após a Finalização de Movimentos

### Descrição
Ao concluir um movimento, o Wms envia ao ERP informações de cabeçalho e itens, atualizando o status para que o fluxo siga normalmente. Este processo é realizado via stored procedures.

#### Cabeçalho

```sql
CREATE procedure [dbo].[sp_RetornoExpEntrada] 
@codFiliaErp varchar(20), @codNotaFiscalErp varchar(20), 
@codFornecedorErp varchar(20), @serie int, @dataFimConferencia datetime as

begin
  update NOTAFISCALENTRADA SET DataFimConferencia = @dataFimConferencia 
  WHERE 
    codFilialERP = @codFiliaErp and 
    codNotaFiscalErp = @codNotaFiscalErp and 
    codFornecedorErp = @codFornecedorErp and
    serie = @serie
end

```
#### Itens

```sql
CREATE procedure [dbo].[sp_RetornoExpEntradaItem] 
@codFiliaErp varchar(20), @codNotaFiscalErp varchar(20), 
@codFornecedorErp varchar(20), @serie int, @codProdutoErp varchar(20), 
@qtdRecebida numeric(10,4), @codFuncConf varchar(20), @dtConferencia datetime as

Declare ItensEntrada Cursor for
  Select codProdutoErp, qtdRecebida, codFuncConf, dtConferencia
    from ItensNotaFiscalEntrada
   where 
    codFiliaErp = @codFiliaErp and 
    codNotaFiscalErp = @codNotaFiscalErp and 
    codFornecedorErp = @codFornecedorErp and 
    serie = @serie
Open ItensEntrada
fetch next from ItensEntrada into @codProdutoErp, @qtdRecebida, @codFuncConf, @dtConferencia
while @@Fetch_Status = 0
begin
  Update ItensNotaFiscalEntrada set
    qtdRecebida = @qtdRecebida,
    codFuncConf = @codFuncConf,
    dtConferencia = @dtConferencia
  where codProdutoErp = @codProdutoErp
end
```
---

## Retorno após a Importação de Movimentos (Saídas)

### Descrição
Após importar um movimento, o Wms notifica o ERP que o movimento foi recebido. Isso é feito via **stored procedure**, que valida o movimento e remove-o da view correspondente.

#### Movimentos de Entrada
```sql
CREATE procedure [dbo].[sp_RetornoImpPedido] 
@codFiliaErp varchar(20), @codPedidoErp varchar(20) as

begin
  update PEDIDO SET WMS = 1 
  WHERE 
    codFilialERP = @codFiliaErp and 
    codPedidoErp = @codPedidoErp
end
```
---

## Retorno após a Exclusão de Movimentos

### Descrição
Quando um movimento é excluído, o Wms notifica o ERP que ele está novamente disponível. Isso é feito via stored procedure, que o retorna para a view correspondente.

#### Movimentos de Entrada
```sql
CREATE procedure [dbo].[sp_RetornoLiberaPedido] 
@codFiliaErp varchar(20), @codPedidoErp varchar(20) as

begin
  update PEDIDO SET WMS = 0
  WHERE 
    codFilialERP = @codFiliaErp and 
    codPedidoErp = @codPedidoErp
end

```

---

## Retorno após a Finalização de Movimentos

### Descrição
Ao concluir um movimento, o Wms envia ao ERP informações de cabeçalho e itens, atualizando o status para que o fluxo siga normalmente. Este processo é realizado via stored procedures.

#### Cabeçalho

```sql
CREATE procedure [dbo].[sp_RetornoExpPedido] 
@codFiliaErp varchar(20), @codPedidoErp varchar(20), 
@dataFimConferencia datetime, @dataFimSeparacao datetime as

begin
  update PEDIDO SET 
    DataFimConferencia = @dataFimConferencia,
    DataFimSeparacao = @dataFimSeparacao 
  WHERE 
    codFilialERP = @codFiliaErp and 
    codPedidoErp = @codPedidoErp
end

```
#### Itens

```sql
CREATE procedure [dbo].[sp_RetornoExpEntradaItem] 
@codFiliaErp varchar(20), @codPedidoErp varchar(20), 
@codProdutoErp varchar(20), @item varchar(5), 
@qtdSeparada numeric(10,4), @qtdConferida numeric(10,4), 
@codFuncConf varchar(20), @codFuncSep varchar(20), 
@dtFimConferencia datetime, @dtFimSeparacao datetime as

Declare ItensPedido Cursor for
  Select codProdutoErp, qtdSeparada, qtdConferida, codFuncConf, 
         codFuncSep, dtFimConferencia, dtFimSeparacao, item
    from ItensPedido
   where 
    codFiliaErp = @codFiliaErp and 
    codPedidoErp = @codPedidoErp
Open ItensPedido
fetch next from ItensPedido into @codProdutoErp, @qtdSeparada, 
    @qtdConferida, @codFuncConf, @codFuncSep, @dtFimConferencia, 
    @dtFimSeparacao, @item
while @@Fetch_Status = 0
begin
  Update ItensPedido set
    qtdSeparada = @qtdSeparada,
    qtdConferida = @qtdConferida,
    codFuncConf = @codFuncConf,
    codFuncSep = @codFuncSep,
    dtFimConferencia = @dtFimConferencia,
    dtFimSeparacao = @dtFimSeparacao
  where 
    codProdutoErp = @codProdutoErp and
    item = @item
end

```
---

