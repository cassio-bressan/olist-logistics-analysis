# Análise de Dados – Operação Logística & Satisfação do Cliente

## 1. Contexto do Projeto
A operação logística é um dos principais fatores que influenciam a experiência do cliente no e-commerce. Atrasos na entrega podem impactar diretamente a satisfação, a reputação da empresa e a recorrência de compras.

Este projeto analisa dados reais de pedidos da Olist com o objetivo de **avaliar o desempenho das entregas**, identificar **padrões de atraso** e entender **como esses atrasos se associam à satisfação dos clientes**.

## 2. Visão Geral dos Dados
A análise considera os **96.470 pedidos entregues** da base, uma linha por pedido. Pedidos cancelados, indisponíveis ou ainda em trânsito ficam fora das métricas de entrega, para não serem contados como "no prazo". Um pedido é considerado atrasado quando chega depois do dia estimado informado ao cliente.

As métricas foram consolidadas em cinco bases:

- **kpis_gerais.csv**: indicadores gerais de entrega.
- **atrasos_por_seller.csv**: número de pedidos, taxa de atraso e atraso médio em dias por vendedor, considerando apenas vendedores com pelo menos 20 pedidos entregues.
- **atrasos_por_estado.csv**: número de pedidos, taxa de atraso, atraso médio e tempo médio de entrega por estado.
- **atraso_vs_score.csv**: nota média dos pedidos em dois cenários, entregas no prazo e entregas atrasadas.
- **probabilidade_nota1.csv**: probabilidade de um pedido receber nota 1 quando há atraso e quando não há atraso.

## 3. Principais KPIs
O dashboard (aba `Dashboard` do `Dashboard.xlsx`) reúne em uma única página os KPIs, os gráficos das três análises e as principais conclusões. Os cartões no topo apresentam uma visão geral do desempenho das entregas:

| Indicador | Valor | Definição |
| --- | ---: | --- |
| Taxa de pedidos atrasados | 6,8% | Percentual de pedidos entregues depois do dia estimado. |
| Atraso médio | 10,6 dias | Média de dias além do prazo, somente entre os pedidos atrasados. |
| Tempo médio de entrega real | 12,5 dias | Tempo médio entre a compra e a entrega efetiva. |
| Tempo médio estimado | 24,4 dias | Prazo médio informado ao cliente no momento da compra. |
| Folga média da estimativa | 11,9 dias | Diferença média entre o prazo estimado e o prazo real (estimado − real). |

Os prazos informados são, em média, cerca de duas vezes maiores que o tempo real de entrega. Mesmo com essa folga, 6,8% dos pedidos ainda chegam atrasados, e quando atrasam, passam em média mais de 10 dias do prazo.

## 4. Análise Logística
Entre os 804 vendedores com pelo menos 20 pedidos entregues, os piores têm **cerca de um terço dos pedidos atrasados** (até 33%), contra 6,8% na média da operação. Entre os vendedores com pelo menos 20 pedidos entregues e 5 pedidos atrasados, alguns apresentam **atraso médio bem acima do restante da base** (mais de 25 dias além do prazo), indicando gargalos operacionais específicos.

O volume mínimo é importante: sem ele, o ranking seria dominado por 51 vendedores com 100% de atraso, todos com apenas 1 ou 2 pedidos entregues.

Vale lembrar que o atraso cobre todo o ciclo do pedido (preparação pelo vendedor e transporte), então o vendedor não é um indicador isolado da transportadora.

Os resultados indicam que os atrasos **não estão distribuídos de forma homogênea** entre os sellers, sugerindo oportunidades claras de atuação direcionada nos piores desempenhos.

## 5. Análise Geográfica
Os atrasos apresentam **concentração regional bem definida**. Estados do **Nordeste, como Alagoas (AL, 21%), Maranhão (MA, 17%), Sergipe (SE, 15%), Piauí (PI, 14%) e Ceará (CE, 14%)**, lideram o ranking de **maior taxa de pedidos atrasados**, indicando maior recorrência de falhas logísticas nessas regiões. O **Rio de Janeiro (RJ)** também merece atenção: com 12% de atraso sobre mais de 12 mil pedidos, é o segundo estado em número absoluto de pedidos atrasados, atrás apenas de São Paulo.

O Nordeste também concentra os **atrasos mais longos**. Considerando os estados com pelo menos 10 pedidos atrasados, **Sergipe (SE, 16,2 dias), Ceará (CE, 15,2), Rio Grande do Norte (RN, 14,5) e Piauí (PI, 13,4)** lideram o atraso médio, ao lado do Rio de Janeiro (RJ, 13,5).

A **Região Norte** tem os **maiores tempos totais de entrega** (Roraima, Amapá e Amazonas levam entre 26 e 29 dias em média, contra 8,7 dias em São Paulo) e, no conjunto, a **segunda maior taxa de atraso do país** (8,6%, acima da média de 6,8%). Mas o comportamento dentro da região é bem diferente:
- **Amazonas (2,8%), Rondônia (2,9%), Amapá (3,0%) e Acre (3,8%)** têm as menores taxas de atraso do país. Nesses estados, os prazos informados são longos (de 39 a 47 dias, contra 24,4 na média nacional) e absorvem o ciclo de entrega demorado. Eles ficam fora do ranking de atraso médio porque têm de 2 a 7 pedidos atrasados cada, poucos demais para uma média confiável.
- **Pará (11,2%)**, que concentra 53% dos pedidos entregues da região, e **Tocantins (9,9%)** atrasam acima da média, com um padrão mais próximo do Nordeste. Roraima também tem taxa alta (12,2%), mas com apenas 41 pedidos entregues.

Esse padrão sugere dois comportamentos distintos:
- **Prazo descumprido** — Nordeste, Pará, Tocantins e RJ (em volume): atrasos mais frequentes e, no Nordeste, também mais longos;
- **Ciclo longo, mas dentro do prazo** — Amazonas, Rondônia, Amapá e Acre, associados a desafios de distância e infraestrutura.

Os resultados indicam a necessidade de **estratégias logísticas regionalizadas**, com foco no cumprimento de prazos no Nordeste, no Pará e no Tocantins, e na redução do tempo de ciclo nos estados do Norte mais distantes dos centros de distribuição.

## 6. Análise de Satisfação do Cliente
Os resultados mostram uma **associação forte entre atraso e pior avaliação do cliente**. Considerando os pedidos entregues com avaliação, os entregues dentro do prazo apresentam **score médio de 4,3**, enquanto os atrasados registram apenas **2,3**, uma queda de aproximadamente **47% na avaliação**.

Além disso, a **probabilidade de um pedido receber nota mínima (1)** salta de **6,6% nos pedidos no prazo para 53,8% nos pedidos atrasados**: mais da metade dos pedidos atrasados recebe a pior nota possível, um risco cerca de **8 vezes maior** de avaliação extremamente negativa.

Esses dados indicam que o atraso na entrega está entre os **principais sinais de insatisfação do cliente**, afetando não apenas a média das avaliações, mas também a ocorrência de experiências críticas. Por se tratar de uma análise observacional, o resultado mostra associação, não causa isolada: outros fatores (como problemas no produto) podem acompanhar os atrasos.

## 7. Impactos no Negócio
Os atrasos logísticos se relacionam com quatro frentes críticas do negócio. No pilar operacional, podem refletir ineficiências na cadeia de entrega, com potencial aumento de custos com suporte e reentregas (não medidos nesta análise). Na experiência do cliente, os dados mostram queda expressiva no score médio e forte aumento no risco de avaliações extremamente negativas.

Do ponto de vista de reputação, a alta concentração de notas baixas associadas a atrasos pode comprometer a imagem da plataforma e a confiança do consumidor. Por fim, há um possível impacto financeiro indireto, já que clientes insatisfeitos tendem a comprar menos vezes, o que aumenta a necessidade de investir em aquisição. Esses efeitos financeiros não foram modelados e devem ser tratados como implicações, não como resultados medidos.

## 8. Recomendações
Com base nos resultados apresentados, recomenda-se a priorização de ações focadas em redução de atrasos, controle operacional e mitigação do impacto na experiência do cliente.

No curto prazo, é essencial atuar sobre os estados e sellers com maiores taxas e médias de atraso, estabelecendo planos de correção específicos e acompanhamento contínuo por meio de metas operacionais. Também é recomendado o uso dos KPIs como indicadores de performance ativa, com monitoramento periódico.

No médio prazo, os prazos estimados de entrega devem ser calibrados por região e por vendedor. Hoje eles são, em média, quase o dobro do tempo real, mas ainda assim são estourados em 6,8% dos pedidos: há espaço para encurtar prazos onde a operação é consistentemente rápida e reforçá-los onde os atrasos se concentram. Adicionalmente, estratégias de comunicação proativa com clientes em pedidos com risco de atraso podem mitigar o impacto na satisfação.

Como ação estratégica, a empresa deve utilizar os dados de atraso como critério para avaliação de parceiros logísticos e vendedores, reforçando padrões mínimos de desempenho operacional.

## 9. Conclusão
A análise evidenciou uma associação forte entre atrasos logísticos e satisfação dos clientes: pedidos atrasados têm score médio quase 50% menor e cerca de 8 vezes mais chance de receber nota mínima. Além disso, foi possível identificar diferenças relevantes de desempenho entre estados e vendedores, indicando gargalos logísticos específicos por região.

Os KPIs mostram que os prazos estimados são, em geral, conservadores (24,4 dias estimados contra 12,5 reais), mas não evitam os atrasos nos pontos críticos da operação. Esse cenário reforça a necessidade de calibrar os prazos, aumentar o acompanhamento dos vendedores com pior desempenho e aprimorar a gestão logística nas regiões mais afetadas.

De forma geral, os dados geram insumos claros para decisões estratégicas voltadas à eficiência operacional, melhoria da experiência do cliente e redução de impactos negativos na imagem e nos resultados do negócio.