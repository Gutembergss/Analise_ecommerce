# 📊 Análise de Dados — E-commerce

Projeto de análise de dados desenvolvido em Python com o objetivo de transformar dados brutos de vendas em informações úteis para tomada de decisão.

O projeto foi dividido em duas etapas principais: **limpeza e preparação dos dados** e **análise exploratória e estatística**.

---

## 🎯 Objetivo

Investigar o comportamento das vendas de um e-commerce, identificando:

* desempenho de receita;
* ticket médio;
* desempenho por categoria de produto;
* comportamento por forma de pagamento;
* distribuição das vendas ao longo do tempo;
* padrões de cancelamento e devolução;
* possíveis associações entre características dos pedidos e seu status.

Além da análise descritiva, foram utilizados testes estatísticos para verificar se algumas diferenças observadas nos dados possuem **significância estatística**.

---

## 🗂️ Estrutura do projeto

```text
projeto_ecommerce/
│
├── projeto_ecommerce_limpeza.ipynb
├── projeto_ecommerce_analise.ipynb
├── ecommerce_limpo.csv
└── README.md
```

### Etapa 1 — Limpeza dos dados

Notebook responsável pela preparação do dataset bruto para análise.

### Etapa 2 — Análise dos dados

Notebook responsável pela EDA, análise de negócio, visualizações e testes estatísticos.

---

## 🧹 1. Limpeza e tratamento dos dados

O dataset original apresentava inconsistências propositalmente inseridas, exigindo uma etapa de preparação antes da análise.

Foram realizados:

* tratamento de valores ausentes;
* correção de tipos de dados;
* padronização de valores monetários;
* tratamento de datas em diferentes formatos;
* identificação e remoção de duplicidades;
* tratamento de valores inválidos;
* padronização de categorias;
* padronização de estados;
* validação e formatação de telefones;
* validação de endereços de e-mail;
* tratamento de idades inconsistentes;
* criação da variável de região;
* remoção de registros sem `id_cliente`, quando necessário para garantir a integridade das análises posteriores.

Ao final, foi gerado um dataset tratado para utilização na etapa de análise.

---

## 🔎 2. Análise Exploratória de Dados (EDA)

Foram analisadas as principais variáveis numéricas do dataset:

* idade;
* quantidade;
* preço unitário;
* valor total;
* avaliação do cliente.

Foram utilizadas estatísticas descritivas, histogramas e boxplots para compreender:

* distribuição dos dados;
* dispersão;
* assimetria;
* presença de outliers.

Os outliers de preço e valor dos pedidos não foram removidos automaticamente, pois podem representar **vendas legítimas de maior valor**, e não necessariamente erros de coleta.

---

## 📈 3. Correlação

Foi utilizada a **correlação de Spearman** para investigar relações entre as variáveis numéricas.

A escolha desse método ocorreu porque algumas variáveis apresentam assimetria e presença de outliers.

A análise permite investigar relações como:

> Existe relação entre a quantidade de itens e o valor total do pedido?

---

## 💰 4. Diagnóstico de receita

Foram analisados:

* faturamento total;
* ticket médio;
* receita por categoria;
* quantidade de itens vendidos por categoria;
* ticket médio por forma de pagamento.

### Principais resultados

O faturamento total analisado foi de aproximadamente:

**R$ 564.405,81**

com ticket médio de aproximadamente:

**R$ 1.198,31**

A categoria **eletrônico** apresentou a maior participação na receita, principalmente devido ao maior valor unitário dos produtos, e não necessariamente ao maior volume de itens vendidos.

O **Cartão de Débito** apresentou o maior ticket médio entre as formas de pagamento analisadas.

---

## 📅 5. Sazonalidade e tendência

Foi analisada a evolução do valor médio dos pedidos ao longo dos meses.

Essa análise permite identificar possíveis períodos de:

* aumento de vendas;
* redução de vendas;
* concentração de pedidos;
* possíveis efeitos sazonais.

Os resultados podem ser posteriormente cruzados com informações sobre campanhas promocionais e datas comerciais para investigar possíveis causas.

---

## ❌ 6. Cancelamentos e devoluções

Foi analisado o percentual de pedidos:

* entregues;
* cancelados;
* devolvidos.

Também foram realizados cruzamentos por:

* categoria de produto;
* estado;
* forma de pagamento.

### Principais observações

A categoria **Esporte e Lazer** apresentou a maior taxa de cancelamento, enquanto **Beleza** apresentou a maior taxa de devolução.

Entretanto, diferenças percentuais observadas entre grupos não significam necessariamente que exista uma relação estatisticamente significativa.

Por isso, foram realizados testes estatísticos.

---

## 🧪 7. Testes estatísticos

Foi adotado nível de significância de:

```text
α = 0,05
```

### Qui-quadrado

Utilizado para verificar se existe associação entre variáveis categóricas.

Foram analisadas:

* forma de pagamento × status do pedido;
* categoria do produto × status do pedido.

### Resultados

**Forma de pagamento × status**

```text
p = 0,0557
```

O resultado ficou ligeiramente acima de 0,05. Portanto, não foi encontrada evidência estatística suficiente para rejeitar a hipótese de independência.

Apesar disso, o resultado próximo ao nível de significância indica que a relação pode merecer investigação com uma amostra maior.

**Categoria × status**

```text
p = 0,3172
```

Não foram encontradas evidências de associação estatisticamente significativa entre categoria do produto e status do pedido.

---

### Mann-Whitney U

Utilizado para comparar a distribuição do valor dos pedidos entre:

* pedidos cancelados;
* pedidos entregues.

Resultado:

```text
p = 0,3009
```

Como o p-valor é superior a 0,05, não foram encontradas evidências de diferença estatisticamente significativa entre os grupos.

Portanto, dentro desta amostra, **o valor do pedido não apresentou evidência de relação com o cancelamento**.

---

## 💡 8. Principais insights

### Receita

A categoria **eletrônico** concentra a maior parte da receita, principalmente devido ao maior valor unitário dos produtos.

### Forma de pagamento

O cartão de débito apresentou o maior ticket médio.

Além disso, a análise estatística encontrou um resultado próximo do nível de significância entre forma de pagamento e status do pedido, indicando um ponto que merece investigação.

### Categoria

Apesar de existirem diferenças nas taxas de cancelamento e devolução entre categorias, o teste Qui-quadrado não encontrou evidência estatística suficiente de associação.

### Valor do pedido

O teste de Mann-Whitney não encontrou diferença estatisticamente significativa entre os valores dos pedidos cancelados e entregues.

---

## 🎯 9. Recomendação de negócio

A principal oportunidade identificada está na **forma de pagamento**.

Embora o teste não tenha atingido o nível de significância de 5%, o p-valor de **0,0557** ficou próximo do limite estabelecido.

Recomenda-se:

1. investigar possíveis problemas relacionados aos meios de pagamento;
2. analisar os motivos específicos dos cancelamentos;
3. aumentar o volume de dados antes de tomar decisões definitivas;
4. acompanhar a taxa de cancelamento por forma de pagamento ao longo do tempo.

A análise deve ser complementada com uma amostra maior antes de concluir que existe um efeito real.

---

## 🛠️ Tecnologias utilizadas

* **Python**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **Seaborn**
* **SciPy**
* **Jupyter Notebook**
* **Regex**
* **Git/GitHub**

---

## 📚 Conceitos aplicados

* Limpeza e tratamento de dados
* EDA — Análise Exploratória de Dados
* Estatística descritiva
* Correlação de Spearman
* Teste Qui-quadrado
* Teste Mann-Whitney U
* Análise de distribuição
* Identificação de outliers
* Análise de receita
* Análise de ticket médio
* Análise de cancelamentos e devoluções
* Interpretação de p-value
* Significância estatística
* Análise orientada a negócio

## 👩‍💻 Sobre o projeto

Projeto desenvolvido como parte do processo de aprendizado em **Análise de Dados**, com foco na aplicação prática de Python, estatística e pensamento orientado a negócio.

O objetivo não é apenas apresentar números, mas transformar os dados em **evidências que possam apoiar decisões**.
