# 📊 Dashboard Comercial | Power BI

## 📌 Sobre o Projeto

Projeto de Business Intelligence desenvolvido para analisar dados comerciais e transformar informações de vendas em indicadores que apoiam a tomada de decisão.

O trabalho contempla consultas SQL, modelagem dimensional, transformação de dados, medidas DAX e construção de um dashboard interativo no Power BI.

## 🚀 Tecnologias utilizadas

- SQL Server
- SQL
- Power Query
- Modelagem Dimensional (Star Schema)
- DAX
- Power BI

## 🧩 Modelagem de Dados

O modelo foi organizado em uma tabela fato de vendas, tabelas dimensionais e uma tabela dedicada à organização das medidas.

### Tabela fato

- **`fNotasFiscais`**: contém os registros de vendas, com códigos de cliente, departamento, loja, produto e vendedor, além de data da venda, quantidade e código da nota fiscal.

### Tabelas dimensionais

- **`dProdutos`**: códigos, nomes, departamentos, custos e valores dos produtos.
- **`dClientes`**: informações cadastrais e características dos clientes.
- **`dLojas`**: informações de lojas, cidades e departamentos.
- **`dDepartamento`**: códigos e nomes dos departamentos.
- **`dCalendario`**: datas, anos, meses e períodos no formato ano-mês.

### Organização das medidas

- **`Medidas`**: reúne medidas e recursos auxiliares utilizados nas análises do dashboard.

## 🔗 Relacionamentos do Modelo

A tabela fato `fNotasFiscais` está relacionada às dimensões por meio das seguintes colunas:

| Tabela fato | Coluna | Tabela dimensional | Coluna |
|---|---|---|---|
| `fNotasFiscais` | `Cod_Produto` | `dProdutos` | `Cod_Produto` |
| `fNotasFiscais` | `Cod_Cliente` | `dClientes` | `ID_Cliente` |
| `fNotasFiscais` | `Cod_Loja` | `dLojas` | `Cod_Loja` |
| `fNotasFiscais` | `Cod_Departamento` | `dDepartamento` | `Cod_Departamento` |
| `fNotasFiscais` | `Data_Venda` | `dCalendario` | `Date` |

Os cinco relacionamentos foram verificados no Power BI. Todos estão ativos, com cardinalidade muitos para um (*:1) e direção de filtro cruzado única.

Essa estrutura permite analisar as vendas por produto, cliente, loja, departamento e período.

## 📈 Principais indicadores

- Faturamento
- Lucro
- Quantidade Vendida
- Ticket Médio
- Ranking de Departamentos
- Receita por Loja
- Receita por Produto
- Receita por Cliente
- Evolução da Receita
- Insight Executivo Dinâmico

## 🗄️ Exemplos de SQL

### Criação do banco de dados

```sql
CREATE DATABASE DashboardComercial;
GO

USE DashboardComercial;
GO
```

### Importação de dados com BULK INSERT

Exemplo de importação de dados CSV para a tabela `Departamento`.

```sql
BULK INSERT Departamento
FROM 'C:\CAMINHO\Departamento.csv'
WITH (
    FIRSTROW = 2,
    FIELDTERMINATOR = ';',
    ROWTERMINATOR = '\n',
    CODEPAGE = '65001'
);
```

**Observação:** substitua `C:\CAMINHO\Departamento.csv` pelo caminho correto do arquivo utilizado no ambiente de execução antes de apresentar esse comando como script executável.

### Análise de lojas por cidade

Consulta para agrupar as lojas por cidade e contar os registros de cada grupo.

```sql
SELECT
    Cidade,
    COUNT(*) AS TotalLojas
FROM Lojas
GROUP BY Cidade;
```

### Validação dos dados importados

Consultas para visualizar os primeiros cinco registros de cada tabela.

```sql
SELECT TOP 5 * FROM Clientes;
SELECT TOP 5 * FROM Produtos;
SELECT TOP 5 * FROM Departamento;
SELECT TOP 5 * FROM Lojas;
SELECT TOP 5 * FROM NotasFiscais;
```

## 🧮 Exemplos de DAX

### Faturamento

Calcula o valor total das vendas a partir do preço dos produtos e das quantidades vendidas.

```dax
Faturamento =
SUMX(
    fNotasFiscais,
    RELATED(dProdutos[VL_Produto]) * fNotasFiscais[Quantidade]
)
```

### Lucro

Calcula a diferença entre o faturamento e o custo total, utilizando a medida `[Custo Total]` existente no modelo.

```dax
Lucro =
[Faturamento] - [Custo Total]
```

### Ticket Médio

Divide o faturamento pela quantidade de notas fiscais distintas.

```dax
Ticket Médio =
DIVIDE(
    [Faturamento],
    DISTINCTCOUNT(fNotasFiscais[Cod_NFiscal])
)
```

### Margem %

Calcula a relação entre o lucro e o faturamento.

```dax
Margem % =
DIVIDE(
    [Lucro],
    [Faturamento]
)
```

## 📷 Dashboard

### Visão de Clientes

![Dashboard de Clientes](clientes.png)

### Evolução da Receita

![Evolução da Receita](evolucao.png)

### Desempenho por Loja

![Receita por Loja](loja.png)

### Análise de Produtos

![Análise de Produtos](produtos.png)

### Receita por Departamento

![Receita por Departamento](receita%20por%20departamento.png)

## 💡 Análises e valor para o negócio

O dashboard reúne diferentes perspectivas dos dados comerciais para facilitar o acompanhamento dos resultados e apoiar a análise do desempenho.

- **Clientes:** identificação dos clientes com maior contribuição para o faturamento.
- **Evolução da receita:** acompanhamento do comportamento das vendas ao longo do tempo.
- **Lojas:** comparação do desempenho comercial entre unidades.
- **Produtos:** análise dos produtos com maior participação nas vendas.
- **Departamentos:** avaliação da contribuição de cada departamento para a receita.

A combinação dos indicadores e das visualizações permite explorar os dados sob diferentes perspectivas e identificar tendências e oportunidades de análise.

## 📚 Aprendizados

Durante o desenvolvimento do projeto, trabalhei com consultas SQL, importação e validação de dados, transformação de dados, modelagem dimensional, relacionamentos entre tabelas, medidas DAX e construção de visualizações no Power BI.
