# Análise E-commerce — Limpeza de Dados

Pipeline de limpeza para um dataset de vendas com sujeira proposital:
formatos de data mistos (BR/US/ISO), preços com separadores decimais
inconsistentes, telefones e e-mails sem padronização, outliers de idade.

## O que foi feito
- Padronização de datas, valores monetários, telefone e e-mail
- Tratamento de nulos com imputação por mediana
- Remoção de duplicatas por (id_cliente, id_pedido)
- Engenharia de atributo: região a partir do estado

## Arquivos
- `vendas_ecommerce_bruto.csv` — dado original
- `projeto_ecommerce_limpeza_lib.ipynb` — notebook de limpeza
- `ecommerce_limpo.csv` — resultado final

## Próximos passos
Análise exploratória, visualizações e dashboard (em andamento)
