# Mini Projeto — Análise de Dados de Vendas com Python

## Sobre o projeto

Este projeto foi desenvolvido como um **mini projeto de análise de dados utilizando Python**, com o objetivo de praticar conceitos de manipulação, exploração e visualização de dados.

A partir de uma base de **dados fictícios de vendas**, foram realizadas análises para identificar padrões relacionados a produtos, categorias, faturamento, cidades, estados e evolução das vendas ao longo do tempo.

O projeto foi desenvolvido utilizando principalmente **Pandas, NumPy, Matplotlib e Seaborn**, permitindo trabalhar desde a criação e organização dos dados até a geração de visualizações para facilitar a interpretação dos resultados.

> **Observação:** os dados utilizados neste projeto são fictícios e foram gerados programaticamente para fins de estudo e prática em análise de dados.

---

## Objetivos

O principal objetivo deste projeto foi colocar em prática conceitos fundamentais de análise de dados, como:

* Geração e organização de dados;
* Criação e manipulação de DataFrames;
* Exploração inicial de uma base de dados;
* Tratamento e transformação de dados;
* Criação de novas variáveis;
* Agrupamento e agregação de informações;
* Análise de vendas por produto;
* Análise de faturamento por localização;
* Análise de faturamento ao longo do tempo;
* Análise de faturamento por estado e categoria;
* Criação de gráficos para visualização dos resultados.

---

## Tecnologias utilizadas

| Tecnologia          | Utilização                               |
| ------------------- | ---------------------------------------- |
| 🐍 **Python**       | Linguagem utilizada no projeto           |
| 🐼 **Pandas**       | Manipulação e análise dos dados          |
| 🔢 **NumPy**        | Operações numéricas e geração de valores |
| 📊 **Matplotlib**   | Criação dos gráficos                     |
| 📈 **Seaborn**      | Estilização e visualização dos dados     |
| ☁️ **Google Colab** | Ambiente utilizado para desenvolvimento  |

As bibliotecas utilizadas no projeto incluem Pandas, NumPy, Matplotlib e Seaborn.

---

## Estrutura dos dados

A base utilizada possui **500 registros de vendas e 9 colunas inicialmente**.

As principais informações são:

| Coluna           | Descrição                  |
| ---------------- | -------------------------- |
| `ID_pedido`      | Identificador do pedido    |
| `Data Pedido`    | Data e horário da venda    |
| `Nome_Produto`   | Produto vendido            |
| `Categoria`      | Categoria do produto       |
| `preço Unitario` | Preço unitário do produto  |
| `Quantidade`     | Quantidade vendida         |
| `ID Cliente`     | Identificador do cliente   |
| `Cidade`         | Cidade relacionada à venda |
| `Estado`         | Estado relacionado à venda |

Os dados foram gerados considerando produtos de diferentes categorias e cidades da região Nordeste, com datas entre janeiro e abril de 2026.

---

## Geração dos dados

Como o objetivo do projeto é praticar análise de dados, foi criada uma função responsável por gerar os registros de vendas.

A função permite definir a quantidade de registros a serem criados:

```python
df_vendas = gera_dados_ficticios(500)
```

A partir disso, foi criado um DataFrame contendo **500 registros de vendas**.

Também foram utilizadas informações como:

* Produtos;
* Categorias;
* Preços;
* Quantidades;
* Clientes;
* Cidades;
* Estados;
* Datas de pedido.

---

## Exploração dos dados

Antes das análises, foram realizadas algumas verificações para entender a estrutura da base.

Entre elas:

```python
df_vendas.shape
```

```python
df_vendas.head()
```

```python
df_vendas.tail()
```

```python
df_vendas.info()
```

```python
df_vendas.describe()
```

A análise inicial mostrou que os 500 registros não apresentavam valores nulos nas colunas analisadas.

Também foram verificados tipos de dados, estatísticas descritivas, valores mínimos e máximos e distribuição das principais variáveis.

---

## Criação de novas informações

Durante a análise, foram criadas novas colunas a partir dos dados existentes.

### Faturamento

O faturamento de cada venda foi calculado através da multiplicação do preço unitário pela quantidade vendida:

```python
df_vendas['Faturamento'] = (
    df_vendas['preço Unitario'] * df_vendas['Quantidade']
)
```

Também foi criada uma classificação de tipo de entrega com base no estado:

```python
df_vendas['Tipo de Entrega'] = df_vendas['Estado'].apply(
    lambda estado: 'Rápida'
    if estado in ['BA', 'SE', 'PE']
    else 'Normal'
)
```

---

# Análises realizadas

## Produtos mais vendidos

Foi realizada uma análise da quantidade total vendida por produto utilizando `groupby()` e `sum()`.

```python
top_10_produtos = (
    df_vendas.groupby('Nome_Produto')['Quantidade']
    .sum()
    .sort_values(ascending=False)
    .head(10)
)
```

O resultado encontrado foi:

| Produto                | Quantidade |
| ---------------------- | ---------: |
| Mouse Attackshark      |        222 |
| SSD 1TB                |        208 |
| RTX 5090               |        207 |
| Headset                |        203 |
| Cadeira Gamer          |        202 |
| Gabinete Aquário       |        201 |
| Notebook Gamer         |        197 |
| Ryzen 7 5700G          |        194 |
| Teclado Machinike B500 |        189 |
| Pasta Térmica PCYES    |        158 |

Também foi criado um gráfico de barras horizontais para facilitar a comparação entre os produtos.

---

## Faturamento por cidade

Outra análise realizada foi o agrupamento do faturamento por cidade e estado.

```python
top_5_faturamentos = (
    df_vendas.groupby(['Cidade', 'Estado'])['Faturamento']
    .nlargest(5)
    .reset_index()
)
```

Entre os maiores valores encontrados estão:

| Cidade    | Estado |   Faturamento |
| --------- | ------ | ------------: |
| Recife    | PE     | R$ 327.132,96 |
| Fortaleza | CE     | R$ 292.970,09 |
| Natal     | RGN    | R$ 265.993,82 |
| Salvador  | BA     | R$ 261.226,92 |
| Maceió    | AL     | R$ 229.293,23 |

Os dados também foram utilizados para criar um gráfico comparativo entre as localidades.

---

## Faturamento mensal

Também foi analisada a evolução do faturamento ao longo dos meses.

Para isso, a data dos pedidos foi utilizada para criar uma nova informação referente ao mês:

```python
df_vendas['Mes'] = df_vendas['Data Pedido'].dt.to_period('M')
```

Depois, os valores foram agrupados por mês:

```python
faturamento_mensal = (
    df_vendas.groupby('Mes')['Faturamento'].sum()
)
```

### Resultado

| Mês            |   Faturamento |
| -------------- | ------------: |
| Janeiro/2026   | R$ 603.356,35 |
| Fevereiro/2026 | R$ 474.735,49 |
| Março/2026     | R$ 473.472,88 |
| Abril/2026     | R$ 188.450,13 |

Foi criado um gráfico de linha para representar visualmente a variação do faturamento mensal.

> **Importante:** abril possui registros apenas até o dia 10, portanto o valor desse mês não representa um mês completo.

---

## Faturamento por estado

Também foi realizada uma análise do faturamento agrupado por estado:

```python
vendas_estados = (
    df_vendas.groupby('Estado')['Faturamento']
    .sum()
    .sort_values(ascending=False)
)
```

Os valores encontrados foram:

| Estado |   Faturamento |
| ------ | ------------: |
| PE     | R$ 327.132,96 |
| CE     | R$ 292.970,09 |
| RGN    | R$ 265.993,82 |
| BA     | R$ 261.226,92 |
| AL     | R$ 229.293,23 |
| SE     | R$ 191.260,53 |
| MA     | R$ 172.137,30 |

Também foi criado um gráfico de barras para visualizar a distribuição do faturamento entre os estados.

---

## Faturamento por categoria

Por fim, foi analisado o faturamento gerado por cada categoria de produto.

```python
faturamento_categoria = (
    df_vendas.groupby('Categoria')['Faturamento']
    .sum()
    .sort_values(ascending=False)
)
```

### Resultado

| Categoria       |   Faturamento |
| --------------- | ------------: |
| Placas de vídeo | R$ 931.500,00 |
| Computadores    | R$ 236.400,00 |
| Processadores   | R$ 164.900,00 |
| Armazenamento   | R$ 145.600,00 |
| Periféricos     | R$ 118.143,17 |
| Móveis          |  R$ 90.900,00 |
| SOM             |  R$ 36.771,68 |
| Manutenção      |  R$ 15.800,00 |

Para essa análise, foi criado um gráfico de barras com formatação dos valores em milhares de reais.

---

# Visualizações

Durante o projeto foram desenvolvidas diferentes visualizações:

* Produtos mais vendidos;
* Faturamento por cidade;
* Evolução do faturamento mensal;
* Faturamento por estado;
* Faturamento por categoria.

As visualizações foram construídas utilizando principalmente **Matplotlib e Seaborn**.

---

# O que pratiquei neste projeto

Este mini projeto foi desenvolvido com foco no aprendizado prático de análise de dados.

Durante sua construção, pratiquei:

* Manipulação de DataFrames com Pandas;
* Criação de dados com Python;
* Utilização de `groupby()`;
* Utilização de `sum()`, `sort_values()` e `nlargest()`;
* Conversão e manipulação de datas;
* Criação de novas colunas;
* Análise estatística com `describe()`;
* Exploração de dados com `head()`, `tail()` e `info()`;
* Visualização de dados;
* Criação de gráficos com Matplotlib;
* Utilização do Seaborn para estilização dos gráficos;
* Organização de uma análise exploratória de dados.

---

# Próximos passos

Este projeto representa uma etapa inicial dos meus estudos em **Python e Análise de Dados**.

Como próximos passos, pretendo evoluir o projeto explorando:

* Limpeza e tratamento de dados mais complexos;
* Identificação e tratamento de valores ausentes;
* Análise de outliers;
* Novas métricas e indicadores;
* Visualizações mais elaboradas;
* Utilização de bases de dados reais;
* Integração com arquivos CSV e Excel;
* Exploração de ferramentas de Business Intelligence;
* Projetos de análise de dados com conjuntos de dados maiores.

---

## Autor

**Fernando**

Estudante de Ciência da Computação, com interesse em desenvolvimento de software, programação e análise de dados.

---

## Observação

Este projeto foi desenvolvido para fins **educacionais**, com dados fictícios gerados em Python.

Sinta-se à vontade para explorar o código, modificar as análises e utilizar o projeto como referência para estudos.

