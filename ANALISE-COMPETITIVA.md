# AI-Native Hedge Fund — Análise Competitiva de Ferramentas e Plataformas

> Mapeamento de pontos fortes e fracos das ferramentas existentes, em quatro categorias: (1) frameworks open source, (2) plataformas comerciais de research com IA, (3) provedores de dados alternativos de pessoas/web, (4) APIs de execução e plataformas autônomas. Pesquisa realizada em **julho/2026** a partir de fontes públicas (sites oficiais, papers, imprensa especializada, decisões judiciais); afirmações não confirmadas em fonte primária estão marcadas *(não verificado)*. Complementa `PROJETO-AI-NATIVE-HEDGE-FUND.md`.

---

## 1. Frameworks open source (comitês de agentes e quant)

**Alerta transversal:** backtests com LLM sofrem de *look-ahead bias paramétrico* — um modelo treinado em 2024+ já "sabe" como os mercados de 2018–2023 se comportaram. Pesquisas mediram decaimento de alpha superior a −15 p.p. entre períodos dentro e fora da janela de treino ([Look-Ahead-Bench, arXiv](https://arxiv.org/pdf/2601.13770)). Nenhum framework abaixo mitiga isso na camada do modelo; só TradingAgents e Qlib mitigam na camada de dados.

### 1.1 [virattt/ai-hedge-fund](https://github.com/virattt/ai-hedge-fund) (~62,5 mil ⭐, MIT)
Simulador com ~19 agentes: personas de investidores lendários (Buffett, Munger, Burry, Cathie Wood...) + valuation, sentimento, técnico, risco e PM.

- **Fortes:** maior comunidade do nicho "AI hedge fund"; arquitetura didática e extensível; backtester incluído; suporta LLMs locais (Ollama); licença MIT.
- **Fracos:** **não executa ordens** ("the system does not actually make any trades" — educacional); dados de uma única API paga (Financial Datasets), foco EUA; sem dados alternativos; sem mitigação de look-ahead; personas são imitação via prompt, não método validado.

### 1.2 [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) (~94,9 mil ⭐, Apache 2.0)
Framework acadêmico (arXiv 2412.20138) sobre LangGraph: analistas → debate bull/bear estruturado → trader → risco/PM.

- **Fortes:** arquitetura de referência do padrão "debate multi-agente" (a mesma do nosso projeto); manutenção muito ativa; único com filtro de look-ahead nos dados; respaldo acadêmico; honestidade metodológica no README.
- **Fracos:** sem execução real nem paper trading via broker; backtest do paper é curto (≈1 trimestre, poucos tickers — Sharpe 5–8 reportado é anomalia sinalizada pelos próprios autores); caro por decisão (~11 chamadas LLM + 20 tool calls); desenhado para 1 ticker por sessão, não para carteira contínua.

### 1.3 [FinGPT](https://github.com/AI4Finance-Foundation/FinGPT) (~21 mil ⭐, MIT)
LLMs financeiros open source (alternativa ao BloombergGPT): sentimento, NER, Q&A, forecaster.

- **Fortes:** fine-tuning barato (LoRA: US$ 17–300 vs. US$ 2,67 mi do BloombergGPT); modelos publicados no HuggingFace; bom para a camada de NLP/sentimento.
- **Fracos:** não é sistema de trading (sem portfólio, risco ou execução); estudos independentes apontam viés de recomendar "hold" e suscetibilidade a prompts adversariais; manutenção do core desacelerou.

### 1.4 [FinRobot](https://github.com/AI4Finance-Foundation/FinRobot) (~7,7 mil ⭐, Apache 2.0)
Agentes para relatórios de equity research; filosofia "números por código, narrativa por LLM".

- **Fortes:** 7 provedores de dados com failover; a separação determinístico/LLM reduz alucinação numérica (boa prática que adotamos); relatórios de qualidade profissional.
- **Fracos:** produto final é relatório, não P&L; sem trading nem backtest de portfólio; desktop restrito a macOS Apple Silicon; dependências de APIs pagas; foco EUA.

### 1.5 [Microsoft Qlib](https://github.com/microsoft/qlib) + [RD-Agent](https://github.com/microsoft/RD-Agent) (~46,8 mil ⭐, MIT)
A plataforma quant open source mais madura: model zoo (20+ modelos), banco point-in-time, RL de execução; RD-Agent adiciona mineração automática de fatores por LLM (ICML 2026).

- **Fortes:** melhor backtesting do open source (point-in-time de verdade); engenharia Microsoft; a ponte mais crível entre LLM e quant clássico; MIT.
- **Fracos:** sem integração com corretora; dados gratuitos (Yahoo) insuficientes para produção; curva de aprendizado alta; cobertura EUA/China; RD-Agent com disclaimer "not ready-to-use for investment".

### 1.6 [freqtrade/FreqAI](https://github.com/freqtrade/freqtrade) (~52,7 mil ⭐, GPLv3)
Bot de **cripto** com ML adaptativo, re-treino contínuo e RL.

- **Fortes:** o único da lista com pipeline completo backtest → dry-run → **execução real madura** (Binance, Kraken, OKX, Bybit...); backtester emula o re-treino histórico (evita um look-ahead comum); comunidade batalha-testada.
- **Fracos:** cripto apenas (não serve para ações); não é LLM/multi-agente (ML clássico); GPLv3 restringe produto comercial fechado; sem fundamentos nem dados alternativos.

### 1.7 Menores relevantes
- **[FinRL](https://github.com/AI4Finance-Foundation/FinRL)** (~15,8 mil ⭐, MIT): deep RL; único da família com paper/live trading parcial via Alpaca. Fraco: RL em finanças sofre overfitting severo a regimes; framework legado.
- **[FinMem](https://github.com/pipiku915/FinMem-LLM-StockTrading)** (~0,9 mil ⭐, MIT): memória hierárquica de agente (referência acadêmica que inspira nossa "memória por ativo"). Fraco: backtest apenas, pouca atividade desde 2024.
- **[ContestTrade](https://github.com/FinStep-AI/ContestTrade)** (~0,7 mil ⭐, Apache 2.0): competição interna entre agentes; foco A-shares chinesas. Fraco: comunidade pequena, sem execução.

**Síntese da categoria:** (1) nenhum framework multi-agente LLM executa ordens reais; (2) nenhum consome LinkedIn/Glassdoor nativamente; (3) nenhum suporta B3; (4) todos os backtests com LLM merecem ceticismo. → O espaço que nosso projeto ocupa (comitê LLM **+** dados de pessoas **+** execução real **+** gates de validação) está vago no open source.

---

## 2. Plataformas comerciais de research e sinais com IA

| Plataforma | Tipo | Sinais buy/sell? | Cobre B3? | Preço |
|---|---|---|---|---|
| AlphaSense | Research/market intel | Não | Parcial (busca em PT) | US$ 10–40k/usuário/ano |
| Bigdata.com (RavenPack) | Research + sentimento | Insumo de sinal | Sim (143 países) | US$ 50–100/mês |
| Brightwave | Research documental | Não | Não indicado | Sob consulta |
| Rogo | Agentes p/ banking | Não | Não indicado | ~US$ 3,3k/assento *(est., não verificado)* |
| Boosted.ai | ML quant + agentes | **Sim** | Provável via LatAm *(não confirmado)* | Sob consulta |
| Danelfin | Stock scores | **Sim** | **Não** (EUA/Europa) | US$ 28–299/mês |
| Reflexivity (ex-Toggle) | Macro/cenários | Parcial | Não confirmado | Grátis–US$ 99/mês (IBKR) |
| Exabel (BattleFin) | Alt-data → KPIs | Previsões de KPI | Praticamente não | Sob consulta |
| Kensho (S&P) | Infra de dados/IA | Não | Sim (Capital IQ) | Enterprise |
| Hebbia | Docs em escala | Não | Só via upload | ~US$ 3–10k/assento *(est.)* |
| Auquan | Agentes de workflow | Não | Não documentado | Sob consulta |
| Fintool | Copiloto SEC | Não | Não | Adquirida pela Microsoft (abr/2026) |

### Destaques por relevância ao projeto

**[AlphaSense](https://www.alpha-sense.com/)** — líder em market intelligence (500M+ documentos, research de 1.700 corretoras, expert calls; ARR > US$ 400M). *Fortes:* maior cobertura agregada; IA com citação de fontes; busca em 37 idiomas incl. português. *Fracos:* caro (mediana ~US$ 18k/usuário/ano); não gera sinais; sem backtesting/execução; profundidade B3 limitada.

**[Bigdata.com / RavenPack](https://www.ravenpack.com/blog/ravenpack-unveils-bigdata-ai-platform)** — research com IA sobre a base da pioneira em news sentiment (histórico desde 2000, 143 países, 13 idiomas). *Fortes:* preço acessível (US$ 50–100/mês); herança quant real; cobre Brasil; rastreabilidade das respostas. *Fracos:* a plataforma é research (o feed quant RavenPack Edge é vendido à parte, caro); filings estruturados focados em SEC; sem broker research.

**[Boosted.ai](https://www.boosted.ai/)** — ML quant no-code para gestores: rankings, baskets, backtesting; 60.000+ ações globais incl. LatAm. *Fortes:* um dos raros que gera sinal acionável com backtest; explicabilidade de fatores. *Fracos:* preço enterprise opaco; parcialmente caixa-preta; sem execução.

**[Danelfin](https://danelfin.com/)** — AI Score 1–10 por ação com track record publicado desde 2017 (~70% de acerto para scores ≥ 8, número da própria empresa). *Fortes:* sinal explícito e explicável, barato. *Fracos:* **não cobre B3**; sem research profundo; escala retail.

**[Reflexivity](https://reflexivity.com/en)** — grafo de conhecimento + cenários macro; clientes como Millennium e Soros; embarcada no Interactive Brokers. *Fortes:* nicho "what-if" quantitativo; evidência histórica por padrão. *Fracos:* sinais por analogia histórica (risco de espúrio); B3 não confirmada.

**[Exabel/BattleFin](https://www.battlefin.com/exabel)** — 75+ datasets alternativos pré-integrados → previsão de KPIs vs. consenso. *Fortes:* melhor ponte alt-data→decisão antes de resultados. *Fracos:* datasets centrados em consumo EUA/Europa; custo real inclui licenças dos dados; B3 mínima.

**[Kensho (S&P Global)](https://kensho.com/)** — infraestrutura: LLM-ready API para consultar dados S&P/Capital IQ via Claude/GPT. *Fortes:* dados confiáveis globais (inclui empresas B3); ideal para quem constrói agentes próprios — modelo que nos interessa. *Fracos:* blocos de construção, não produto final; preso a licenças S&P caras.

**Sinal de mercado relevante:** a Microsoft comprou a Fintool (abr/2026) e a Symphony comprou a Amenity — consolidação acelerada; ferramentas standalone de research tendem a ser absorvidas por plataformas maiores.

**Síntese da categoria:** quase tudo é *suporte à pesquisa*; sinal acionável de compra/venda só em Danelfin (retail, sem B3) e Boosted.ai. **Nenhuma executa ordens. Nenhuma cobre a B3 com profundidade nativa** (filings CVM, research local em português). Essa é a lacuna que nosso pipeline de ingestão próprio preenche.

---

## 3. Dados alternativos de pessoas e sentimento (o insumo do Agente de Pessoas)

| Provedor | Ticker mapping | Point-in-time | Histórico | Custo aprox. |
|---|---|---|---|---|
| Revelio Labs | Sim | Sim | ~2008+ | ~US$ 85k+/ano *(AWS; não verificado p/ contrato cheio)* |
| LinkUp (GlobalData) | Sim (PermID) | Sim | 2007+ | Institucional, sob consulta |
| Thinknum | Por ticker | Parcial | ~10 anos (jobs) | US$ 15–100k+/ano *(est., não verificado)* |
| Coresignal | Não | Não (~40 meses) | Curto | US$ 49–1.500+/mês |
| Bright Data | Não | Não | Snapshots | ~US$ 0,0025/registro |
| Aura (ex-Bain) | Não evidenciado | Não evidenciado | ~2021+ | Consumo/por usuário |
| People Data Labs | Não | Não | Datas de CV | US$ 98+/mês |
| RavenPack (news) | Sim | Sim (ms) | 2000+ | US$ 75–500k/ano *(est.)* |
| MarketPsych/LSEG | Sim (RIC/PermID) | Sim | 1998+ (PT: 2020+) | LSEG, sob consulta |

### Destaques

**[Revelio Labs](https://www.reveliolabs.com/)** — o mais completo em fluxos de funcionários (~1,1 bi de perfis, 4,5 mi de empresas; usado por fundos, Fed e academia). *Fortes:* distingue fluxo orgânico de M&A; metodologia publicada de correção de viés; cobre Brasil. *Fracos:* caro (US$ 85k+/ano só o pacote AWS); viés white-collar (no Brasil subrepresenta o trabalho informal/operacional); lag inerente de atualização de perfis.

**[LinkUp](https://www.linkup.com/industry-solutions/capital-markets)** — vagas coletadas **direto dos sites dos empregadores** desde 2007, point-in-time, com PermID. *Fortes:* melhor qualidade de sinal de contratação (sem vagas fantasma); ~20 anos para backtest. *Fracos:* profundidade fora dos EUA bem menor; só vagas (sem fluxos nem reviews); institucional caro.

**[Coresignal](https://coresignal.com/pricing/)** — perfis brutos (865M+) a preço self-service. *Fortes:* ordem de grandeza mais barato; granular; cobre Brasil. *Fracos:* sem ticker mapping nem point-in-time — o trabalho quant fica conosco; zona cinzenta legal mais exposta.

**[Bright Data](https://brightdata.com/products/datasets/glassdoor)** — datasets de marketplace, incl. **o único dataset de Glassdoor à venda em escala** (35M+ registros). *Fortes:* mais barato por registro; posição legal fortalecida (venceu Meta e X em 2024); subconjuntos custom (ex.: só Brasil). *Fracos:* dado bruto sem qualquer curadoria financeira; snapshots (sem série longa pronta); risco de compliance transferido ao comprador.

**[MarketPsych/LSEG](https://www.marketpsych.com/)** — sentimento de notícias+social com **texto em português desde 2020** e índices por país/moeda (Brasil, BRL). *Fortes:* único com suporte explícito a mídia brasileira; 28 anos de histórico em inglês; entrega via LSEG pronta para quant. *Fracos:* backtest do sinal local limitado a ~6 anos; sentimento agregado é sinal cada vez mais commodity.

### Estado legal 2026 (resumo executivo)

- *hiQ v. LinkedIn* terminou em **acordo favorável ao LinkedIn** (2022): scraping de dado público não é crime federal (CFAA), mas quebra de ToS + contas falsas geram responsabilidade civil real ([resumo](https://en.wikipedia.org/wiki/HiQ_Labs_v._LinkedIn)).
- *Meta v. Bright Data* e *X v. Bright Data* (2024): scraping **deslogado** de dados públicos não viola ToS ([análise](https://www.fbm.com/publications/major-decision-affects-law-of-scraping-and-online-data-collection-meta-platforms-v-bright-data/)).
- LinkedIn segue litigando agressivamente: a Proxycurl (maior API de scraping de LinkedIn) **fechou em 2025** com injunction que alcança também os clientes ([notícia](https://www.socialmediatoday.com/news/linkedin-wins-legal-case-data-scrapers-proxycurl/756101/)).
- Glassdoor não oferece API/parceria; acesso legal na prática é via agregadores.

**Conclusão prática (confirma a decisão do projeto):** scraping próprio de LinkedIn/Glassdoor = risco civil relevante, desaconselhado. Comprar de scrapers brutos = defensável nos EUA pós-2024, mas risco residual + trabalho de curadoria ficam conosco. Provedores investment-grade (Revelio, LinkUp) = menor risco e melhor dado, a custo muito maior. **Estratégia em fases: começar com MarketPsych/Bigdata (sentimento BR barato) → amostras Coresignal/Bright Data para validar o sinal → só contratar Revelio/LinkUp se o sinal provar valor no paper trading.**

---

## 4. Execução e plataformas autônomas

### 4.1 APIs de corretora

| Corretora/via | Mercado | Aceita residente BR? | Custo | Observação |
|---|---|---|---|---|
| [Alpaca](https://alpaca.markets/) | Ações/ETFs/opções EUA + cripto | **Sim** | Comissão zero; paper grátis | Nossa escolha de MVP confirmada |
| [IBKR](https://www.interactivebrokers.com/campus/ibkr-api-page/ibkr-api-home/) | ~160 mercados globais | **Sim** | Comissões baixas; dados pagos | Sem B3; API poderosa porém arcaica (TWS/Gateway) |
| [Tradier](https://tradier.com/individuals/pricing) | Ações/opções EUA | Parcial *(não verificado)* | US$ 0–35/mês | Melhor custo p/ opções via API |
| [TradeStation](https://developer.tradestation.com/trading-api/) | Ações/opções/futuros EUA | *(não verificado)* | Sem custo de API | Futuros via API |
| MT5 via [Genial](https://www.genialinvestimentos.com.br/trader/plataformas/meta-trader/) etc. | **B3** (WIN, WDO, ações) | Sim | Plataforma grátis (Genial) | Único caminho programável barato p/ B3; MQL5 proprietário |
| [Cedro Technologies](https://www.cedrotech.com/blog/roteamento-de-ordens-via-api-b3-bmf-e-bovespa/) | **B3** (dados + roteamento FIX/REST) | Sim (PF e PJ) | Centenas de R$/mês *(não verificado)* | Caminho "profissional" p/ B3 sem ser institucional |
| [SmarttBot](https://smarttbot.com/) | **B3** via corretoras (XP, BTG, Genial...) | Sim | R$ 99–1.399/mês | Automação sem programar; menos flexível p/ nosso caso |
| DMA 2–4 na B3 | B3 baixa latência | Contratos | Alto | Só faz sentido em estágio institucional |

**Achado importante:** **nenhuma grande corretora brasileira oferece API REST pública self-service de execução para pessoa física** ao estilo Alpaca. Para a B3, os caminhos reais são MT5, Cedro (dados+roteamento) ou plataformas licenciadas. Isso confirma a decisão do projeto: MVP na Alpaca (paper), B3 numa fase posterior via MT5/Cedro atrás do nosso `BrokerAdapter`.

### 4.2 Plataformas autônomas com IA

**[Standard Signal](https://www.ycombinator.com/companies/standard-signal)** (YC 2026) — "primeiro hedge fund onde a IA faz cada trade". *Fortes:* tese idêntica à nossa, com ênfase em explicabilidade/auditoria. *Fracos:* ~1 funcionário; alega Sharpe >3 **sem auditoria pública, período e AUM não divulgados** — ceticismo obrigatório; acesso só para investidores qualificados.

**[Numerai](https://numer.ai/)** — fundo quant alimentado por torneio de modelos crowdsourced. *Fortes:* o track record real mais documentado do segmento (2024: +25,45%, Sharpe 2,75; AUM US$ 60M→550M; JPMorgan comprometeu até US$ 500M). *Fracos:* 2023 negativo (−17%) e 2025 abaixo do benchmark — até o melhor caso do gênero oscila forte; participante não controla execução e arrisca token volátil.

**[Composer.trade](https://www.composer.trade/)** — estratégias no-code que executam sozinhas em conta real (EUA). *Fortes:* melhor UX; regulada; US$ ~24–32/mês. *Fracos:* **não aceita não residentes dos EUA**; automação de regras, não IA discricionária; backtests comunitários com sobrevivência/overfit.

**[QuantConnect](https://www.quantconnect.com/)** — pesquisa/backtest/deploy (motor LEAN open source) em 20+ corretoras. *Fortes:* padrão da indústria para quants independentes; utilizável do Brasil via IBKR/Alpaca. *Fracos:* sem B3; a "IA" é o que você programar; custos de nós somam.

**[Robinhood Agentic Trading](https://robinhood.com/us/en/support/articles/agentic-trading-overview/)** (beta mai/2026) — primeira grande corretora a deixar agentes LLM externos (Claude, ChatGPT...) **enviarem ordens reais**, em conta segregada com kill switch. *Fortes:* valida nosso padrão de salvaguardas (conta ring-fenced + kill switch + alertas). *Fracos:* só EUA; sem track record; responsabilidade segue com o usuário.

**[Q.ai (Forbes)](https://moneywise.com/investing/reviews/q-ai-review)** — **encerrada em dez/2023**. Lição: marketing de IA + marca forte não substituem track record; risco de continuidade em plataformas retail de IA é real.

**[Arcesium Intelligence](https://www.arcesium.com/press-release/arcesium-launches-comprehensive-ai-platform)** (D. E. Shaw spin-off, mai/2026) — IA agêntica para **operações** de fundos (reconciliação, dados), não para gerar alfa. Confirma que o institucional está usando agentes primeiro no back-office.

**[Alpha Arena / Nof1.ai](https://www.datawallet.com/crypto/alpha-arena-nof1-ai-explained)** (2025) — experimento público: 6 LLMs operando US$ 10k reais cada em cripto. Resultado: dispersão enorme (Qwen3 Max +22%; GPT-5 −60% ou pior; maioria perdeu). **A evidência pública mais transparente de que LLM "cru" sem motor de risco determinístico perde dinheiro** — exatamente o que nosso design evita.

**Agentes cripto (Virtuals, AIXBT, ElizaOS)** — bolha desinflada em 2026 (tokens −87% a −97% do pico); quase nenhum tinha alfa real. Cautela máxima com o gênero; e desconsiderar sites promocionais tipo "88% de retorno com IA" (marketing de afiliados sem auditoria).

---

## 5. Conclusões — onde nosso projeto se posiciona

1. **A lacuna existe e é tripla.** Nenhuma ferramenta pesquisada combina: (a) comitê de agentes LLM com debate e teses auditáveis, (b) sinal de pessoas (LinkedIn/Glassdoor) integrado ao processo, e (c) execução real com motor de risco determinístico. Os open source param antes da execução; as plataformas comerciais param antes do sinal; os provedores de dados param antes da decisão; as corretoras não têm inteligência.
2. **B3 é deserto competitivo.** Nenhuma plataforma de IA cobre a B3 com profundidade (filings CVM, português, execução). Para um fundo brasileiro, isso é simultaneamente a maior fricção (teremos que construir a ingestão CVM e a ponte MT5/Cedro) e a maior vantagem (ninguém está arbitrando esses sinais localmente — lembrando que 95% dos fundos promptam os mesmos LLMs com os mesmos dados públicos em inglês).
3. **Validações do nosso design pela evidência de mercado:** debate adversarial é o padrão que melhor performa (TradingAgents); "números por código, narrativa por LLM" (FinRobot) já está no nosso motor de risco; conta segregada + kill switch é o que a Robinhood adotou; e o Alpha Arena provou empiricamente que LLM sem veto determinístico perde dinheiro.
4. **Reusar em vez de construir:** estudar/fork de TradingAgents (Apache 2.0) para o comitê; Qlib para backtesting point-in-time; Alpaca para execução MVP; Bigdata.com (US$ 50–100/mês) como primeira fonte de sentimento com cobertura Brasil — deixando Revelio/LinkUp para depois que o sinal de pessoas provar valor.
5. **Ceticismo calibrado:** o único autônomo com track record plurianual documentado (Numerai) teve ano de −17%; o mais barulhento (Standard Signal) não tem auditoria. Nosso plano de gates com critérios a priori é mais conservador que o padrão do mercado — e deve continuar assim.

---

*Documento de projeto — v1. Pesquisa: jul/2026, via múltiplas fontes públicas citadas em cada seção.*
