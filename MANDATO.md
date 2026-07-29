# Mandato do Fundo

> Documento-mãe do projeto (Ponto 1 da `REVISAO-CRITICA.md`, decidido pelo gestor em jul/2026). Todos os gates numéricos, orçamentos por livro e decisões de universo dos demais documentos referenciam este mandato. Mudança aqui exige decisão explícita do gestor humano e propaga revisão nos documentos dependentes.

## 1. Estrutura legal e capital

**Capital próprio, via pessoa física ou PJ/holding.** Sem registro de gestora na CVM nesta fase — nenhuma captação de terceiros é permitida. Gestora própria (Resolução CVM 21 + fundo sob Resolução 175) fica registrada como **meta condicionada**: só entra em avaliação após 2–3 anos de track record auditável do sistema em capital real.

Implicações tributárias operacionais (PF, a validar com contador na fase de implementação):
- Swing trade em ações: 15% sobre ganho líquido mensal (isenção de vendas até R$ 20 mil/mês em ações à vista); day trade: 20% sem isenção — **o sistema deve evitar enquadramento em day trade** (compra e venda do mesmo ativo no mesmo dia), restrição codificada no motor de risco para os livros de curtíssimo prazo.
- Apuração e DARF mensais entram no ciclo operacional (relatório mensal calcula o imposto devido).

## 2. Direcionalidade

**Long-only + caixa remunerado agora; long/short é evolução condicionada (fase 4+).**

- A posição defensiva é caixa (CDI/Selic via Tesouro/fundos DI — nos EUA-lab, T-bills/money market).
- A análise relativa de peers (§4.4 do projeto) serve, nesta fase, para **seleção** (comprar o melhor operador do setor), não para pares long/short.
- **Critério objetivo de habilitação do short** (não é data, é prova): (a) meta-modelo e camada de calibração em produção há ≥ 12 meses; (b) calibração comprovada (Brier score e curvas de confiabilidade dentro da meta definida no plano de validação); (c) IR positivo do livro longo no período; (d) estudo específico de custos de aluguel/squeeze na B3 aprovado pelo gestor. Os quatro cumulativos.

## 3. Benchmark e metas

**Régua dupla: Ibovespa como benchmark de habilidade + CDI como piso de existência.**

- **Piso (condição de existência):** retorno líquido > CDI no horizonte de avaliação. Capital próprio abaixo do CDI é destruição de valor — sem desculpa relativa.
- **Benchmark de habilidade:** exceder o Ibovespa mede a seleção de ações de fato.
- **Horizonte de avaliação:** janelas móveis de 12 meses para acompanhamento; avaliação de mandato em 36 meses.
- Parâmetros de risco herdados dos documentos vigentes e agora ancorados aqui: drawdown máximo tolerado 20% (circuit breaker do motor de risco); metas de gate (Sharpe ≥ 0,8 fora da amostra etc.) conforme `IMPLEMENTACAO-VALIDACAO-GOLIVE.md`, agora medidas contra **as duas réguas** deste mandato.

## 4. Mercado-alvo e sequência

**B3 é o alvo; EUA é laboratório agora — e vira alvo também depois da validação.**

1. **Fase atual (construção/validação):** universo analisável é B3 (onde está todo o diferencial de fontes do projeto: CVM, diários oficiais das 3 esferas, judicial, imprensa regional — sinais que ninguém arbitra). Execução nos EUA via Alpaca funciona como **laboratório**: valida o ciclo ordem→fill→reconciliação em paper trading enquanto a ponte de execução B3 (MT5/Cedro) não entra.
2. **Pós-validação (gates cumpridos em capital real na B3):** os EUA são promovidos de laboratório a **segundo mercado-alvo**, com o mesmo rito de entrada exigido para qualquer expansão: dossiês setoriais dos setores americanos cobertos, painéis econômicos, peer sets — os pré-requisitos dos §§4.1–4.6 valem integralmente; nenhum atalho por ser mercado "mais fácil".

## 5. Capacidade e giro

- Capacidade limitada pela regra de liquidez já vigente (posição ≤ X% do volume médio diário — motor de risco); irrelevante como restrição na escala de capital próprio inicial, revisitada se houver gestora.
- Giro esperado dominado pelos livros de médio/longo prazo e núcleo (ver §5.1 do projeto); livros de curtíssimo/curto prazo operam sob a restrição tributária do item 1 (evitar day trade).

## 6. Pendências delegadas à implementação

- Validação tributária formal (contador) antes do primeiro real em produção.
- Definição numérica de vol-alvo (proposta a calibrar no backtest: teto de volatilidade anualizada em torno de 15–20%, compatível com o drawdown máximo de 20%).

---

*Mandato v1 — decisões do gestor registradas em 29/07/2026. Documentos dependentes: `PROJETO-AI-NATIVE-HEDGE-FUND.md`, `IMPLEMENTACAO-VALIDACAO-GOLIVE.md`, `REVISAO-CRITICA.md` (ponto 6: atendido).*
