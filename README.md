# Olist — Análise de Atrasos na Entrega e Satisfação do Cliente

> Projeto completo de análise de dados que investiga o desempenho das entregas, os gargalos operacionais e a relação entre atrasos na entrega e satisfação do cliente, usando a base pública de e-commerce brasileiro da Olist.

**[Ver o Dashboard](./Projeto/Dashboard.xlsx)** · **[Ver o Notebook de Análise](./Projeto/Analysis.ipynb)** · **[Ver os Insights de Negócio](./Projeto/Insights.MD)**

---

## Visão Geral do Projeto

Este projeto analisa o desempenho das entregas em um marketplace brasileiro de e-commerce, com foco em entender como os atrasos na entrega se associam à satisfação do cliente.

A análise foi desenvolvida com a **Brazilian E-Commerce Public Dataset by Olist** e segue um fluxo analítico estruturado de ponta a ponta: definição de escopo e preparação relacional, extração dos dados, criação de variáveis, análise exploratória, construção do dashboard e geração de insights de negócio.

O projeto combina:

* **MySQL** para definição de escopo, preparação relacional e criação da view analítica;
* **SQL** para seleção de tabelas, joins, filtros e construção da view analítica;
* **Python e pandas** para tratamento, transformação, criação de variáveis e análise dos dados;
* **SQLAlchemy e PyMySQL** para extrair a view analítica do MySQL;
* **Microsoft Excel** para o dashboard e a visualização dos dados.

A análise está organizada em três dimensões de negócio:

1. **Desempenho Logístico** — identificar os sellers com maior frequência e gravidade de atrasos na entrega;
2. **Satisfação do Cliente** — avaliar como as notas dos clientes diferem entre pedidos atrasados e entregues no prazo;
3. **Desempenho Geográfico** — identificar os estados brasileiros com maiores taxas de atraso e maiores atrasos médios.

O objetivo não é apenas medir o desempenho das entregas, mas conectar indicadores operacionais às suas possíveis implicações para a experiência do cliente.

---

## Contexto de Negócio

No e-commerce, o desempenho das entregas é um componente crítico da experiência do cliente.

Um pedido atrasado pode representar mais do que uma ineficiência operacional. Quando a expectativa de entrega não é cumprida, a satisfação do cliente pode cair, aumentando a chance de avaliações negativas e podendo afetar a confiança do cliente e a reputação do marketplace.

Com base nesse contexto, o projeto investiga a seguinte pergunta central de negócio:

> **Como os atrasos na entrega se associam à satisfação do cliente, e onde estão os principais gargalos operacionais?**

Para responder a essa pergunta, a análise avalia o desempenho das entregas por seller e por estado e compara a satisfação do cliente entre pedidos atrasados e entregues no prazo.

---

## Objetivos de Negócio

O projeto foi estruturado em torno de quatro objetivos principais.

### 1. Medir o Desempenho das Entregas

Calcular indicadores operacionais capazes de descrever:

* Taxa de atraso nas entregas;
* Gravidade média dos atrasos;
* Tempo real de entrega;
* Tempo estimado de entrega;
* Diferença entre o tempo estimado e o tempo real de entrega.

### 2. Identificar Gargalos Operacionais

Determinar quais sellers e estados brasileiros apresentam o desempenho de entrega mais crítico.

### 3. Avaliar a Satisfação do Cliente

Comparar as notas dos clientes entre pedidos atrasados e entregues no prazo e medir como a probabilidade de receber a pior nota possível muda quando a entrega atrasa.

### 4. Gerar Insights Acionáveis

Transformar os resultados da análise em recomendações relacionadas a:

* Gestão de desempenho dos sellers;
* Logística regional;
* Prazos estimados de entrega;
* Experiência do cliente.

---

## Base de Dados

O projeto usa a **Brazilian E-Commerce Public Dataset by Olist**, uma base pública com informações sobre pedidos feitos no marketplace brasileiro da Olist.

A base original contém várias tabelas relacionais sobre pedidos, clientes, sellers, produtos, avaliações, pagamentos e outros aspectos da operação do marketplace.

Em vez de processar a base inteira sem critério, o projeto começou com uma **definição de escopo** deliberada. Foram selecionadas apenas as tabelas e os campos necessários para responder às perguntas de negócio definidas.

### Escopo Analítico

O fluxo analítico se concentrou nas seguintes tabelas de origem:

| Tabela de Origem | Finalidade Analítica                               |
| ---------------- | -------------------------------------------------- |
| `orders`         | Ciclo de vida do pedido e desempenho da entrega    |
| `order_items`    | Informações de produto, seller, preço e frete      |
| `order_reviews`  | Satisfação do cliente                              |
| `products`       | Características do produto                         |
| `customers`      | Análise geográfica                                 |
| `sellers`        | Localização do seller                              |
| `order_payments` | Tipo de pagamento, parcelas e valor                |

Essa definição de escopo reduziu a base às informações necessárias para a análise, mantendo o fluxo de trabalho focado e gerenciável.

### Fonte Original dos Dados

**Brazilian E-Commerce Public Dataset by Olist**

Disponível no Kaggle:

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

A base é usada para fins educacionais e de portfólio.

---

## Arquitetura dos Dados

O projeto começa consolidando os dados selecionados em uma view analítica no MySQL:

```text
vw_olist_full
```

A view analítica combina as informações relevantes das tabelas relacionais selecionadas:

```text
orders
   │
   ├── order_items ─── products
   │        └───────── sellers
   │
   ├── order_payments
   │
   ├── order_reviews
   │
   └── customers
```

Essa estrutura gera uma base analítica unificada, com as informações necessárias para avaliar logística, satisfação do cliente, características dos produtos e desempenho geográfico.

Por causa desses joins, cada linha da `vw_olist_full` representa uma combinação **item × pagamento × avaliação**, então um mesmo pedido pode aparecer em várias linhas (119.143 linhas para 99.441 pedidos). Por isso, todas as métricas de entrega e satisfação são calculadas em **nível de pedido**, com uma linha por pedido entregue.

### Fluxo Analítico dos Dados

```text
Base pública da Olist
        │
        ▼
      MySQL
        │
        ├── Definição de escopo
        ├── Seleção de tabelas
        ├── Joins em SQL
        └── Preparação analítica
        │
        ▼
   vw_olist_full
        │
        ▼
SQLAlchemy + PyMySQL
        │
        ▼
vw_olist_full.csv
        │
        ▼
   Python / pandas
        │
        ├── Tratamento dos dados
        ├── Criação de variáveis
        ├── Cálculo de KPIs
        └── Análise exploratória
        │
        ▼
Bases analíticas em CSV
        │
        ▼
   Microsoft Excel
        │
        ▼
Dashboard de uma página
```

Esse fluxo separa o projeto em etapas analíticas distintas, mantendo um caminho claro dos dados de origem até os insights de negócio.

---

## Preparação dos Dados

Depois de definir o escopo analítico e criar a view `vw_olist_full`, os dados selecionados foram extraídos do MySQL para o Python.

A extração usa **SQLAlchemy** e **PyMySQL** para consultar a view analítica e gerar a base usada na análise seguinte.

As principais etapas de preparação foram:

* Limpeza dos dados;
* Renomeação de variáveis;
* Ajuste dos tipos de dados;
* Restrição aos pedidos entregues (`order_status = delivered` com data de entrega);
* Deduplicação para uma linha por pedido (mantendo a avaliação mais recente quando o pedido tem mais de uma);
* Criação de variáveis analíticas;
* Cálculo de KPIs;
* Agregação por seller;
* Agregação por estado;
* Comparação entre pedidos atrasados e entregues no prazo;
* Geração das bases analíticas usadas no dashboard.

---

## Criação de Variáveis

Diversas variáveis derivadas foram criadas para apoiar a análise.

Todas as variáveis de entrega são calculadas apenas para pedidos entregues. Pedidos cancelados, indisponíveis ou em trânsito ficam vazios, para não serem contados como "no prazo".

### Tempo Real de Entrega (`tempo_entrega_real`)

Número de dias entre a compra e a entrega efetiva ao cliente.

```text
tempo_entrega_real =
order_delivered_customer_date
-
order_purchase_timestamp
```

### Tempo Estimado de Entrega (`tempo_entrega_estimado`)

Número de dias entre a compra e a data estimada de entrega informada ao cliente.

```text
tempo_entrega_estimado =
order_estimated_delivery_date
-
order_purchase_timestamp
```

### Dias de Atraso (`dias_atraso`)

Número de dias entre a data estimada de entrega e a entrega efetiva. Valores negativos indicam que o pedido chegou antes do prazo.

```text
dias_atraso =
order_delivered_customer_date
-
order_estimated_delivery_date
```

### Indicador de Entrega Atrasada (`entrega_atrasada`)

Indica se o pedido foi entregue depois do **dia** estimado. A data estimada não tem horário, então um pedido entregue no próprio dia estimado conta como no prazo.

```text
entrega_atrasada =
1 quando dias_atraso > 0
```

### Folga da Estimativa (`folga_estimativa`)

Diferença entre o tempo estimado e o tempo real de entrega. Valores positivos indicam que o prazo prometido foi maior que o tempo real de entrega.

```text
folga_estimativa =
tempo_entrega_estimado
-
tempo_entrega_real
```

### Variáveis Exploratórias por Pedido

O notebook também agrega, para cada pedido entregue, o número de itens, o valor total do frete e o peso total dos produtos, usados para explorar possíveis causas de atraso.

---

## Principais Métricas

A análise usa métricas operacionais e de experiência do cliente para avaliar o desempenho das entregas.

### Taxa de Atraso nas Entregas

Percentual de pedidos entregues depois da data estimada de entrega.

### Atraso Médio

Média de dias de atraso (`dias_atraso`), considerando apenas os pedidos atrasados.

### Tempo Médio Real de Entrega

Média de dias entre a compra e a entrega efetiva.

### Tempo Médio Estimado de Entrega

Prazo médio de entrega informado aos clientes.

### Folga da Estimativa

Diferença média entre o tempo estimado e o tempo real de entrega (estimado − real). Um valor positivo indica que os clientes recebem prazos maiores do que a operação realmente precisa.

### Nota Média do Cliente

Nota média das avaliações dos pedidos analisados.

### Probabilidade de Nota 1

Probabilidade de um pedido receber a pior nota possível do cliente.

---

## Análise e Dashboard

A análise final é apresentada em um dashboard de **uma única página** no Excel (aba `Dashboard`), pensado para ser lido de uma vez e impresso em uma folha A4 na horizontal. Ele é organizado em:

* **Cartões de KPI** no topo: pedidos entregues, taxa de atraso, atraso médio, tempo real de entrega, prazo estimado e folga do prazo;
* **Três seções lado a lado**, com dois gráficos cada: logística (sellers), satisfação do cliente e geografia (estados);
* **Principais conclusões** em uma faixa no rodapé, com fonte e período dos dados.

Os dados que alimentam os gráficos ficam em abas ocultas (`Dados_*` e `Rankings`), carregadas a partir dos CSVs da pasta `Tabelas/`.

### 1. Desempenho Logístico

A análise logística foca no desempenho de entrega por seller.

#### Top 10 Sellers por Taxa de Atraso

Classifica os sellers pelo percentual de pedidos entregues com atraso, considerando apenas sellers com pelo menos 20 pedidos entregues.

Essa métrica identifica os sellers em que os problemas de entrega acontecem com mais frequência. Sem o volume mínimo, o ranking seria preenchido por sellers com um ou dois pedidos que por acaso atrasaram.

#### Top 10 Sellers por Atraso Médio

Classifica os sellers pela média de dias de atraso entre os seus pedidos atrasados, considerando apenas sellers com pelo menos 5 pedidos atrasados.

Essa métrica mede a gravidade dos problemas de entrega, e não a sua frequência.

Analisar as duas métricas em conjunto dá uma visão mais completa do desempenho dos sellers, porque um seller pode apresentar:

* Alta frequência de atrasos relativamente curtos; ou
* Menor frequência de atrasos significativamente mais longos.

Essa distinção ajuda a identificar diferentes tipos de gargalos operacionais.

---

### 2. Atraso na Entrega e Satisfação do Cliente

Esta seção avalia a relação entre o desempenho das entregas e a experiência do cliente.

#### Nota Média do Cliente — Atrasado vs. No Prazo

Compara a nota média das avaliações entre pedidos entregues dentro do prazo estimado e pedidos entregues com atraso.

#### Probabilidade de Nota 1 — Atrasado vs. No Prazo

Compara a probabilidade de receber a pior nota possível nos dois cenários.

Isso dá uma visão mais direta da insatisfação extrema do cliente do que a nota média sozinha.

---

### 3. Desempenho Geográfico

A análise geográfica avalia o desempenho das entregas por estado.

#### Top 10 Estados por Taxa de Atraso

Identifica os estados onde as entregas atrasadas acontecem com mais frequência.

#### Top 10 Estados por Atraso Médio

Identifica os estados com os atrasos mais longos, entre os pedidos que atrasaram, considerando apenas estados com pelo menos 10 pedidos atrasados.

O mínimo é necessário porque alguns estados têm pouquíssimos pedidos atrasados: sem ele, o Amapá lideraria o ranking com uma média de 72 dias calculada a partir de apenas 2 pedidos.

Essas métricas oferecem visões complementares do desempenho regional:

> **A taxa de atraso mede a frequência, enquanto o atraso médio mede a gravidade.**

---

## Principais Resultados

### Desempenho dos Sellers

A análise por seller revelou diferenças relevantes no desempenho das entregas.

Entre os 804 sellers com pelo menos 20 pedidos entregues, os piores tiveram cerca de um em cada três pedidos entregues com atraso, contra uma taxa geral de atraso de aproximadamente 6,8%.

Isso indica que os problemas de entrega não estão distribuídos de forma homogênea entre os sellers e sugere oportunidades de atuação operacional direcionada.

Em vez de aplicar a mesma ação corretiva a todo o marketplace, os sellers com desempenho consistentemente ruim podem ser priorizados para acompanhamento e planos de melhoria.

---

### Desempenho Geográfico

A análise revelou padrões regionais distintos no desempenho das entregas.

Estados do **Nordeste**, como Alagoas (AL, 21%), Maranhão (MA, 17%), Sergipe (SE, 15%), Piauí (PI, 14%) e Ceará (CE, 14%), apresentaram as maiores taxas de atraso. O Rio de Janeiro (RJ) também se destaca: com 12% de atraso sobre mais de 12 mil pedidos entregues, é o segundo estado em número absoluto de pedidos atrasados.

O Nordeste também concentra os **atrasos mais longos**: entre os estados com pelo menos 10 pedidos atrasados, Sergipe (SE, 16,2 dias), Ceará (CE, 15,2), Rio Grande do Norte (RN, 14,5) e Piauí (PI, 13,4) estão no topo, ao lado do Rio de Janeiro (RJ, 13,5).

Os **estados do Norte**, como Roraima (RR), Amapá (AP) e Amazonas (AM), têm os maiores tempos totais de entrega (de 26 a 29 dias em média, contra 8,7 dias em São Paulo), mas poucos pedidos atrasados: Amapá, Amazonas, Acre e Rondônia têm taxas de atraso entre 3% e 4%, as menores do país. Roraima é a exceção (12%), mas com apenas 41 pedidos entregues. Como os prazos informados já consideram a distância, o problema dessa região é o ciclo de entrega longo, e não o descumprimento do prazo.

Isso sugere dois padrões operacionais diferentes:

* **Nordeste:** atrasos mais frequentes e mais longos, ou seja, o prazo prometido não é cumprido;
* **Norte:** ciclos de entrega longos, mas em geral dentro do prazo prometido.

A distinção é importante porque cada padrão exige uma resposta operacional diferente: no Nordeste, cumprir o prazo; no Norte, encurtar o ciclo de entrega.

---

### Satisfação do Cliente

O resultado mais forte da análise foi a diferença na satisfação do cliente entre pedidos atrasados e entregues no prazo.

Considerando os pedidos entregues com avaliação, os pedidos entregues no prazo apresentaram nota média de aproximadamente:

**4,3 / 5**

enquanto os pedidos atrasados apresentaram nota média de aproximadamente:

**2,3 / 5**

Isso representa uma **queda de aproximadamente 47% na nota média** ao comparar pedidos atrasados com pedidos entregues no prazo.

A probabilidade de receber a pior nota possível também aumentou de forma expressiva:

| Status da Entrega | Pedidos | Probabilidade de Nota 1 |
| ----------------- | ------: | ----------------------: |
| No prazo          |  89.443 |                  ~6,6%  |
| Atrasado          |   6.381 |                 ~53,8%  |

Os pedidos atrasados apresentaram, portanto, uma **probabilidade cerca de 8× maior de receber nota 1**.

Esses resultados trazem evidência forte de uma associação clara entre atrasos na entrega e experiências negativas do cliente.

---

## Implicações para o Negócio

Os resultados sugerem que o desempenho das entregas não deve ser tratado apenas como uma métrica operacional.

A forte associação observada entre atrasos e notas dos clientes indica que desempenho logístico e experiência do cliente devem ser monitorados em conjunto.

### Eficiência Operacional

Taxas de atraso elevadas podem indicar gargalos na operação dos sellers, no processamento dos pedidos, no transporte ou na logística regional.

### Experiência do Cliente

Pedidos atrasados estão associados a notas significativamente menores e a uma probabilidade bem maior de avaliações extremamente negativas.

### Reputação do Marketplace

A concentração de avaliações negativas associadas a problemas de entrega pode afetar a confiança do cliente e a percepção de confiabilidade do marketplace.

### Possível Impacto Financeiro

Os resultados financeiros **não foram modelados diretamente** neste projeto. Ainda assim, experiências negativas recorrentes podem ter consequências indiretas para o negócio, como maior demanda de suporte, menor retenção e mais pressão sobre a aquisição de clientes.

Por isso, esses efeitos devem ser tratados como **implicações de negócio, e não como resultados financeiros medidos diretamente**.

---

## Recomendações

Com base nos resultados da análise, recomendam-se as seguintes ações.

### Priorizar os Sellers com Pior Desempenho

Os sellers com as maiores taxas de atraso e os maiores atrasos médios devem ser priorizados para acompanhamento operacional e ações corretivas.

### Estabelecer o Monitoramento de Desempenho por Seller

A taxa de atraso e o atraso médio podem ser incorporados às avaliações periódicas de desempenho dos sellers.

### Desenvolver Estratégias Logísticas Regionais

Os diferentes padrões observados entre os estados brasileiros sugerem que as estratégias logísticas devem considerar as características de cada região, em vez de aplicar a mesma abordagem a todas as localidades.

### Melhorar os Prazos Estimados de Entrega

As datas estimadas de entrega devem ser avaliadas continuamente em relação ao desempenho real. Em média, os clientes receberam uma promessa de cerca de 24 dias, enquanto os pedidos chegaram em cerca de 12,5; ou seja, a maioria dos prazos é bastante conservadora e, ainda assim, 6,8% dos pedidos não os cumpriram. Os prazos podem ser calibrados por região e por seller, encurtando-os onde a operação é consistentemente rápida e reforçando-os onde os atrasos se concentram.

### Comunicar Proativamente os Riscos de Atraso

Pedidos identificados com alta probabilidade de atraso poderiam acionar uma comunicação proativa com o cliente, reduzindo a frustração quando o atraso não puder ser totalmente evitado.

### Monitorar Logística e Experiência do Cliente em Conjunto

Os KPIs de entrega devem ser analisados junto com as métricas de satisfação do cliente.

Um dashboard logístico pode identificar um aumento nos atrasos, enquanto as métricas de experiência do cliente ajudam a revelar a possível consequência desses atrasos.

---

## Estrutura do Projeto

```text
PROJETO-EXCEL/
│
├── Arquivos/
│   ├── Extract_data.ipynb
│   └── vw_olist_full.csv
│
├── Projeto/
│   ├── Analysis.ipynb
│   ├── Dashboard.xlsx
│   └── Insights.MD
│
├── Tabelas/
│   ├── atraso_vs_score.csv
│   ├── atrasos_por_estado.csv
│   ├── atrasos_por_seller.csv
│   ├── kpis_gerais.csv
│   ├── olist_tratada.csv
│   └── probabilidade_nota1.csv
│
├── .gitattributes
└── README.md
```

### `Arquivos/`

Contém o fluxo de extração dos dados e a base analítica exportada do MySQL.

* `Extract_data.ipynb` — conecta ao MySQL e extrai a view analítica `vw_olist_full` usando SQLAlchemy e PyMySQL.
* `vw_olist_full.csv` — base analítica exportada, usada como entrada da análise em Python.

### `Projeto/`

Contém o fluxo analítico principal, o dashboard e a documentação de negócio.

* `Analysis.ipynb` — faz o tratamento dos dados, a criação de variáveis, o cálculo dos KPIs e a análise de negócio. Lê e grava arquivos por caminhos relativos à pasta `Projeto/`.
* `Dashboard.xlsx` — dashboard de uma única página com os KPIs, os gráficos de logística, satisfação do cliente e desempenho geográfico, e as principais conclusões.
* `Insights.MD` — documenta os principais resultados, implicações para o negócio, recomendações e conclusões.

### `Tabelas/`

Contém as bases analíticas geradas no fluxo em Python.

* `atraso_vs_score.csv` — nota média dos clientes e número de pedidos para pedidos atrasados e no prazo.
* `atrasos_por_estado.csv` — número de pedidos, pedidos atrasados, taxa de atraso, atraso médio e tempo médio de entrega por estado.
* `atrasos_por_seller.csv` — número de pedidos, pedidos atrasados, taxa de atraso e atraso médio por seller (sellers com pelo menos 20 pedidos entregues).
* `kpis_gerais.csv` — KPIs logísticos gerais.
* `olist_tratada.csv` — base analítica tratada (uma linha por item × pagamento × avaliação, com as variáveis de entrega).
* `probabilidade_nota1.csv` — probabilidade de receber nota 1 e número de pedidos para pedidos atrasados e no prazo.

---

## Tecnologias

| Tecnologia           | Finalidade                                                               |
| -------------------- | ------------------------------------------------------------------------ |
| **MySQL**            | Definição de escopo, preparação relacional e criação da view analítica   |
| **SQL**              | Seleção de dados, joins, filtros e construção da view                    |
| **Python**           | Preparação, transformação, criação de variáveis e análise dos dados      |
| **pandas**           | Manipulação e processamento analítico dos dados                          |
| **SQLAlchemy**       | Conexão com o banco de dados e extração dos dados                        |
| **PyMySQL**          | Conectividade com o MySQL                                                |
| **Jupyter Notebook** | Fluxos de extração e de análise                                          |
| **Microsoft Excel**  | Construção do dashboard e visualização dos dados                         |

---

## Perguntas Analíticas

O projeto foi estruturado para responder às seguintes perguntas de negócio.

### Desempenho Logístico

* Qual percentual dos pedidos foi entregue com atraso?
* Qual é o atraso médio na entrega?
* Quais sellers têm as maiores taxas de atraso?
* Quais sellers têm os maiores atrasos médios?

### Desempenho Geográfico

* Quais estados têm as maiores taxas de atraso?
* Quais estados têm os atrasos de entrega mais longos?
* Os padrões de atraso são consistentes entre as regiões?

### Satisfação do Cliente

* Como a nota média do cliente difere entre pedidos atrasados e entregues no prazo?
* Como a probabilidade de receber nota 1 muda quando o pedido atrasa?
* Quão forte é a associação entre atrasos na entrega e experiências negativas do cliente?

---

## Desenvolvimento com Apoio de IA

O projeto foi concebido, desenvolvido e analisado por mim, com a IA usada como ferramenta de apoio durante o desenvolvimento.

O apoio da IA foi usado de forma pontual para:

* Revisão de consultas SQL;
* Revisão de código Python;
* Discussões técnicas sobre modelagem de dados relacionais;
* Boas práticas de preparação e análise de dados;
* Revisão e organização da documentação.

O escopo do projeto, as perguntas de negócio, a metodologia analítica, as métricas, a interpretação dos resultados e as recomendações finais foram definidos como parte do processo de desenvolvimento do projeto.

---

## Segurança

As credenciais do banco de dados foram removidas dos arquivos publicados do projeto.

Nenhuma senha, credencial de autenticação ou outra informação sensível de conexão está incluída no repositório.

---

## Propósito do Projeto

Este projeto foi desenvolvido como um estudo de caso de portfólio, para demonstrar competências práticas ao longo do fluxo de análise de dados.

O projeto combina:

**Definição de Escopo → SQL → Extração de Dados → Python → Criação de Variáveis → Análise Exploratória → Construção de KPIs → Business Intelligence → Insights de Negócio**

O principal objetivo foi demonstrar não apenas a capacidade de manipular e visualizar dados, mas também de transformar um problema de negócio em um fluxo analítico estruturado e traduzir os dados em conclusões de negócio acionáveis.

---

## Conclusão

A análise identificou diferenças relevantes no desempenho das entregas entre sellers e estados brasileiros e revelou uma forte associação entre atrasos na entrega e satisfação do cliente.

Os pedidos atrasados apresentaram notas médias substancialmente menores e uma probabilidade consideravelmente maior de receber a pior nota possível.

Os resultados reforçam a importância de tratar o desempenho logístico e a experiência do cliente como dimensões interligadas da operação de e-commerce.

Do ponto de vista analítico, o projeto mostra como um fluxo estruturado pode transformar uma grande base pública em uma solução analítica focada, capaz de apoiar decisões sobre desempenho dos sellers, logística regional, prazos estimados de entrega e experiência do cliente.
