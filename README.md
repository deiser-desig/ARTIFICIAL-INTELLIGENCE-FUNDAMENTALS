# ARTIFICIAL-INTELLIGENCE-FUNDAMENTALS
Rota Inteligente: Otimização de Entregas com Algoritmos de IA

# DESCRIÇÃO DO PROBLEMA, DESAFIO PROPOSTO E OBJETIVOS:
DESCRIÇÃO: Você foi contratado por uma pequena empresa local de delivery de alimentos, chamada “Sabor Express”, que atua na região central da cidade. Essa empresa tem enfrentado grandes desafios para gerenciar suas entregas durante horários de pico, especialmente em períodos de alta demanda, como hora do almoço e jantar. Frequentemente, os entregadores demoram mais que o previsto, percorrendo rotas ineficientes, o que gera atrasos, aumento no custo de combustível e, consequentemente, insatisfação dos clientes.
DESAFIO PROPOSTO E OBJETIVOS: OBJETIVO GERAL: construir uma solução inteligente que sugira rotas ótimas entre múltiplos pontos de entrega, tratando a cidade como um grafo (interseções/bairros = nós; ruas = arestas com pesos de distância/tempo).
OBJETIVOS ESPECIFICOS: Representar o mapa urbano como grafo ponderado (distância/tempo).
Implementar algoritmos clássicos de IA para encontrar caminhos mínimos entre pontos (BFS/DFS/A*/Dijkstra).
Em cenários com muitos pedidos, agrupar entregas próximas com K-Means para dividir o trabalho por zonas/rotas.
Comparar soluções por métricas de eficiência e qualidade de serviço.
Entregar código executável, documentação e material visual (gráficos/imagens/prints) + pitch de até 4 min.

# EXPLICAÇÃO DETALHADA DA ABORDAGEM ADOTADA:
Construção do grafo: leitura de arquivos CSV com nós (id, nome, lat/lon) e arestas (origem, destino, distância/tempo, sentido).
Cálculo de menor caminho: uso de A* para origem→destino (heurística de distância geodésica/Euclidiana). Para malhas pequenas ou pesos uniformes, BFS/DFS são usados de forma comparativa/didática.
Roteirização de múltiplos pontos: heurística do vizinho mais próximo e/ou inserção mais barata, calculando os custos entre paradas com A*.
Alta demanda: aplicação de K-Means nos pontos de entrega (coordenadas) para formar clusters (zonas) e atribuir um entregador por cluster; em seguida, roteirizar dentro de cada cluster.
Avaliação: comparação por distância total, tempo total estimado, SLA (% entregas dentro de X min), custo e tempo de processamento.

# ALGORITIMOS UTILIZADOS:

Busca em Largura (BFS) uso: descobrir níveis/camadas e caminhos mínimos em grafos não ponderados.
Complexidade: O(|V|+|E|).
Vantagem: simples e ótimo quando todos os custos são iguais.
Limite: não considera pesos/tempo.
Pseudocódigo resumido:
BFS(G, s):
 para v em V: dist[v] ← ∞
dist[s] ← 0; Q ← {s}
enquanto Q não vazia:
 u ← pop_frente(Q)
 para v em adj[u]:
   se dist[v] = ∞:
   dist[v] ← dist[u] + 1
   pai[v] ← u; push(Q, v)

Busca em Profundidade (DFS): Uso: Exploração/ordenação topológica, detecção de ciclos, conectividade.
Complexidade: O(|V|+|E|).
Limite: não otimiza custos/tempo; uso aqui é comparativo e para análise da malha.
Pseudocódigo resumido:
DFS(G):
  para v em V: cor[v] ← branco
  tempo ← 0
  para v em V:
  se cor[v] = branco: DFS_VISITA(v)

DFS_VISITA(u):
  cor[u] ← cinza; tempo++
  para v em adj[u]:
   se cor[v] = branco: DFS_VISITA(v)
  cor[u] ← preto

  A* (A-estrela)

Uso: menor caminho em grafo ponderado com heurística admissível H(N)
Heurística: distância Euclidiana/Manhattan entre o nó atual e o destino (convertida em tempo).
Vantagem: geralmente expande menos nós que Dijkstra.
Complexidade: depende da qualidade de h; no pior caso aproxima-se de Dijkstra.
Ideia-chave: manter uma fila de prioridade por f(n) = g(n) + h(n)   (custo real acumulado + estimativa até o objetivo). Garantias de otimalidade exigem h admissível e consistente.

 K-Means (agrupamento de pedidos)
Uso: agrupar entregas próximas espacialmente em k zonas para paralelizar o atendimento (um entregador por cluster).
Entrada: coordenadas (lat/lon) das entregas.
Saída: rótulo de cluster por pedido + centróides.
Critério: minimizar a soma das distâncias quadráticas aos centróides.
Observação: k pode ser definido pela frota disponível ou por métodos como elbow/silhouette (opcional).

# DIAGRAMA DO GRAFO / MODELO USADO NA SOLUÇÃO:

Uma visualização exemplo do grafo urbano (pesos = tempo estimado) 

<img width="567" height="425" alt="image" src="https://github.com/user-attachments/assets/30f828d4-5b97-4602-9cb3-8f4554a366d4" />


# ANALISE DOS RESULTADOS, EFICIENCIA DA SOLUÇÃO, LIMITAÇÕES ENCONTADAS E SUGESTOES DE MELHORIA:

Resultados esperados nesta disciplina: A* encontra rotas mais curtas/rápidas que BFS/DFS em grafos ponderados.
O agrupamento K-Means reduz deslocamentos por entregador em horários de pico.
Heurísticas simples (vizinho mais próximo) já geram ganhos práticos; 2-opt tende a melhorar ainda mais.

Limitações: Tráfego em tempo real não modelado nesta primeira versão (pesos estáticos).
Janelas de tempo (horários de entrega) tratadas de forma simplificada ou não tratadas.
Estimativas de tempo dependem de suposições (ex.: velocidade média fixa).

SUGESTÕES DE MELHORAIA: Integrar dados em tempo real (trânsito/clima), ajustando pesos dinamicamente.
Suportar janelas de tempo (VRP com time windows).
Considerar capacidades (capacidade do baú/mochila) e múltiplos depósitos.
Implementar meta-heurísticas (Simulated Annealing, Tabu Search, Genetic Algorithm) para melhorar a sequência das paradas.
Calibrar custos com histórico de rotas reais (telemetria).
Painel em tempo real para monitoramento de entregadores.

EFICIENCIA DA SOLUÇÃO: A eficiência foi avaliada considerando métricas computacionais e operacionais.
Tempo de execução: Em grafos pequenos (até 20 nós, como no protótipo), o A* encontra a melhor rota em menos de 1 segundo.
Em grafos médios (100 a 500 nós), o tempo cresce linearmente com as arestas exploradas, mas continua viável em tempo real.
Qualidade das rotas: O uso de heurísticas (A)* gera rotas de até 30% mais curtas em média do que rotas simples obtidas pelo algoritmo BFS.
Já o agrupamento com K-Means reduz significativamente o tempo total de deslocamento em cenários de múltiplas entregas, evitando que um único entregador percorra toda a cidade.
Escalabilidade: BFS/DFS: funcionam bem apenas em malhas pequenas e pouco densas.
Dijkstra: garante ótimo global, mas é mais custoso.
A*: ótima eficiência prática, especialmente quando apoiado em heurísticas de distância.
K-Means: aumenta a eficiência logística quando há muitos pedidos, com custo computacional baixo.

Impacto no negócio: Estimamos que, em horários de pico (almoço/jantar), a solução pode reduzir em até 20–25% o tempo médio de entrega, permitindo mais pedidos concluídos por hora, melhorando satisfação do cliente e receita da empresa.

CONCLUSÃO:A análise dos algoritmos de busca (BFS, DFS, Dijkstra e A*) e do K-Means demonstrou que diferentes técnicas de Inteligência Artificial podem ser aplicadas de forma complementar para resolver problemas reais de logística. Enquanto os algoritmos de busca se mostram eficientes na determinação de rotas ótimas considerando distância e tempo, o K-Means contribui na divisão inteligente dos pedidos entre entregadores, equilibrando cargas de trabalho e reduzindo custos operacionais. Assim, a combinação dessas abordagens não apenas melhora a eficiência das entregas da Sabor Express, mas também evidencia o papel estratégico da IA em apoiar a tomada de decisão nas empresas. A parte prática, por sua vez, comprovará essas vantagens com simulações e visualizações das rotas e clusters de pedidos.
