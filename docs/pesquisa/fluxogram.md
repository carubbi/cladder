A metodologia central descrita no artigo CLADDER para eliciar o raciocínio causal formal em Large Language Models (LLMs) é a estratégia **CAUSALCOT** (Chain-of-Thought Causal). Esta estratégia de *prompting* é inspirada no conceito do **Motor de Inferência Causal (CI Engine)** postulado por Pearl, que decompõe um problema causal complexo em seis passos sequenciais e fundamentados simbolicamente.

O fluxograma a seguir, criado em Mermaid JS, detalha esses seis passos, divididos nas fases de Preparação e Solução, conforme exigido pelo prompt do CAUSALCOT:

```mermaid
graph TD
    subgraph A["FASE DE PREPARAÇÃO - Coleta de Informações"]
        style A fill:#D0E7FF,stroke:#333
        Q["Questão Causal em Linguagem Natural"]
        S1["Passo 1: Extrair o Grafo Causal"]
        S2["Passo 2: Determinar o Tipo de Consulta (Rung 1, 2 ou 3)"]
        S3["Passo 3: Formalizar a Consulta (Expressão Simbólica)"]
        S4["Passo 4: Reunir Dados Relevantes (Probabilidades)"]
        Q --> S1 --> S2 --> S3 --> S4
    end

    subgraph B["FASE DE SOLUÇÃO - Inferência Formal"]
        style B fill:#F5E0D0,stroke:#333
        S5["Passo 5: Deduzir o Estimand (do-calculus ou Contrafactual)"]
        S6["Passo 6: Calcular o Estimand (Cálculo Aritmético)"]
        S4 --> S5 --> S6
    end

    F{"Resposta Final: Sim ou Não"}
    S6 --> F

    %% Descrições adicionais do processo
    classDef CausalNode fill:#ADD8E6,stroke:#000,color:#000
    classDef graph_extraction fill:#B3E5FC,stroke:#0077B6,color:#000
    classDef query_classification fill:#FFE8A1,stroke:#C77E00,color:#000
    classDef formalization fill:#E2C2FF,stroke:#6A0DAD,color:#000
    classDef data_parsing fill:#D1FFD6,stroke:#2E7D32,color:#000
    classDef formal_inference fill:#FDD6C1,stroke:#E65100,color:#000
    classDef arithmetic fill:#C7FFD8,stroke:#00695C,color:#000
    class Q,F CausalNode
    class S1 graph_extraction
    class S2 query_classification
    class S3 formalization
    class S4 data_parsing
    class S5 formal_inference
    class S6 arithmetic

    linkStyle 0 stroke-width:2px,stroke:#1E90FF
    linkStyle 1 stroke-width:2px,stroke:#1E90FF
    linkStyle 2 stroke-width:2px,stroke:#1E90FF
    linkStyle 3 stroke-width:2px,stroke:#1E90FF
    linkStyle 4 stroke-width:2px,stroke:#FF8C00
    linkStyle 5 stroke-width:2px,stroke:#FF8C00
    linkStyle 6 stroke-width:2px,stroke:#3CB371
```

### Explicação do Fluxograma (CAUSALCOT)

O processo CAUSALCOT guia o LLM através de uma sequência lógica de sub-habilidades para garantir que ele aplique o **raciocínio causal formal**, e não apenas o conhecimento de senso comum (causal parrots).

#### Fase de Preparação

Esta fase envolve a compreensão e a transcrição dos elementos da pergunta (formulada em linguagem natural) para uma representação formal:

1.  **Passo 1: Extrair o Grafo Causal**: O modelo identifica e expressa a estrutura de grafo acíclico dirigido (DAG) que representa as relações de causa e efeito no cenário hipotético. Esta etapa avalia a sub-habilidade de **Extração de Relação Causal**.
2.  **Passo 2: Determinar o Tipo de Consulta (Rung)**: O modelo classifica a pergunta de acordo com a Escada de Causalidade de Pearl (Rung 1: Associação, Rung 2: Intervenção, Rung 3: Contrafactual). Esta é uma classificação multi-classe, e a correta identificação do Rung é crucial para a formalização correta. Esta etapa avalia a sub-habilidade de **Classificação de Questão Causal**.
3.  **Passo 3: Formalizar a Consulta**: A consulta, já classificada, é traduzida para sua **forma simbólica precisa** (por exemplo, usando a notação $P(Y|X)$ para Rung 1, $E[Y|do(X)]$ para Rung 2, ou $P(Y_x=y)$ para Rung 3). Esta etapa avalia a sub-habilidade de **Formalização**.
4.  **Passo 4: Reunir Dados Relevantes**: O modelo extrai os valores numéricos de probabilidades marginais ou condicionais fornecidos no prompt. O LLM deve parsear semanticamente os dados para o formato padronizado (e.g., $P(\dots|\dots)=\dots$). Esta etapa avalia a sub-habilidade de **Análise Semântica** (Semantic Parsing).

#### Fase de Solução

Esta fase é onde o raciocínio formal de inferência causal é aplicado:

5.  **Passo 5: Deduzir o Estimand**: Utilizando o grafo causal (Passo 1), o tipo de consulta (Passo 2) e a expressão simbólica (Passo 3), o modelo aplica as regras da inferência causal (como o *do-calculus* para Rung 2 ou previsão contrafactual para Rung 3) para simplificar a expressão formal em um **Estimand** — uma expressão que pode ser calculada com os dados disponíveis (Rung 1). Esta etapa avalia a sub-habilidade de **Inferência Causal Formal**.
6.  **Passo 6: Calcular o Estimand**: O modelo insere os dados numéricos (Passo 4) no *Estimand* deduzido (Passo 5) e realiza os cálculos aritméticos básicos para obter a resposta numérica final. Esta etapa avalia a sub-habilidade de **Aritmética**.

O resultado final do Passo 6 leva à **Resposta Final (Sim/Não)**, que é o objetivo principal do *benchmark* 

### Explicação do Fluxograma (CAUSALCOT)

O processo CAUSALCOT guia o LLM através de uma sequência lógica de sub-habilidades para garantir que ele aplique o **raciocínio causal formal**, e não apenas o conhecimento de senso comum (causal parrots).

#### Fase de Preparação

Esta fase envolve a compreensão e a transcrição dos elementos da pergunta (formulada em linguagem natural) para uma representação formal:

1.  **Passo 1: Extrair o Grafo Causal**: O modelo identifica e expressa a estrutura de grafo acíclico dirigido (DAG) que representa as relações de causa e efeito no cenário hipotético. Esta etapa avalia a sub-habilidade de **Extração de Relação Causal**.
2.  **Passo 2: Determinar o Tipo de Consulta (Rung)**: O modelo classifica a pergunta de acordo com a Escada de Causalidade de Pearl (Rung 1: Associação, Rung 2: Intervenção, Rung 3: Contrafactual). Esta é uma classificação multi-classe, e a correta identificação do Rung é crucial para a formalização correta. Esta etapa avalia a sub-habilidade de **Classificação de Questão Causal**.
3.  **Passo 3: Formalizar a Consulta**: A consulta, já classificada, é traduzida para sua **forma simbólica precisa** (por exemplo, usando a notação $P(Y|X)$ para Rung 1, $E[Y|do(X)]$ para Rung 2, ou $P(Y_x=y)$ para Rung 3). Esta etapa avalia a sub-habilidade de **Formalização**.
4.  **Passo 4: Reunir Dados Relevantes**: O modelo extrai os valores numéricos de probabilidades marginais ou condicionais fornecidos no prompt. O LLM deve parsear semanticamente os dados para o formato padronizado (e.g., $P(\dots|\dots)=\dots$). Esta etapa avalia a sub-habilidade de **Análise Semântica** (Semantic Parsing).

#### Fase de Solução

Esta fase é onde o raciocínio formal de inferência causal é aplicado:

5.  **Passo 5: Deduzir o Estimand**: Utilizando o grafo causal (Passo 1), o tipo de consulta (Passo 2) e a expressão simbólica (Passo 3), o modelo aplica as regras da inferência causal (como o *do-calculus* para Rung 2 ou previsão contrafactual para Rung 3) para simplificar a expressão formal em um **Estimand** — uma expressão que pode ser calculada com os dados disponíveis (Rung 1). Esta etapa avalia a sub-habilidade de **Inferência Causal Formal**.
6.  **Passo 6: Calcular o Estimand**: O modelo insere os dados numéricos (Passo 4) no *Estimand* deduzido (Passo 5) e realiza os cálculos aritméticos básicos para obter a resposta numérica final. Esta etapa avalia a sub-habilidade de **Aritmética**.

O resultado final do Passo 6 leva à **Resposta Final (Sim/Não)**, que é o objetivo principal do *benchmark* CLADDER.
