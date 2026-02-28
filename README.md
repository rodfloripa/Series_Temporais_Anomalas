
# Análise e Previsão de Séries Temporais Anômalas de Vendas com Lógica de Detecção Autônoma

<p align="justify">
Quando lidamos com séries que têm muitos "zeros" ou inícios abruptos, as métricas estatísticas tradicionais revelam muito sobre a natureza do dado. Abaixo, apresento a análise estatística, a escolha da métrica de erro, o modelo de Machine Learning (ML) ideal e a lógica de previsão para cada cenário.
</p>
<p align="center">
  <img src="https://github.com/rodfloripa/Series_Temporais_Anomalas/blob/main/series-temporais.png">
</p>

## Lógica de Detecção Autônoma para Séries Temporais

<p align="justify">
Para tornar o sistema autônomo, precisamos de uma lógica que analise a série temporal antes de decidir o modelo. As estatísticas-chave para essa detecção, aplicadas ao nosso portfólio, são:
</p>



---

* **Série 1 - Status Recente (Ciclo Encerrado):**
    <p align="justify">Se os últimos 20% da série são compostos quase integralmente por zeros, detecta-se que o ciclo de vida do produto se encerrou. O modelo deve interromper projeções de reposição para evitar estoque parado.</p>

* **Série 2 - Sparsity (Extrema Intermitência):**
    <p align="justify">Analisa a proporção de zeros no histórico. Se for >60%, a série é classificada como intermitente. Nesses casos, utiliza-se o cálculo de intervalo médio entre demandas (como o método de Croston) em vez de regressões lineares simples.</p>

* **Série 3 - Count de Eventos (One-Hit Wonder):**
    <p align="justify">Se houver apenas 1 ou 2 vendas registradas em todo o histórico, o sistema identifica o item como uma demanda única e não recorrente, tratando-o estatisticamente como ruído para não inflar a previsão futura.</p>

* **Série 4 - Tendência de Início (Lançamento):**
    <p align="justify">Se a série inicia com uma sequência longa de zeros e estabiliza em vendas positivas apenas na fase final, ela é classificada como lançamento. A lógica autônoma prioriza modelos de <i>boosting</i> que capturem a rampa de aceleração inicial.</p>

* **Série 5 - Sincronização de Portfólio (Datas Distintas):**
    <p align="justify">Em cenários com múltiplas séries e entradas temporais diferentes, a análise estatística é aplicada individualmente a cada item após a remoção dos "zeros à esquerda" (<i>leading zeros</i>). Isso garante que a idade relativa do produto não mascare sua verdadeira natureza, permitindo que o sistema aprenda padrões com produtos maduros para prever o comportamento de itens recém-chegados.</p>
