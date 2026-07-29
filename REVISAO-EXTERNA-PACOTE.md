# Pacote de Revisão Externa (Red-team #2 — outra família de IA)

> Cumpre o §5.3 do projeto (revisão externa por revisor fora do loop). O Red-team #1 foi feito por um revisor adversarial da **mesma** família de IA que redigiu os documentos; esta rodada deve rodar em **outra família** (ex.: GPT, Gemini, Grok) para caçar os vieses que a família redatora não enxerga em si mesma.

## Como usar (passo a passo)

1. Abra o modelo escolhido (de família diferente de Claude) — de preferência a versão mais capaz disponível, com janela de contexto grande.
2. Anexe ou cole os **5 documentos**, nesta ordem: `MANDATO.md` → `PROJETO-AI-NATIVE-HEDGE-FUND.md` → `IMPLEMENTACAO-VALIDACAO-GOLIVE.md` → `REVISAO-CRITICA.md` → `PREPARACAO-CONTEUDO.md`. (Se não couberem numa mensagem, envie em mensagens sequenciais e o prompt por último.)
3. Cole o prompt abaixo, **sem editar**.
4. Traga a resposta de volta para esta sessão — os achados serão triados, verificados contra os documentos e incorporados como **Red-team #2** na `REVISAO-CRITICA.md`, cada um com decisão sua registrada.

## Prompt (colar verbatim)

```
Você é um revisor adversarial (red-team) contratado para ATACAR o desenho de um
projeto de fundo de investimento AI-native, antes de qualquer linha de código.
Seu trabalho é encontrar erros conceituais, contradições internas, hipóteses
frágeis e riscos não tratados — não elogiar, não resumir, não sugerir melhorias
genéricas.

Os documentos do projeto estão anexados/colados acima. Contexto essencial:
gestor único (uma pessoa, 15h/semana), capital próprio pequeno (PF, começar
pequeno), B3 small/mid caps negligenciadas como território, long-only + caixa,
teto de custo R$ 1.000/mês na construção, fontes de dados gratuitas primeiro,
LLMs como núcleo analítico com meta-modelo estatístico dimensionando posições,
paper trading de 6 meses em simulador próprio B3 antes de capital real.

Ataque com estas lentes (e outras que julgar relevantes):
1. Contradições internas entre documentos ou entre seções.
2. Hipóteses de retorno frágeis: onde o desenho assume edge que provavelmente
   não existe ou não sobrevive a custos, slippage e impostos.
3. Viabilidade para UMA pessoa em 15h/semana: o que ainda não cabe.
4. Estatística: onde amostras serão pequenas demais para as máquinas de
   calibração/meta-modelo/atribuição funcionarem como descritas.
5. Regras que soam bem mas são inexequíveis ou não-testáveis.
6. Especificidades do mercado brasileiro (B3, CVM, tributação PF, liquidez de
   small caps, qualidade de dados públicos) que o desenho trata mal.
7. O que um gestor profissional experiente riria ao ler.

NÃO repita o que o projeto já admite. Estão documentados e tratados: bus factor
de uma pessoa; edge não comprovado e não-backtestável nas fontes exóticas;
dependência de LLMs de terceiros; cold start estatístico; ciclo EOD e
long-only; iliquidez do território; qualidade dos dados brasileiros; custo fixo
alto; arquitetura copiável (§14.1 do PROJETO). Também já corrigidos (Red-team
#1): paper trading no mercado errado; contradição numérica de custos; percepção
variante sem consenso observável (resolvida via expectativas implícitas no
preço); look-ahead paramétrico do LLM no backtest (Gate 1 re-escopado);
estatística impossível no Gate 2 (n mínimo + rampa por livro); ausência de
orçamento de horas humanas; camada sistemática inexecutável em capital pequeno
(virou ETF configurável); livro de curtíssimo prazo (eliminado).

Retorne NO MÁXIMO os 8 achados mais importantes, ordenados por gravidade, cada
um neste formato: TÍTULO CURTO | onde está (documento/seção) | o problema em
2-4 frases | sugestão de correção em 1-2 frases. Cite seções específicas. Se
encontrar menos de 8 achados genuínos, retorne menos — achado forçado é ruído.
Retorne apenas os achados, sem introdução nem conclusão.
```

## O que fazer com a resposta

- Cole a resposta inteira aqui na sessão. Cada achado será verificado contra os documentos (red-teams também erram) e triado: **procedente** (vira correção com sua decisão), **parcial** (corrige-se o núcleo válido) ou **improcedente** (registrado com a refutação — também é informação).
- O resultado consolidado entra na `REVISAO-CRITICA.md` como **Red-team #2**, fechando o ciclo de revisão externa pré-código do §5.3. As rodadas seguintes são semestrais.
