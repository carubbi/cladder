## Conceitos abordados

### D-Separação

O conceito de **d-separação** (separação direcional, onde "d" denota direcional) é um critério gráfico fundamental utilizado em modelos causais, como Redes Bayesianas e Modelos Causais Estruturais (SCMs), para determinar as relações de independência condicional entre conjuntos de variáveis.

O d-separação estabelece uma ponte essencial entre a estrutura topológica de um diagrama causal (as causas) e as relações probabilísticas (as independências condicionais) que devem ser observadas nos dados gerados por essa estrutura.

### Propósito e Princípio Básico

1.  **Analogia Gráfica da Independência:** O d-separação é um critério para decidir se um conjunto de variáveis $X$ é independente de outro conjunto $Y$, dado um terceiro conjunto $Z$. Se $X$ e $Y$ são **d-separados** por $Z$, isso significa que as variáveis que eles representam são definitivamente independentes, condicionalmente a $Z$. Se $X$ e $Y$ são **d-conectados** (ou seja, existe pelo menos um caminho não bloqueado), elas são possivelmente, ou muito provavelmente, dependentes.

2.  **Bloqueio de Caminhos:** $X$ e $Y$ são d-separados por $Z$ se **cada caminho** (sequência de arestas consecutivas de qualquer direção) entre um nó em $X$ e um nó em $Y$ for **bloqueado**. Apenas um nó que bloqueie é suficiente para bloquear o caminho inteiro.

### O Critério de Bloqueio (Definição Formal)

Para que um conjunto de nós $Z$ **d-separe** $X$ de $Y$ em um Grafo Acíclico Dirigido (DAG) $D$, todo caminho $p$ entre um nó em $X$ e um nó em $Y$ deve conter um nó $w$ que satisfaça uma das seguintes duas condições:

**1. Para Cadeias e Garfos (Não Colisores):**
*   Se o caminho $p$ contém uma **cadeia** ($A \to B \to C$) ou um **garfo** ($A \leftarrow B \to C$), o nó intermediário $B$ deve **estar no conjunto condicionante $Z$**.
*   Condicionar em um nó intermediário em uma cadeia ou garfo "bloqueia" o fluxo de informação (ou dependência) ao longo do caminho.

**2. Para Colisores (Garfos Invertidos):**
*   Se o caminho $p$ contém um **colisor** ($A \to B \leftarrow C$), o nó de colisão $B$ e **nenhum de seus descendentes** devem **estar no conjunto condicionante $Z$**.
*   Colisores (variáveis que são efeitos comuns de dois ou mais fatores) bloqueiam o fluxo de dependência na ausência de condicionamento.

### O Papel dos Colisores e o Condicionamento

A d-separação é particularmente importante devido à forma como o condicionamento afeta os colisores:

*   **Bloqueio Incondicional:** Em um caminho que passa por um colisor (ex: $A \to B \leftarrow C$), as variáveis nas "caudas" ($A$ e $C$) são marginalmente independentas (d-separadas) quando nada é observado. O colisor é o nó inativo (bloqueado).
*   **Ativação por Condicionamento:** O condicionamento (observação) de um nó colisor ($B$) ou de **qualquer um de seus descendentes** inverte esse status, tornando o caminho **ativo** (d-conectado). Saber o valor da consequência ($B$) faz com que as causas ($A$ e $C$) se tornem mutuamente dependentes, pois refutar uma explicação aumenta a probabilidade da outra.

Em resumo, $Z$ d-separa $X$ de $Y$ se $Z$ bloqueia todos os caminhos causais diretos (cadeias/garfos) *e* todos os caminhos espúrios abertos (colisores) entre $X$ e $Y$. A verificação de d-separação é uma ferramenta poderosa para testar modelos causais, pois a falha na independência condicional esperada nos dados permite a rejeição de um modelo causal proposto.

## Viés de Colisor

O **Viés de Colisor** é um conceito central em inferência causal e teoria de grafos (como nas Redes Bayesianas), que descreve como a observação de um evento específico pode alterar a probabilidade de suas causas potenciais, levando à dependência entre fatores que, de outra forma, seriam independentes.

Este efeito ocorre exclusivamente em estruturas gráficas que contêm um **colisor** (também chamado de "garfo invertido" ou *inverted fork*), que é um nó que recebe setas de dois ou mais outros nós (Causas $\to$ Efeito Comum $\leftarrow$ Causas).

### Conceito e Mecanismo

O *Viés de Colisor* é definido pela quebra da independência marginal que existe entre os nós pais (as causas) do colisor, quando o nó colisor ou qualquer um de seus descendentes é observado (ou seja, quando se condiciona nesse nó).

**1. Estrutura de Colisor (Independência Marginal):**

Em um grafo causal com um colisor $C$, onde $A \to C \leftarrow B$, se as variáveis $A$ e $B$ são causalmente independentes (não há caminho que as conecte, a não ser através de $C$), elas são **marginalmente independentes** (d-separadas) quando nada é observado.

**2. O Efeito Explicação-Eliminação (Dependência Condicional):**

O efeito ocorre quando se **condiciona em $C$** (o colisor). Condicionar no colisor *abre* o caminho entre as causas $A$ e $B$, induzindo uma correlação (dependência) entre elas.
Este é um excelente exemplo de raciocínio diagnóstico e do **efeito explicação-eliminação** (*explaining away effect*), que ocorre quando há condicionamento em um **colisor** ou em seu descendente.

O diagrama causal (Figura 1.2) estabelece as seguintes relações, onde $X_1$ é a Estação, $X_2$ é a Chuva, $X_3$ é o Aspersor, $X_4$ é o Piso Molhado e $X_5$ é o Piso Escorregadio:

1.  **Chuva ($X_2$)** e **Aspersor ($X_3$)** são causas do **Piso Molhado ($X_4$)**.
2.  O **Piso Molhado ($X_4$)** é a causa do **Piso Escorregadio ($X_5$)**.
3.  $X_4$ é um **colisor** no caminho entre $X_2$ e $X_3$ ($X_2 \to X_4 \leftarrow X_3$).
4.  A **Estação ($X_1$)** é uma causa comum de $X_2$ e $X_3$ ($X_2 \leftarrow X_1 \to X_3$).

### Análise Causal do Cenário

A questão exige calcular a probabilidade de ter chovido ($X_2=\text{Chuva}$) dadas duas observações (evidências): $X_1=\text{Seca}$ e $X_5=\text{Escorregadio}$.

#### 1. Ativação do Colisor
A observação de que o **piso está escorregadio ($X_5=\text{Escorregadio}$)** serve como forte evidência para a sua causa, o **piso molhado ($X_4=\text{Molhado}$)**.
Uma vez que o nó $X_4$ é um colisor e $X_5$ é um descendente desse colisor, a observação de $X_5$ **ativa** o caminho de dependência entre as causas $X_2$ (Chuva) e $X_3$ (Aspersor). O fluxo de informação é agora permitido entre $X_2$ e $X_3$.

#### 2. O Efeito Explicação-Eliminação (*Explaining Away*)
O fato de o chão estar molhado ($X_4$) precisa ser explicado por uma de suas causas: Chuva ($X_2$) ou Aspersor ($X_3$). Este é o efeito *explaining away*. A confirmação de uma causa torna a outra menos provável, induzindo uma correlação negativa entre elas.

#### 3. Introdução da Evidência da Estação
A informação chave é que a **estação é seca ($X_1=\text{Seca}$)**.

*   A estação ($X_1$) influencia diretamente a probabilidade de Chuva ($X_2$) e de Aspersor ($X_3$).
*   Uma estação "seca" torna a **Chuva ($X_2$)** *a priori* **menos provável**.
*   Ao mesmo tempo, uma estação "seca" pode tornar o **Aspersor ($X_3$)** *a priori* **mais provável** (ou pelo menos é uma explicação plausível para o molhado em um período seco).

Como já temos o resultado (piso molhado, implicado por escorregadio), a observação da Causa Comum $X_1=\text{Seca}$ reforça o contraste:

*   Se o chão está molhado, mas a causa $X_2$ (Chuva) é intrinsecamente improvável (devido à estação seca), isso **aumenta a probabilidade da alternativa $X_3$ (Aspersor)**.
*   A Aspersor ($X_3$) serve como a explicação mais provável para o Piso Molhado ($X_4$) na ausência de Chuva ($X_2$). Isso "explica e elimina" (explaining away) a necessidade de Chuva ($X_2$) como causa.

### Conclusão

Embora o piso escorregadio (que implica molhado) seja uma evidência de que *algo* causou a umidade, o fato de a **estação ser seca** reduz significativamente a probabilidade de que esse *algo* tenha sido a chuva.

Portanto, **é improvável que tenha chovido**. A explicação mais forte para o piso escorregadio/molhado, dada a estação seca, recai sobre o Aspersor.
A intuição central do *explaining away* é que **encontrar uma segunda explicação para um dado resultado torna a primeira explicação menos crível**. Se o efeito comum $C$ for verdadeiro:

*   Se soubermos que a Causa $A$ ocorreu, isso "explica" o resultado $C$, diminuindo a necessidade de invocar a Causa $B$.
*   Portanto, a confirmação de $A$ reduz a probabilidade de $B$, e vice-versa.

Essa relação induzida entre $A$ e $B$, após o condicionamento em $C$, é tipicamente uma **correlação negativa**.

### Nomenclatura e Importância

O **Viés de Colisor** (*collider bias*) também é conhecido como **paradoxo de Berkson**.

O entendimento desse efeito é crucial para a inferência causal, pois ele adverte contra o erro de ajustar estatisticamente para uma variável que é um efeito comum (colisor) das duas variáveis de interesse, já que isso introduziria um viés espúrio (o viés de colisor) na correlação entre elas.

Por exemplo, se $A$ e $B$ são independentes, mas ambos causam $C$. Se observamos $C$ (condicionamos em $C$), $A$ e $B$ se tornam dependentes: se $A$ for verdadeiro, isso *explica* $C$, e a probabilidade de $B$ diminui.

