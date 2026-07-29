# Projeto E-commerce — Limpeza e Análise de Vendas

Pipeline completo de dados de um e-commerce: da base bruta (com sujeira proposital)
até a análise estatística de receita, sazonalidade e cancelamentos/devoluções.

## Estrutura do projeto

```
.
├── Vendas_ecommerce_bruto.csv              # Base original, com dados sujos
├── projeto_ecommerce_limpeza.ipynb         # Notebook de limpeza e tratamento dos dados
├── Ecommerce_limpo.csv                     # Resultado da limpeza — base usada na análise
├── projeto_ecommerce_analise.ipynb         # Notebook de análise estatística e diagnóstico
└── README.md                               # Este arquivo
```

O projeto é dividido em duas etapas sequenciais: **limpeza** (`projeto_ecommerce_limpeza.ipynb`), que transforma o CSV bruto no CSV limpo, e **análise** (`projeto_ecommerce_analise.ipynb`), que consome o CSV limpo para gerar os diagnósticos de negócio.

## Etapa 1 — Limpeza dos dados

**Entrada:** `Vendas_ecommerce_bruto.csv` — dataset com sujeira proposital: formatos de data mistos (BR/US/ISO), preços com separadores decimais inconsistentes (`R$ 49,90` vs `R$129,90`), telefones e e-mails sem padronização, categorias com grafias diferentes, textos com caixa e acentuação inconsistentes, e outliers de idade.

**Principais tratamentos aplicados** (`projeto_ecommerce_limpeza.ipynb`):

- **Valores monetários** (`valor_total`, `preco_unitario`): remoção de símbolos e normalização dos separadores decimais/milhar antes da conversão para `float`.
- **Quantidade e avaliação do cliente:** limpeza de caracteres não numéricos e imputação de nulos pela **mediana**.
- **Valor total ausente:** recalculado a partir de `preco_unitario * quantidade` quando possível; pedidos com valor zerado são tratados como nulos.
- **Forma de pagamento:** valores ausentes preenchidos com `"Não informado"`.
- **id_cliente:** limpeza de caracteres inválidos; registros sem esse identificador são descartados, pois impossibilitam relacionar o cliente aos pedidos.
- **Idade:** limpeza de caracteres não numéricos; valores abaixo de 18 ou acima de 100 anos são tratados como inconsistentes e descartados (nulos).
- **Data do pedido:** normalização de separadores e conversão para `datetime` (formatos mistos).
- **Nome do cliente:** remoção de caracteres indesejados, padronização de capitalização (`title case`).
- **Telefone:** parseado e validado com a biblioteca `phonenumbers` (código do país BR), formatado no padrão E.164; números inválidos viram nulo.
- **E-mail:** validado com `email_validator`; e-mails inválidos viram nulo.
- **Categorias de texto** (`forma_pagamento`, `status_pedido`, `categoria_produto`, `cidade`, `produto`): padronização de caixa e remoção de caracteres especiais.
- **Categoria de produto:** unificação de variações de grafia (ex.: `eletronicos`, `eletrônicos` → `eletrônico`).
- **Estado:** padronização para sigla (UF), incluindo mapeamento de nomes por extenso e remoção de valores inválidos.
- **Duplicatas:** remoção por `(id_cliente, id_pedido)`, preservando pedidos distintos de um mesmo cliente.
- **Engenharia de atributo:** criação da coluna `regiao` a partir do `estado`.

**Saída:** `Ecommerce_limpo.csv`, com as mesmas colunas do bruto mais `regiao`, pronto para análise.

## Etapa 2 — Análise estatística e diagnóstico

**Entrada:** `Ecommerce_limpo.csv` (480 pedidos).

**O que o notebook `projeto_ecommerce_analise.ipynb` faz:**

1. Remove colunas de identificação pessoal (`id_cliente`, `nome_cliente`, `email`, `telefone`) antes de qualquer análise.
2. Análise exploratória: estatísticas descritivas, distribuição das variáveis numéricas, outliers e correlação (Spearman).
3. Diagnóstico de receita: faturamento total, ticket médio, receita e volume por categoria, ticket médio por forma de pagamento.
4. Sazonalidade: evolução do valor médio dos pedidos ao longo dos meses.
5. Cancelamentos e devoluções: percentual por categoria, estado e forma de pagamento, com testes de **Qui-quadrado** (associação entre variáveis categóricas) e **Mann-Whitney U** (diferença de valor entre pedidos cancelados e entregues).

**Principais achados:**

- Faturamento total do período: ≈ R$ 564.405,81, com ticket médio de ≈ R$ 1.198,31 por pedido.
- A categoria **eletrônico** concentra a maior receita, não por vender mais unidades, mas por ter o maior valor unitário.
- **Cartão de Débito** tem o maior ticket médio; pedidos sem forma de pagamento informada têm ticket bem mais baixo.
- A testes com α = 0,05: apenas **forma de pagamento** mostra associação marginal com o status do pedido (p = 0,0557); **categoria de produto** (p = 0,317) e **valor do pedido** (p = 0,301) não mostram associação estatisticamente significativa com cancelamento/devolução.

## Requisitos

```
pandas
numpy
scipy
seaborn
matplotlib
phonenumbers
email_validator
```

## Como executar

1. Instale as dependências: `pip install pandas numpy scipy seaborn matplotlib phonenumbers email_validator`
2. Rode `projeto_ecommerce_limpeza.ipynb` com `Vendas_ecommerce_bruto.csv` na mesma pasta — ele gera `Ecommerce_limpo.csv`.
3. Rode `projeto_ecommerce_analise.ipynb` com `Ecommerce_limpo.csv` na mesma pasta.

## Próximos passos

- Investigar a associação marginal entre forma de pagamento e status do pedido com uma amostra maior.
- Construir um dashboard para acompanhamento contínuo dos indicadores de receita e cancelamento.
