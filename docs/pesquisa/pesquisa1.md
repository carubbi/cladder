# Query sem dados numéricos - probabilidade

1. Extrair o Gráfico Causal
O motor deve identificar a estrutura do gráfico causal (como Confounding, Mediation, etc.) a partir da descrição fornecida em linguagem natural. Esta etapa transforma o texto em um modelo gráfico formal (DAG).

2. Determinar o Tipo de Consulta
A consulta deve ser classificada dentro da Escada da Causalidade de Pearl (Rung 1, 2 ou 3) e seu tipo específico (ex: Efeito Médio do Tratamento - ATE, Probabilidade Contrafactual, etc.).

3. Formalizar a Consulta
O motor traduz a pergunta em linguagem natural para sua expressão matemática simbólica.
• Para consultas de intervenção (Rung 2), ele usaria o operador $do(\cdot)$ (e.g., $E[Y | do(X=1)]$ ).
• Para consultas contrafactuais (Rung 3), ele usaria notações contrafactuais (e.g., $P(Y_x = y)$).

4. Coletar os Dados Disponíveis
Nesta etapa, o motor idealmente extrai as probabilidades marginais e condicionais (os "dados") necessárias para o cálculo.
• Se os números estão ausentes: Esta etapa identificaria simbolicamente quais termos de probabilidade observacional (Rung 1, como $P(Y|X, Z)$ e $P(Z)$) seriam necessários, mas o resultado não incluiria valores numéricos.

5. Deducir o Estimand (O Ponto Principal da Inferência)
Esta é a etapa mais crítica de raciocínio causal formal e é independente dos valores numéricos. O motor deve:
- Aplicar as regras de inferência causal (como o do-calculus para Rung 2, ou métodos de previsão contrafactual para Rung 3).
- Simplificar a consulta formal (Etapa 3) em uma expressão equivalente (estimand) que possa ser calculada usando apenas as quantidades observacionais identificadas na Etapa 4.
- O resultado desta etapa deve ser a fórmula final, livre de operadores causais como $do(\cdot)$ ou notações contrafactuais. Por exemplo, converter $E[Y | do(X=1)] - E[Y|do(X = 0)]$ (ATE) na fórmula de ajuste backdoor: $\sum_{Z=v} P(Z=z)[P(Y=1|Z=z,X=1) - P(Y=1|Z=z, X=0)]$.

Se a consulta for identificável (ou seja, se o estimand puder ser derivado a partir de termos observacionais), o motor de inferência determinaria que a solução existe e forneceria o estimand como a resposta simbólica.

6. Calcular o Estimand (Etapa Bloqueada)
Sem a presença de valores numéricos ("dados"), o motor não pode prosseguir para a etapa final de cálculo aritmético para chegar à resposta final binária ("Sim" ou "Não"). Portanto, essa etapa seria omitida ou produziria uma resposta simbólica, mas não numérica.
Em resumo, a implementação do motor de inferência causal de Pearl, quando carece de dados numéricos, concentra-se em garantir a correta derivação do estimand (Etapa 5) através da aplicação rigorosa das regras do do-calculus ou dos Modelos Causais Estruturais (SCMs), confirmando assim a identificabilidade da quantidade causal.