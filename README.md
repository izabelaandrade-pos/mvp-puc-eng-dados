# mvp-puc-eng-dados
Repositório para documentação do MVP produzido na Sprint de Engenharia de Dados, parte da pós graduação em Ciência de Dados e Analytics da PUC-RJ.

# Análise de Mercado Imobiliário Residencial em São Paulo: Estratégia House Flipping

— Documentação e resumo gerado com auxílio do Gemini (LLM).

---

## 1. Visão Geral

Este repositório contém o projeto de Engenharia de Dados e a análise completa do mercado de imóveis residenciais na cidade de São Paulo. O objetivo é fornecer um panorama de preços e valorização para orientar a estratégia de **"House Flipping"** (compra, reforma e revenda rápida de imóveis com desconto).

O trabalho envolveu a ingestão de grandes volumes de dados públicos, a aplicação de técnicas de qualidade de dados (**Delta Lake/Spark**) e a construção de um modelo de dados otimizado para análise.

## 2. Resultados Chave e Descobertas

O estudo focado em transações de compra e venda de imóveis residenciais (Jan/2022 a Out/2025) revelou os seguintes *insights* críticos para investidores:

* **Top 10 Bairros de Alta Liquidez:** Os bairros mais caros e, presumivelmente, de maior liquidez (acima de 30 transações/ano) são dominados pela região Oeste de São Paulo, incluindo Jardim América, Jardim Paulistano, Jardim Europa, Itaim Bibi e Cidade Jardim.
* **Maior Valorização:** O bairro Cidade Jardim apresentou a maior valorização no preço do metro quadrado, com um aumento de **97,6%** no período de 2022 a 2025.
* **Tendência e Sazonalidade:** O mercado imobiliário em São Paulo apresenta uma valorização constante do preço médio do metro quadrado (aumento de **20,2%** de Jan/22 a Out/25, com R2 de 0.922 na tendência), sem sazonalidade definida que justifique esperar por um "melhor momento" de compra.
* **Preferência de Pagamento:** A maioria das transações (cerca de **63,5%**) é realizada sem financiamento imobiliário. Isso sugere que propostas de compra à vista têm um peso competitivo significativo para conseguir descontos.
* **Limitação da Análise:** Não foi possível correlacionar a valorização com características granulares dos imóveis (quartos, vagas, elevador), devido à indisponibilidade dessas informações em dados públicos de ITBI.

## 3. Metodologia e Tecnologias

O projeto seguiu a **arquitetura de Data Lakehouse**, utilizando o padrão **Medallion**, com camadas Bronze (dados brutos), Prata (dados tratados) e Ouro (dados agregados para análise).

| Área | Ferramentas e Tecnologias | Detalhes Técnicos |
| :--- | :--- | :--- |
| **Plataforma** | Databricks (Ambiente principal) | Uso de Notebooks para desenvolvimento, execução e visualização. |
| **Linguagem** | Python e Spark SQL | Python (requests, pandas, openpyxl) para ingestão e Spark SQL para transformações (`CREATE TABLE AS`, `JOINs`). |
| **Fontes de Dados** | ITBI (Prefeitura de SP) e CEP Aberto | Ingestão e consolidação de 4 arquivos .xlsx do ITBI (2022 a 2025) e dados de CEP em formato CSV. |
| **Modelagem** | Esquema Estrela (Prata) e Flat (Ouro) | Criação de tabelas Fato (`guias_itbi_prata`) e Dimensões (`cep_sp_capital`, `tabela_dim_usos`). E criação de uma tabela Flat (`guias_itbi_ouro`). |
| **Qualidade de Dados** | Testes de Unicidade, Integridade, Validade e Consistência | Tratamento de valores nulos, *outliers* de área, padronização de percentuais, exclusão de valores não válidos e filtragem de transações fora do escopo. |

## 4. Estrutura do Repositório

O projeto está dividido em três *notebooks*, que seguem a jornada da Engenharia de Dados e Análise:

* **20251201_MVP_EngDados_parte1.ipynb:** Contexto de Negócio, Objetivos e Ingestão de Dados (Criação da Camada Bronze).
* **20251201_MVP_EngDados_parte2.ipynb:** Modelagem, Qualidade e Transformação de Dados (Criação das Camadas Prata e Ouro).
* **20251201_MVP_EngDados_parte3.ipynb:** Análise Exploratória e Conclusões Finais (Resposta às Perguntas de Negócio e Visualizações).

## 5. Como Executar (Pré-requisitos)

Para reproduzir este projeto, você precisará de um ambiente que suporte o processamento distribuído de dados:

* **Plataforma Databricks:** Acesso a um *workspace* com suporte a Delta Lake.
* **Bibliotecas Python:** `openpyxl`, `pandas`, `requests`, `re`, `zipfile`.
* **Ambiente Spark:** Um cluster Spark configurado para executar os comandos Python e SQL.