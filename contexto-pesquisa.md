# Contexto da pesquisa

## Tema

Comparação empírica de estratégias de recuperação de informação (RAG denso,
RAG denso-esparso e GraphRAG incremental) e de estratégias de geração de
resposta (single-pass e verificada multiagente, padrão
Researcher-Auditor-Adjudicator) sobre um corpus jurídico-administrativo
brasileiro — leis federais, resoluções do Conselho Nacional de Justiça e
instruções normativas do Tribunal de Contas da União.

## Problema

A busca vetorial pura erra na recuperação de termos exatos (siglas, números
de processo, remissões normativas) e não resolve perguntas que exigem
conectar informação distribuída entre documentos distintos — por exemplo,
saber se uma norma continua em vigor diante de um ato que a revoga. O GraphRAG
ataca essa segunda limitação com um grafo de conhecimento entre documentos,
mas as implementações consolidadas reconstroem o grafo inteiro em lote,
premissa incompatível com um acervo que cresce continuamente. Falta saber
qual combinação de recuperação e geração produz respostas mais precisas,
fundamentadas e rastreáveis nesse domínio.

## O que já está decidido

- Arquitetura: PostgreSQL com pgvector e Apache AGE em um único storage
  (em vez de bancos vetorial e de grafo separados).
- Golden dataset com perguntas intra-documento e inter-documento (multi-hop),
  curado em duas fases.
- Métricas: RAGAS (Faithfulness, Context Precision, Context Recall, Answer
  Relevancy) e comparação estatística por teste de Wilcoxon signed-rank
  pareado com correção de Bonferroni (pareado, porque uma pergunta difícil
  tende a ser difícil nas três condições de retrieval — o teste pareado
  neutraliza essa variação compartilhada).
- Hipóteses pré-registradas (fase confirmatória): H1 — GraphRAG incremental
  supera RAG denso e RAG denso-esparso; H2 — geração verificada multiagente
  melhora a Faithfulness e a verificação de citação em relação à single-pass.
- Resultado já obtido na fase confirmatória: H1 e H2 **não se confirmaram** —
  o GraphRAG incremental não superou o RAG denso-esparso, e a geração
  multiagente não melhorou a Faithfulness. Uma fase exploratória posterior
  ampliou a comparação para catorze condições de recuperação e achou que a
  configuração mais precisa é, na verdade, a mais simples: RAG denso-esparso
  sem reranking com geração single-pass.
- Fora do escopo: validação qualitativa com usuários reais do domínio
  jurídico-administrativo foi cogitada, mas não integra o desenho — exigiria
  pesquisa com seres humanos, e a instituição não dispõe no momento de
  comissão de ética em funcionamento para submetê-la.

## O que ainda está em aberto

- Por que o GraphRAG incremental não se diferenciou, quando a literatura
  revisada o descreve com vantagem sobre RAG denso e híbrido em tarefas
  multi-hop — há uma explicação candidata (descasamento entre o pseudo-chunk
  sintético de entidade que o grafo entrega ao gerador e o que a métrica de
  Faithfulness penaliza), mas não testada formalmente até este ponto.
- Qual o recorte exato da contribuição científica do trabalho, dado que o
  resultado central é uma não confirmação de hipótese e uma configuração
  vencedora mais simples do que a prevista — isso é publicável como está, ou
  precisa de um reposicionamento do que o trabalho está de fato
  contribuindo?
- Se e como relatar a fase exploratória (catorze condições) no corpo
  principal da dissertação, já que ela não foi pré-especificada.

## Pergunta de aprofundamento sobre impacto científico

Dado que as hipóteses H1 e H2 não se confirmaram e que a configuração mais
precisa encontrada é a mais simples (RAG denso-esparso sem reranking com
geração single-pass, não a arquitetura mais sofisticada que as hipóteses
previam), qual é exatamente a contribuição científica deste trabalho que o
torna citável para além de "testamos GraphRAG neste domínio e não funcionou
melhor"?
