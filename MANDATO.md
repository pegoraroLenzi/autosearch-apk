# Mandato do Fundo

> Documento-mãe do projeto (Ponto 1 da `REVISAO-CRITICA.md`, decidido pelo gestor em jul/2026). Todos os gates numéricos, orçamentos por livro e decisões de universo dos demais documentos referenciam este mandato. Mudança aqui exige decisão explícita do gestor humano e propaga revisão nos documentos dependentes.

## 1. Estrutura legal e capital

**Capital próprio, via pessoa física** (decisão do gestor, jul/2026): simplicidade máxima e isenção de IR em vendas até R$ 20 mil/mês em ações à vista — relevante na escala inicial. A consulta formal ao contador (item 6) confirma ou revisa a estrutura **antes do primeiro real em produção**; migração para PJ/holding fica como opção futura por escala ou planejamento patrimonial. Sem registro de gestora na CVM nesta fase — nenhuma captação de terceiros é permitida. Gestora própria (Resolução CVM 21 + fundo sob Resolução 175) fica registrada como **meta condicionada**: só entra em avaliação após 2–3 anos de track record auditável do sistema em capital real.

**Capital: começar pequeno, dimensionar com prova** (decisão do gestor, jul/2026): o capital alvo do go-live pleno **não é fixado agora** — será definido com o track record do paper trading em mãos (Gate 2), quando a conta custo do sistema × capital × performance esperada puder ser feita com dados reais em vez de esperança. O piloto (Gate 3) usa o critério já vigente: valor que dói zero perder por inteiro. Até lá, a viabilidade é governada pelo teto de custo (R$ 1.000/mês, `PROJETO...md` §12).

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

1. **Fase atual (construção/validação):** universo analisável é B3 (onde está todo o diferencial de fontes do projeto: CVM, diários oficiais das 3 esferas, judicial, imprensa regional — sinais que ninguém arbitra). **O paper trading valida a carteira B3 em simulador próprio com dados B3** (Red-team #1, achado 1 — a rampa deve validar o mercado onde o dinheiro vai operar); a Alpaca fica como laboratório opcional de integração ordem→fill→reconciliação contra uma API real. A ponte de execução B3 (MT5/Cedro) é construída e testada **durante** o Gate 2, e "ponte B3 validada em modo espelho" é critério do Gate 3.
2. **Pós-validação (gates cumpridos em capital real na B3):** os EUA são promovidos de laboratório a **segundo mercado-alvo**, com o mesmo rito de entrada exigido para qualquer expansão: dossiês setoriais dos setores americanos cobertos, painéis econômicos, peer sets — os pré-requisitos dos §§4.1–4.6 valem integralmente; nenhum atalho por ser mercado "mais fácil".

## 5. Capacidade, giro e orçamento de atenção

- Capacidade limitada pela regra de liquidez já vigente (posição ≤ X% do volume médio diário — motor de risco); irrelevante como restrição na escala de capital próprio inicial, revisitada se houver gestora.
- Giro esperado dominado pelos livros de médio/longo prazo e núcleo (ver §5.1 do projeto); o livro de curto prazo opera sob a restrição tributária do item 1 (evitar day trade). O livro de curtíssimo prazo foi **eliminado do escopo** até existirem dados intraday e ponte B3 reais (Red-team #1, achado 8).
- **Orçamento de atenção do gestor: 15 horas/semana** (decisão do gestor, Red-team #1 achado 6) — o teto de horas humanas que o sistema pode consumir (aprovações, rotulagem, curadoria de dossiês, autópsias, revisões). O escopo deriva dele: **fase inicial com 2 setores e ~10 empresas + âncoras**, expandindo para os 4 setores candidatos via `universe_config` conforme dossiês ficam prontos e as horas reais couberem. **Horas reais vs. orçadas é métrica acompanhada e critério de gate** — estouro sistemático reduz escopo, não aumenta a jornada.

## 6. Pendências delegadas à implementação

- Validação tributária formal (contador) antes do primeiro real em produção — confirma a escolha de PF do item 1.
- **Vol-alvo (decidido em jul/2026): teto provisório de 18% de volatilidade anualizada**, registrado em `policy_config` desde o dia 1 (meio da faixa, coerente com o drawdown máximo de 20%). O Gate 1 pode propor ajuste dentro de 15–20% com justificativa registrada — parâmetro ancorado no mandato: mudar além da faixa exige o rito de mudança deste documento.

---

*Mandato v1 — decisões do gestor registradas em 29/07/2026. Documentos dependentes: `PROJETO-AI-NATIVE-HEDGE-FUND.md`, `IMPLEMENTACAO-VALIDACAO-GOLIVE.md`, `REVISAO-CRITICA.md` (ponto 6: atendido).*
