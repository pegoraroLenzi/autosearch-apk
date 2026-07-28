# AI-Native Hedge Fund — Fundamentação Teórica

> Base bibliográfica do projeto: da matemática aos casos de empresas, passando por finanças, gestão e comportamento humano. **Critério de inclusão rigoroso** (§1) e uma seção explícita do que foi **deliberadamente excluído** por fragilidade (§9). Documento de projeto — acompanha `PROJETO-AI-NATIVE-HEDGE-FUND.md`.

---

## 1. Critério de confiabilidade (o filtro anti-fontes-duvidosas)

Uma referência só entra nesta base se atender a pelo menos um destes critérios:

1. **Peer review em periódico de primeira linha** (Journal of Finance, Journal of Financial Economics, Econometrica, American Economic Review) ou prêmio Nobel/equivalente ligado ao trabalho.
2. **Texto canônico com décadas de citação acadêmica** e uso continuado em programas de pós-graduação sérios.
3. **Jornalismo investigativo com apuração documentada** (para os estudos de caso), de autores com reputação verificável.

E é **excluída ou marcada com ressalva** se: depende de amostras selecionadas a posteriori (viés de sobrevivência), teve resultados que falharam em replicação, envolve autor com fraude comprovada, ou é literatura de "guru" sem base empírica. As ressalvas estão sinalizadas com ⚠️ ao longo do texto — incluir um livro com ressalva significa "leia sabendo o limite", não "confie cegamente".

---

## 2. Fundamentos matemáticos

A matemática que sustenta o fundo, na ordem em que importa:

| Área | Referência canônica | Por que entra |
|---|---|---|
| **Probabilidade** | Kolmogorov, *Foundations of the Theory of Probability* (1933); Feller, *An Introduction to Probability Theory and Its Applications* (1950/68) | Toda decisão do fundo é uma aposta probabilística; a axiomatização de Kolmogorov é o alicerce de tudo que vem depois |
| **Estatística e inferência** | Casella & Berger, *Statistical Inference* (1990); Hastie, Tibshirani & Friedman, *The Elements of Statistical Learning* (2001) | Separar sinal de ruído é o trabalho central; ESL é a ponte canônica estatística→machine learning |
| **Séries temporais** | Box & Jenkins, *Time Series Analysis: Forecasting and Control* (1970); Hamilton, *Time Series Analysis* (1994); Tsay, *Analysis of Financial Time Series* (2002) | Preços são séries temporais não estacionárias; Tsay é o padrão para as particularidades financeiras (caudas gordas, volatilidade condicional/GARCH) |
| **Teoria da informação** | Shannon, *A Mathematical Theory of Communication* (1948); Kelly, *A New Interpretation of Information Rate* (1956) | O **critério de Kelly** — dimensionar aposta pela vantagem informacional — é a base matemática do position sizing do nosso motor de risco (usamos Kelly fracionado) |
| **Otimização e álgebra linear** | Boyd & Vandenberghe, *Convex Optimization* (2004) | Otimização de portfólio é um problema de otimização convexa com restrições |
| **Aritmética e teoria dos números** | Hardy & Wright, *An Introduction to the Theory of Numbers* (1938); Knuth, *The Art of Computer Programming*, vol. 2 (aritmética seminumérica) | Honestidade intelectual: teoria dos números pura tem aplicação **indireta** em finanças — ela sustenta a criptografia (RSA/curvas elípticas, que protegem as chaves da corretora), geradores de números pseudoaleatórios (simulações de Monte Carlo) e aritmética de ponto flutuante correta (Knuth). É fundação da infraestrutura, não fonte de sinal de mercado |

> **Nota de projeto:** a tentação de buscar "padrões numerológicos" (Fibonacci, Gann, números "mágicos") em preços é folclore sem validação estatística — está na lista de exclusões (§9). A matemática que gera alpha comprovadamente é probabilidade, estatística e otimização.

---

## 3. Teoria de finanças e investimento

### 3.1 A linhagem clássica (base dos agentes Fundamentalista e Quant)

- **Graham & Dodd, *Security Analysis* (1934)** e **Graham, *The Intelligent Investor* (1949)** — a fundação da análise fundamentalista: valor intrínseco, margem de segurança, "Mr. Market". É literalmente o prompt-base do nosso Agente Fundamentalista.
- **Markowitz, "Portfolio Selection" (*Journal of Finance*, 1952)** — Nobel 1990. Risco medido como variância; diversificação como única "refeição grátis". Base do nosso portfólio-alvo.
- **Sharpe, "Capital Asset Prices" (*Journal of Finance*, 1964)** — Nobel 1990. CAPM, beta, e o índice de Sharpe que usamos como métrica de gate no plano de validação.
- **Fama, "Efficient Capital Markets" (*Journal of Finance*, 1970)** — Nobel 2013. A hipótese dos mercados eficientes é o **adversário teórico** do projeto: todo sinal que buscamos é uma aposta de que a eficiência é imperfeita em alguma margem. Conhecer a EMH a fundo é o que impede otimismo ingênuo.
- **Fama & French, "The Cross-Section of Expected Stock Returns" (*Journal of Finance*, 1992)** — modelos de fatores (valor, tamanho, depois momentum/qualidade). Base do Agente Técnico/Quant.
- **Black & Scholes (1973); Merton (1973)** — Nobel 1997. Precificação de derivativos; entra como fundamento de gestão de risco (hedge), não como estratégia principal.
- **Grinold & Kahn, *Active Portfolio Management* (1995)** — a "lei fundamental da gestão ativa": IR ≈ IC × √breadth. Tradução direta para o nosso projeto: alpha vem de **pequena vantagem preditiva aplicada a muitas decisões independentes** — argumento matemático para cobrir muitos ativos com agentes, em vez de poucas apostas concentradas.

### 3.2 Finanças comportamentais (base do Agente de Sentimento)

- **Kahneman & Tversky, "Prospect Theory" (*Econometrica*, 1979)** — Nobel 2002. Aversão a perdas, enquadramento: por que mercados reagem exageradamente. É a justificativa teórica de existir sinal em sentimento.
- **Shiller, "Do Stock Prices Move Too Much...?" (*AER*, 1981)** e ***Irrational Exuberance* (2000)** — Nobel 2013. Volatilidade excessiva e narrativas; complementa com *Narrative Economics* (2019): notícias e histórias movem preços — exatamente o que nosso pipeline de notícias explora.
- **Thaler, *Misbehaving* (2015)** — Nobel 2017. Síntese da economia comportamental por seu fundador.
- ⚠️ **Taleb, *Fooled by Randomness* (2001) e *The Black Swan* (2007)** — entram pelas ideias centrais (sobrevivência ao ruído, caudas gordas, fragilidade de modelos), que são sólidas e ecoam literatura técnica séria. Ressalva: estilo polêmico e afirmações não quantificadas; usar como vacina intelectual, não como método.

---

## 4. Machine learning financeiro e validação de estratégias

A subárea mais crítica para este projeto — é onde a maioria dos fundos quantitativos amadores morre:

- **López de Prado, *Advances in Financial Machine Learning* (2018)** — o texto de referência sobre por que ML "de manual" falha em finanças: purged cross-validation, importância de features, e o **Deflated Sharpe Ratio**. Leitura obrigatória antes do nosso Gate 1.
- **Bailey, Borwein, López de Prado & Zhu, "The Probability of Backtest Overfitting" (*Journal of Computational Finance*, 2017)** — formaliza matematicamente o que nosso plano de validação chama de "número limitado de rodadas de ajuste": quanto mais configurações você testa, mais o melhor backtest é ilusão.
- **Harvey, Liu & Zhu, "…and the Cross-Section of Expected Returns" (*Review of Financial Studies*, 2016)** — demonstra que a maioria dos "fatores" publicados não sobrevive a correção estatística por múltiplos testes. Justifica nosso ceticismo de gates com critérios definidos a priori.
- **Chan, *Quantitative Trading* (2008)** — prático e honesto sobre a distância entre backtest e produção; complemento operacional aos anteriores.

---

## 5. Estratégia e gestão de empresas (base dos agentes Fundamentalista e de Concorrência)

Os "gurus" filtrados pelo critério do §1 — entram os que criaram frameworks analíticos testados pelo tempo:

- **Porter, *Competitive Strategy* (1980) e *Competitive Advantage* (1985)** — as Cinco Forças e a cadeia de valor são **o framework operacional do nosso Agente de Concorrência**: poder de fornecedores/clientes, ameaça de entrantes e substitutos, rivalidade. Academicamente robusto (base em economia de organização industrial).
- **Drucker, *The Practice of Management* (1954) e *The Effective Executive* (1967)** — o fundador da gestão moderna; base para avaliar qualidade de gestão das empresas analisadas.
- **Christensen, *The Innovator's Dilemma* (1997)** — disrupção: por que incumbentes racionais perdem para entrantes. Sinal-chave para teses short em setores em transição (baseado em pesquisa doutoral em Harvard, com estudo de caso da indústria de discos rígidos).
- **Deming, *Out of the Crisis* (1982)** — qualidade e melhoria contínua por variação estatística; curiosamente, o mesmo raciocínio estatístico que aplicamos ao próprio fundo (atribuição de performance por agente).
- **Mintzberg, *The Rise and Fall of Strategic Planning* (1994)** — o contraponto acadêmico: estratégia emergente vs. planejada. Vacina contra ler guidance de empresas ao pé da letra.
- **Kaplan & Norton, "The Balanced Scorecard" (*Harvard Business Review*, 1992)** — indicadores além do financeiro; espelha nossa tese de que sinais não financeiros (pessoas, clientes, processos) antecipam o resultado financeiro.
- ⚠️ **Collins, *Good to Great* (2001)** — entra **somente com ressalva grave**: a metodologia sofre de viés de sobrevivência e efeito halo (ver Rosenzweig abaixo); várias empresas "great" decaíram depois (Circuit City faliu, Fannie Mae colapsou em 2008). Os conceitos (ex.: "Level 5 leadership") são hipóteses interessantes, não leis.
- **Rosenzweig, *The Halo Effect* (2007)** — o antídoto metodológico para toda a literatura de gestão: demonstra como atribuímos qualidades à gestão a partir do resultado (e não o contrário). **Leitura obrigatória antes de qualquer livro de "empresas excelentes"** — e um alerta direto para o nosso Agente de Pessoas: reviews de Glassdoor também sofrem efeito halo do desempenho da ação.

### 5.1 Planejamento estratégico (base do Agente de Planejamento Estratégico)

O cânone acadêmico da disciplina, na ordem histórica — cada obra vira um "instrumento de leitura" que o agente aplica às empresas analisadas:

- **Chandler, *Strategy and Structure* (1962)** — o estudo histórico fundador (MIT/Harvard, baseado em DuPont, GM, Sears, Standard Oil): "a estrutura segue a estratégia". Instrumento: quando a estrutura organizacional de uma empresa contradiz a estratégia anunciada, a estratégia é discurso.
- **Ansoff, *Corporate Strategy* (1965)** — o pai do planejamento estratégico formal; a Matriz de Ansoff (produto×mercado) classifica o risco de qualquer movimento de crescimento anunciado (penetração < desenvolvimento < diversificação). Instrumento: precificar o risco de execução de cada expansão anunciada em fato relevante.
- **Andrews / Learned et al., *Business Policy* (Harvard, 1965)** — origem acadêmica do que popularizou-se como SWOT: adequação entre competências internas e ambiente. Citado como raiz histórica; o agente usa as versões rigorosas modernas (VRIO, Cinco Forças), não o SWOT de slide.
- **Porter, *Competitive Strategy* (1980) / *Competitive Advantage* (1985)** — já listado acima; para este agente, o instrumento específico é o teste de **consistência do posicionamento**: empresa "presa no meio" (nem custo, nem diferenciação) é red flag estrutural.
- **Hamel & Prahalad, "The Core Competence of the Corporation" (*Harvard Business Review*, 1990)** — competências centrais como raiz da vantagem; diversificações longe da competência central historicamente destroem valor. Instrumento: avaliar aderência de M&A anunciado à competência central do comprador.
- **Barney, "Firm Resources and Sustained Competitive Advantage" (*Journal of Management*, 1991)** — a Visão Baseada em Recursos (RBV) e o framework **VRIO** (Valioso, Raro, Inimitável, Organizado): o teste mais rigoroso de sustentabilidade de vantagem competitiva. É o critério formal do agente para responder "esse moat é real?" — complementar e mais operacional que o conceito informal de moat de Buffett.
- **Teece, Pisano & Shuen, "Dynamic Capabilities and Strategic Management" (*Strategic Management Journal*, 1997)** — em setores em transformação, o que importa não é o recurso que a empresa tem, mas a capacidade de reconfigurá-lo. Instrumento: nos setores sob disrupção (cruzamento com Christensen), avaliar a capacidade adaptativa demonstrada, não o portfólio atual.
- **Wack, "Scenarios: Uncharted Waters Ahead" (*Harvard Business Review*, 1985)** — o planejamento por cenários da Shell (que a preparou para o choque do petróleo de 1973); aprofundado por **Schoemaker, "Scenario Planning: A Tool for Strategic Thinking" (*Sloan Management Review*, 1995)**. É a metodologia formal da saída de cenários do agente: cenários não são previsões, são estruturas para testar em quais futuros a tese sobrevive.
- **Rumelt, *Good Strategy / Bad Strategy* (2011)** — o filtro de qualidade: estratégia de verdade tem diagnóstico, política orientadora e ação coerente; "visões" com metas sem diagnóstico são *bad strategy*. Instrumento direto para ler investor days e planos plurianuais — empresa cuja "estratégia" é uma lista de aspirações recebe score baixo, por mais bonito que seja o deck.
- **Brandenburger & Nalebuff, *Co-opetition* (1996)** — teoria dos jogos aplicada a estratégia (sobre a base de von Neumann & Morgenstern, 1944, e Schelling, *The Strategy of Conflict*, 1960 — Nobel 2005): rede de valor, complementadores, e antecipação de reação competitiva. Instrumento: prever a resposta dos concorrentes a um movimento anunciado (guerra de preço destrói a tese?).
- **Mintzberg, *The Rise and Fall of Strategic Planning* (1994)** — já listado acima; para este agente é a **ressalva estrutural**: planejamento formal não é estratégia, e estratégias reais são parcialmente emergentes. O agente pondera menos o plano publicado e mais o padrão revelado de decisões (alocação de capital efetiva vs. discurso).
- ⚠️ **Kim & Mauborgne, *Blue Ocean Strategy* (2005)** — citado apenas com ressalva: útil como vocabulário para inovação de valor, mas a metodologia sofre das mesmas críticas de seleção retrospectiva de casos que *Good to Great* (só analisa vencedores). Não usar como critério de score.

**Síntese operacional do agente:** ler a estratégia declarada com Rumelt (é estratégia ou aspiração?), testar a vantagem com Barney/VRIO e Porter (é sustentável?), conferir coerência com Chandler e com a alocação de capital real (Mintzberg: o padrão revelado > o discurso), precificar movimentos com Ansoff e Hamel & Prahalad, antecipar reações com teoria dos jogos, e devolver tudo ao comitê como **cenários à la Shell/Wack** — em quais futuros a tese sobrevive e o que a invalida.

### 5.2 Bases teóricas setoriais — pré-requisito por empresa analisada

**Regra do projeto (`PROJETO...md` §4.1): nenhuma empresa entra no universo analisável sem um Dossiê Setorial aprovado** — uma base teórica forte sobre o mercado em que ela atua. Esta seção define o alicerce comum e o padrão de fontes de cada dossiê.

**Fundamento transversal (vale para todo setor):**

- **Tirole, *The Theory of Industrial Organization* (1988)** — Nobel 2014. A teoria rigorosa de como estruturas de mercado (monopólio, oligopólio, competição) determinam preços, margens e poder de mercado. É a base científica por trás das Cinco Forças.
- **Besanko, Dranove, Shanley & Schaefer, *Economics of Strategy* (1996, sucessivas edições)** — o texto acadêmico padrão que conecta economia industrial à análise prática de setores; template intelectual do conteúdo "economia do setor" de cada dossiê.
- **Damodaran (NYU Stern)** — datasets públicos e material de valuation **por setor** (margens, betas, múltiplos por indústria, atualizados anualmente e com metodologia aberta): [pages.stern.nyu.edu/~adamodar](https://pages.stern.nyu.edu/~adamodar/). Referência de benchmarks setoriais verificáveis.
- **Porter, "How Competitive Forces Shape Strategy" (*HBR*, 1979)** — a aplicação setorial das Cinco Forças, refeita dossiê a dossiê.

**Padrão de fontes por setor (critério do §1 aplicado a mercados):**

| Tipo de fonte | Exemplos (Brasil / global) | Status |
|---|---|---|
| Regulador oficial do setor | BCB e relatórios de estabilidade financeira (bancos); ANEEL/ONS/EPE (energia); ANP (óleo & gás); Anatel (telecom); ANS (saúde suplementar); CVM (mercado de capitais) | **Preferencial** — dados primários, metodologia pública |
| Organismos internacionais | BIS/Basileia (bancos), IEA (energia), USGS (mineração), FAO/USDA (agro), OMS (saúde) | **Preferencial** |
| Estatísticas oficiais | IBGE, séries do BCB (SGS), FRED, Eurostat | **Preferencial** |
| Portais e diários oficiais de governo (federal, estadual, municipal) | gov.br, Agência Brasil, DOU, diários oficiais dos estados/municípios das sedes e operações das companhias | **Preferencial** — atos oficiais são fonte primária (tributos, licenças, concessões) |
| Dados da economia mundial (camada global do §4.3 do projeto) | FMI (World Economic Outlook), Banco Mundial, OCDE, BIS | **Preferencial** — peso menor que o setorial, nunca zero; base histórica de 30 anos (convenção para dados globais) |
| Literatura acadêmica do setor | Journals de economia/finança aplicada; handbooks setoriais (ex.: *Handbook of Banking*, Oxford) | **Preferencial** |
| Associações setoriais com metodologia pública | ABRAINC/CBIC (construção), ANFAVEA (autos), ABIQUIM (química), FEBRABAN (bancos) | Aceitável — checar conflito de interesse (associação defende o setor) |
| Research de bancos/consultorias | Relatórios setoriais de research | Aceitável como fonte secundária — nunca única |
| "Market reports" vendidos sem metodologia (mercado de US$ X bi crescendo Y%...) | Relatórios genéricos de firmas de pesquisa de mercado sem amostra/método divulgado | **Excluído** (mesmo critério do §9) |

**Exemplo do padrão aplicado (setor bancário, o primeiro dossiê do universo inicial):** economia do setor via *Handbook of Banking* e Besanko; métricas e riscos via arcabouço de Basileia (BIS) e Relatório de Estabilidade Financeira do BCB; benchmarks de NIM/ROE via Damodaran e dados públicos do BCB (IF.data); estrutura competitiva via Tirole/Porter aplicados à concentração bancária brasileira; regulação via normativos CMN/BCB. Cada setor novo do universo repete esse gabarito antes de o primeiro sinal ser emitido.

**Racional final:** Buffett chama isso de círculo de competência; nós o tornamos **verificável e bloqueante** — o círculo de competência do fundo é, literalmente, o conjunto de dossiês setoriais aprovados no banco de dados.

---

## 6. Casos reais e lições aprendidas (a "jurisprudência" do projeto)

Jornalismo investigativo e casos documentados — cada um mapeado à lição que o nosso design incorpora:

| Obra | Caso | Lição incorporada no projeto |
|---|---|---|
| Lowenstein, *When Genius Failed* (2000) | LTCM (1998): dois Nobéis, modelos impecáveis, falência sistêmica | Modelos elegantes + alavancagem + caudas gordas = ruína. Justifica: limites duros de exposição, circuit breaker, e desconfiança de correlações históricas |
| Mallaby, *More Money Than God* (2010) | História completa da indústria de hedge funds (apuração de anos, bem documentada) | O que separa os sobreviventes: gestão de risco e adaptação, não a "estratégia genial" inicial |
| Zuckerman, *The Man Who Solved the Market* (2019) | Renaissance/Medallion | O padrão-ouro do quant: sinais fracos × milhares de apostas × execução impecável (a lei de Grinold-Kahn na prática); e o valor do sigilo sobre os sinais |
| McLean & Elkind, *The Smartest Guys in the Room* (2003) | Enron | Demonstrações financeiras mentem antes de quebrar; sinais não contábeis (saída de executivos, cultura) antecipam — **validação histórica do Agente de Pessoas** |
| Carreyrou, *Bad Blood* (2018) | Theranos | Idem: funcionários sabiam antes do mercado. Reviews internos e êxodo de técnicos como sinal antecedente |
| Lewis, *The Big Short* (2010) | Crise de 2008 | A tese contrária certa exige paciência e sobrevivência até o mercado reconhecer — por isso nossas teses têm gatilho de invalidação explícito, não só de saída |
| SEC, *In the Matter of Knight Capital* (Release 34-70694, 2013) | Knight Capital (2012): US$ 460 mi perdidos em 45 min por deploy defeituoso de software de trading | **O caso mais importante para nós**: automação de execução sem kill switch testado, sem reconciliação e sem controle de deploy destrói uma empresa em minutos. Fonte primária oficial, gratuita, leitura obrigatória do time |

---

## 7. Gestão de pessoas e comportamento humano (base científica do Agente de Pessoas)

Esta é a seção que transforma "olhar LinkedIn e Glassdoor" de intuição em estratégia baseada em evidência peer-reviewed:

### 7.1 A evidência empírica central — satisfação de funcionários prediz retorno de ações

- **Edmans, "Does the Stock Market Fully Value Intangibles? Employee Satisfaction and Equity Prices" (*Journal of Financial Economics*, 101, 2011, pp. 621–640)** — o paper fundador: portfólio das "100 Best Companies to Work For" gerou **alpha de ~3,5% ao ano (1984–2009)**, controlando por fatores de risco. A metodologia usa retornos (não lucros) como variável dependente justamente para tratar causalidade reversa. Conclusão: o mercado **não precifica integralmente** a satisfação dos funcionários — existe alpha em dados de pessoas. ([SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=985735); [LBS](https://lbsresearch.london.edu/id/eprint/387/))
- **Edmans, Li & Zhang (CEPR/ECGI, versão global do estudo)** — extensão internacional com nuance importante para nós: o efeito é mais forte em mercados de trabalho **flexíveis**; em mercados rígidos, o sinal enfraquece. Implicação direta: calibrar o peso do sinal de pessoas por país (Brasil tem mercado de trabalho mais rígido que os EUA). ([CEPR](https://cepr.org/voxeu/columns/employee-satisfaction-and-firm-value-global-perspective))
- **Green, Huang, Wen & Zhou, "Crowdsourced Employer Reviews and Stock Returns" (*Journal of Financial Economics*, 134(1), 2019)** — a ponte para a nossa fonte específica: **mudanças em avaliações do Glassdoor** predizem retornos e surpresas de lucro. É a validação acadêmica de que o dado que planejamos comprar contém sinal.
- **Harter, Schmidt & Hayes, "Business-Unit-Level Relationship Between Employee Satisfaction, Employee Engagement, and Business Outcomes: A Meta-Analysis" (*Journal of Applied Psychology*, 2002)** — meta-análise (padrão-ouro de evidência) ligando engajamento a resultados operacionais; o elo micro que explica o efeito macro de Edmans.

### 7.2 Teoria organizacional e de pessoas (o "porquê" por trás do sinal)

- **Schein, *Organizational Culture and Leadership* (1985)** — o framework acadêmico de cultura organizacional (artefatos → valores → pressupostos). Dá ao Agente de Pessoas vocabulário para classificar o que lê em reviews.
- **Lazear & Gibbs, *Personnel Economics in Practice* (2009)** — economia de pessoal: incentivos, turnover e produtividade tratados com rigor econômico (Lazear foi pioneiro do campo em Stanford).
- **Pfeffer, *The Human Equation* (1998)** — evidência de práticas de gestão de pessoas → desempenho; acadêmico de Stanford, base empírica documentada.
- ⚠️ **Herzberg ("motivação-higiene") e Maslow (hierarquia de necessidades)** — citados apenas como contexto histórico: são frameworks intuitivos cuja validação empírica é fraca/mista. Não usar como base de decisão; a base de decisão é §7.1.

### 7.3 Comportamento e decisão humana (para entender o mercado e a nós mesmos)

- **Kahneman, *Thinking, Fast and Slow* (2011)** — síntese de décadas de pesquisa premiada. ⚠️ Ressalva honesta: os capítulos sobre *priming* social se apoiam em estudos que falharam na crise de replicação (o próprio Kahneman reconheceu publicamente); o núcleo (heurísticas, vieses, prospect theory) permanece sólido.
- **Cialdini, *Influence* (1984)** — persuasão com base experimental; útil para o agente de sentimento reconhecer narrativas manipulativas em notícias e comunicados.
- **Meehl, *Clinical versus Statistical Prediction* (1954)** — o clássico esquecido que fundamenta o projeto inteiro: previsões estatísticas simples batem julgamento de especialistas na maioria dos domínios estudados. É o argumento científico mais antigo e replicado a favor de decisão sistematizada sobre discricionária — confirmado meio século depois por Tetlock, *Expert Political Judgment* (2005) e *Superforecasting* (2015), que adiciona o perfil do bom previsor: atualização incremental, pensamento probabilístico, accountability — exatamente o comportamento que projetamos nos agentes.

---

## 8. Mapa: de cada base teórica para cada componente do sistema

| Componente do projeto | Bases que o sustentam |
|---|---|
| Agente Fundamentalista | Graham & Dodd; Porter; Drucker; Christensen |
| Agente de Sentimento | Kahneman & Tversky; Shiller (narrativas); Cialdini |
| Agente Técnico/Quant | Fama-French (fatores); Grinold & Kahn; Tsay |
| Agente de Pessoas | **Edmans 2011; Green et al. 2019; Harter 2002**; Schein; Lazear; Rosenzweig (antídoto) |
| Agente de Concorrência | Porter (Cinco Forças); Christensen |
| Agente de Planejamento Estratégico | Rumelt; Barney (VRIO); Chandler; Ansoff; Hamel & Prahalad; Teece; Wack/Schoemaker (cenários); Brandenburger & Nalebuff; Mintzberg (ressalva) |
| Comitê / PM | Meehl; Tetlock; Grinold & Kahn (breadth) |
| Motor de risco | Kelly (sizing); Markowitz; Taleb (caudas); LTCM como caso-limite |
| Backtesting / Gates | López de Prado; Bailey et al.; Harvey et al.; Fama (EMH como hipótese nula) |
| OMS / Execução | Caso Knight Capital (SEC 2013) |
| Melhoria contínua do fundo | Deming; atribuição por agente |

---

## 9. Excluídos deliberadamente (e por quê)

Em cumprimento ao pedido de "desprezar bases duvidosas ou questionáveis":

| Fonte/categoria | Motivo da exclusão |
|---|---|
| **Dan Ariely** (*Predictably Irrational* etc.) | Fraude de dados comprovada em estudo central sobre honestidade (caso do seguro, 2021); campo contaminado — usar Kahneman/Thaler no lugar |
| **Peters & Waterman, *In Search of Excellence*** | Viés de sobrevivência clássico; das 43 empresas "excelentes", boa parte decaiu em poucos anos; o próprio caso que motivou *The Halo Effect* |
| **Análise técnica folclórica** (Fibonacci, Gann, Elliott Waves, padrões gráficos sem teste) | Sem validação estatística replicável; o que sobrevive a teste rigoroso já está capturado em fatores (momentum) na literatura de Fama-French/Jegadeesh-Titman |
| **Literatura de "guru de trading" de varejo** (fórmulas de enriquecimento, day-trade "infalível") | Evidência acadêmica direta em contrário: Barber, Lee, Liu & Odean, "Do Individual Day Traders Make Money?" — a esmagadora maioria perde |
| **Napoleon Hill, PNL aplicada a vendas/gestão, e afins** | Sem qualquer base empírica verificável; Hill inclusive com biografia fraudulenta documentada |
| **Numerologia de mercado** (astros, ciclos místicos, "números mágicos") | Autoexplicativo dado o critério do §1 |
| *Good to Great* como "lei" | Rebaixado a "hipótese com ressalva" (§5), não excluído — mas nunca citado pelos agentes como evidência |

---

## 10. Prioridade de leitura (se a equipe só puder ler oito)

1. López de Prado — *Advances in Financial Machine Learning* (evita a morte nº 1: backtest overfit)
2. Lowenstein — *When Genius Failed* (evita a morte nº 2: alavancagem e caudas)
3. SEC — caso Knight Capital (evita a morte nº 3: automação sem controles)
4. Edmans 2011 + Green et al. 2019 (a evidência do nosso sinal diferencial)
5. Grinold & Kahn — *Active Portfolio Management* (a matemática do porquê "muitos sinais fracos")
6. Rosenzweig — *The Halo Effect* (o filtro para tudo que os agentes lerem sobre empresas)
7. Tetlock — *Superforecasting* (o comportamento que queremos nos agentes)
8. Graham — *The Intelligent Investor* (a fundação que não envelhece)

---

*Documento de projeto — v1. Referências verificadas contra fontes públicas em jul/2026; papers citados com periódico/ano para conferência independente.*
