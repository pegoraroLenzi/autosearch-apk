# Revisão Crítica — O que eu faria diferente

> **Status de tratamento:** ponto 6 atendido (`MANDATO.md`); pontos 1–2 atendidos (§5.2 do projeto — meta-modelo com features qualitativas obrigatórias, cold start por pesos iguais + shrinkage, conflito assimétrico); ponto 3 atendido com decisão do gestor (§4.2 e §5.1 — cobertura em dois níveis aprovada; o histórico de 10 anos vira **contexto de compreensão, não fator determinante**: transparência via Ficha de Consistência de Dados no lugar de bloqueio ou modulação mecânica; IPOs entram com coleta pré-IPO obrigatória); ponto 4 atendido (arquivo point-in-time perpétuo desde o dia 1, pré-registro obrigatório de previsões falsificáveis em `predictions`, paper trading estendido para 6 meses com track record prospectivo como critério do Gate 2 — linha do tempo total ~12 meses). Demais pontos pendentes de decisão, um a um.
>
> Crítica estruturada do projeto como está após todas as decisões acumuladas. Não são ajustes cosméticos: são pontos onde, na minha avaliação, o design atual contradiz a própria base teórica que adotamos, subestima um problema técnico, ou deixa uma decisão fundamental sem dono. Cada ponto traz o fundamento (teórico ou técnico), o problema concreto e o que eu mudaria. Ordenado por gravidade.

---

## 1. O comitê viola Meehl — a agregação final é "clínica", não estatística

**O problema.** Colocamos Meehl (*Clinical versus Statistical Prediction*, 1954) na fundamentação como "o argumento científico mais antigo e replicado a favor de decisão sistematizada". Mas olhe o que o nosso comitê faz: sete agentes emitem scores, e um **agente PM decide por debate e síntese argumentativa**. Isso é exatamente o que Meehl chama de agregação clínica — um julgador (humano ou LLM) combinando evidências por raciocínio verbal. A literatura de 70 anos é consistente: a combinação **mecânica** de preditores (uma regressão simples, até com pesos unitários — Dawes, "The Robust Beauty of Improper Linear Models", *American Psychologist*, 1979) bate a síntese julgamental na maioria dos domínios. Nós citamos a lei e a infringimos no órgão central do sistema.

**O que eu faria.** Inverter os papéis: o debate multi-agente serve para **gerar e refinar evidência** (checar consistência, produzir a tese escrita, achar o furo), mas o **score final que dimensiona a posição sai de um meta-modelo estatístico** — regressão regularizada ou gradient boosting raso sobre os scores dos agentes + features duras (valuation, momentum, qualidade), treinado contra retornos realizados e re-estimado com disciplina de walk-forward. O PM-LLM escreve a tese e propõe; o número que vira ordem vem do modelo. Isso também resolve um problema prático: scores de LLM não são comparáveis entre setores nem estáveis entre versões de prompt — o meta-modelo absorve essas não-estacionariedades como um problema de calibração.

## 2. Scores de LLM não são probabilidades — falta a camada de calibração

**O problema.** O sistema inteiro opera com `score −5..+5 + confiança` emitidos por LLMs. Isso é escala ordinal com rótulo de cardinal: nada garante que "confiança 0,8" do Agente de Pessoas signifique a mesma coisa que 0,8 do Fundamentalista, nem que 0,8 corresponda a 80% de acerto histórico. Position sizing por Kelly (que adotamos) é **extremamente sensível à calibração da probabilidade** — Kelly com probabilidade superestimada não é subótimo, é ruína acelerada.

**O que eu faria.** Camada de calibração formal entre agentes e motor de risco: para cada agente, mapear score→probabilidade com **regressão isotônica** (ou Platt scaling) ajustada nos resultados realizados, e medir continuamente **Brier score e curvas de confiabilidade por agente e por setor** — é a operacionalização quantitativa do Tetlock que citamos. Regra dura: agente sem histórico de calibração suficiente entra com peso encolhido (shrinkage) em direção a zero. O plano de evals atual mede acurácia direcional; acurácia sem calibração é inutilizável para sizing.

## 3. Os pré-requisitos empilhados destroem o breadth — e a nossa própria lei fundamental avisa

**O problema.** Adotamos Grinold & Kahn como base: IR ≈ IC × √breadth. Depois empilhamos gates que reduzem breadth brutalmente: dossiê setorial aprovado por humano + 10 anos de listagem (exclui toda a safra de IPOs 2020–21 da B3 — algo como um terço das listadas) + peer set coberto + fornecedores mapeados. O universo viável converge para 15–30 nomes de setores maduros. Com breadth ~20 e decisões correlacionadas dentro de poucos setores, o IC exigido para um Sharpe decente é altíssimo — justamente o que sinais de LLM sobre dados públicos dificilmente entregam (95% dos fundos promptando os mesmos modelos, como registramos na análise competitiva). E há um viés estrutural: o gate de 10 anos cria uma carteira permanentemente inclinada a value/old-economy — o fundo monitora disrupção pelo radar, mas está **estruturalmente proibido de possuir os disruptores**.

**O que eu faria.** Duas mudanças: (a) **cobertura em dois níveis** — o universo profundo (com todos os gates) para os livros de longo prazo/núcleo, mais uma **camada sistemática rasa e larga** (fatores clássicos: valor, momentum, qualidade — só dados de preço e fundamentos, sem dossiê) sobre 100+ ativos, com orçamento de risco próprio, para comprar breadth barato onde profundidade não é necessária; (b) transformar os gates binários em **score de completude de dados** (0–100) que modula o limite de posição continuamente — empresa com 7 anos de histórico pode entrar com limite reduzido em vez de ser invisível. Gate binário só onde é indefensável operar sem (tese escrita, stop, dossiê do setor).

## 4. O backtest prometido é inexecutável para metade das fontes — validação deve ser prospectiva

**O problema.** O Gate 1 exige walk-forward com corte temporal rígido. Mas não existe arquivo point-in-time de: portais regionais, diários municipais, reviews de Glassdoor (deltas), mapa de fornecedores reconstituído, radar de disrupção. Para essas fontes, o backtest ou é impossível ou será silenciosamente contaminado (reconstrução retrospectiva = look-ahead disfarçado). O plano atual dá ao backtest um peso epistêmico que ele não pode sustentar — exatamente o erro contra o qual López de Prado e o Look-Ahead-Bench alertam, agravado pelo look-ahead paramétrico do próprio LLM.

**O que eu faria.** Rebalancear a validação: (a) backtest walk-forward **apenas** para o que tem point-in-time real (preços, fundamentos CVM, fatos relevantes, sentimento licenciado com histórico); (b) para as fontes sem histórico, **validação prospectiva com pré-registro**: desde o Sprint 1, cada sinal novo grava previsão falsificável com prazo ("este sinal implica surpresa negativa no ITR de X em ≤2 trimestres") e o sistema acumula um track record prospectivo auditável — metodologia de Tetlock aplicada literalmente; (c) **arquivar tudo desde o dia 1** (o pipeline já coleta — basta nunca descartar) para que o backtest completo se torne possível daqui a 2–3 anos. Consequência honesta: o paper trading precisa ser mais longo (6–12 meses, não 60–90 dias) porque está carregando o peso que o backtest não pode carregar.

## 5. Não existe modelo de risco de fatores — limites por posição não controlam risco correlacionado

**O problema.** O motor de risco tem limites por posição/setor/livro, vol targeting e VaR. Nada disso impede o modo de falha clássico: dez teses "independentes" que são todas, no fundo, a mesma aposta (juros caindo, ou BRL, ou China). LTCM — nosso próprio caso-escola — não quebrou por falta de limites por posição, quebrou por correlação escondida entre posições "diversificadas".

**O que eu faria.** Modelo de fatores para o portfólio: decomposição das posições em exposições a fatores (mercado, juros/duration implícita, BRL, commodities, valor/momentum/qualidade — via regressão das ações nos fatores, estilo Barra simplificado ou PCA estatístico), com **limites sobre as exposições líquidas a fatores**, não só sobre nomes; stress tests determinísticos usando a base global de 30 anos que já exigimos (2008, 2015–16 Brasil, 2020) como cenários de choque obrigatórios no relatório diário. Sem isso, os cinco livros podem estar todos comprados no mesmo fator sem que ninguém veja.

## 6. O mandato do fundo nunca foi definido — e tudo depende dele

**O problema.** Decidimos arquitetura, dados, agentes, livros — mas não decidimos **o que o fundo é**: long-only ou long/short? Benchmark (CDI? Ibov? IPCA+?) — os gates dizem "Sharpe ≥ 0,8 e retorno > benchmark" sem fixar qual; vol-alvo; capacidade (a análise relativa do §4.4 clama por pares long/short, mas short na B3 tem custo de aluguel e squeeze que nunca discutimos); tributação (day-trade vs. posição, come-cotas se virar fundo, 15% PF em swing — a estrutura tributária muda o giro ótimo dos livros de curto prazo). Sem mandato, os orçamentos por livro (0–5%, 40–60%...) são números soltos.

**O que eu faria.** Antes do Sprint 1, um documento de mandato de uma página, decidido pelo humano, com: estrutura legal (PF/PJ/fundo), long-only ou long/short (minha recomendação: começar long-only com caixa como posição defensiva — short amplifica todos os riscos de modelo), benchmark e horizonte de avaliação, vol-alvo, capacidade estimada, e regras tributárias que condicionam o giro. Todos os gates numéricos passam a referenciar esse documento.

## 7. A ingestão é ~60% do projeto real e o cronograma trata como 20%

**O problema técnico mais subestimado.** Entity linking por CNPJ através de tribunais, diários municipais (sem feed, formatos caóticos), notas explicativas em PDF para extrair fornecedores e receita por geografia, deduplicação em português — isso é a maior parte do esforço de engenharia, e o plano de sprints dá a ela ~2 sprints. NLP jurídico/contábil em português é difícil; extração de "receita por geografia" de notas explicativas não padronizadas é um projeto em si.

**O que eu faria.** (a) Re-sequenciar: Sprints 1–4 quase inteiramente dados (o "agente" da fase 1 é um relatório burro em cima de dados limpos — valor nasce da limpeza, não do agente); (b) usar infraestrutura existente em vez de construir: **Querido Diário** (Open Knowledge Brasil — raspagem de diários oficiais municipais, open source), **DataJud/Comunica CNJ** (API pública), dados abertos CVM (ITR/DFP estruturados desde 2010); (c) um **golden set de entity linking** (CNPJ↔razões sociais↔nomes de pregão↔apelidos de imprensa) como o primeiro eval do projeto — se o linking erra, todos os sinais a jusante estão errados e nenhum eval de agente detecta.

## 8. Monocultura de modelo — o advogado do diabo com o mesmo cérebro não é adversarial

**O problema.** Todos os agentes, incluindo o cético do debate, rodam sobre a mesma família de LLM. Erros de LLM são correlacionados dentro da mesma família (mesmos dados de treino, mesmos vieses): o "advogado do diabo" tende a comprar os mesmos pressupostos errados que o proponente — o debate vira teatro de discordância superficial. A literatura de ensembles é inequívoca: o ganho vem de **diversidade de erros**, não de quantidade de votantes.

**O que eu faria.** Heterogeneidade obrigatória nos papéis adversariais: o refutador e pelo menos um verificador rodam em **família de modelo diferente** do proponente; e ao menos um verificador **não-LLM** por tese — um checklist determinístico de invariantes (números da tese batem com o banco? evidências existem e estão na janela temporal? a tese contradiz alguma posição viva sem reconciliação?). Barato e imune a alucinação correlacionada.

## 9. Atribuição de performance por fonte de dado — com critério de morte

**O problema.** Fomos acumulando fontes (Glassdoor, judicial, diários municipais, patentes, fornecedores...) cada uma com custo de licença, engenharia e manutenção. O plano tem atribuição por agente e por livro, mas **não por fonte** — logo nunca saberemos se o dossiê judicial gera alpha ou só custo, e fontes nunca morrem (só se acumulam). Isso é o oposto da disciplina de Deming que citamos.

**O que eu faria.** Cada sinal já carrega `evidências[]` → propagar a atribuição até a fonte: P&L marginal estimado por fonte de dado (via ablação no meta-modelo do ponto 1: quanto o IC cai sem aquela família de features). Revisão semestral com **critério de morte explícito**: fonte que por 2 ciclos não paga seu custo total (licença + manutenção + tokens) é desligada — e o TCO por empresa coberta (hoje implícito e crescente) vira número acompanhado no relatório mensal.

## 10. O livro núcleo (40–60% do PL) tem o ciclo de revisão mais fraco — e um catraca comportamental embutido

**O problema.** A maior alocação (retenção geral) tem a menor cadência de revisão (trimestral) e um caminho de entrada ("promoção") sem simétrico rigoroso de saída. Isso institucionaliza dois vieses que a nossa própria fundamentação manda combater: **endowment effect** (Kahneman/Thaler — o que já está na carteira é julgado com régua mais frouxa) e **efeito halo** (Rosenzweig — empresa que performou vira "empresa de qualidade" e a tese para de ser testada).

**O que eu faria.** **Re-underwriting zero-based anual** para todo o núcleo: uma vez por ano, cada posição núcleo é reavaliada como se fosse compra nova, por um agente **sem acesso ao fato de que já é posição** (blind re-analysis — tecnicamente trivial com a separação de bases do §4.7: basta montar o contexto sem o estado de portfólio). Se a análise cega não recomendaria comprar hoje, o comitê é obrigado a justificar a manutenção por escrito ou rebaixar. O custo é um dia de análise por ano; o benefício é a única defesa estrutural contra o envelhecimento silencioso do núcleo.

## 11. O humano é o gargalo não dimensionado

**O problema.** Aprovação de dossiês, exceções de IPO, alçadas de ordem, revisão de 10% das teses, re-underwriting — tudo converge para um gestor humano cuja capacidade nunca foi orçada. Sem SLA, o sistema trava silenciosamente (dossiê parado = setor invisível) ou o humano vira carimbo (aprovação sem leitura — pior que não ter gate, porque gera falsa segurança).

**O que eu faria.** Orçamento explícito de horas humanas por semana no documento de mandato; SLA por tipo de aprovação com **default conservador** no vencimento (dossiê sem revisão em 10 dias úteis → volta ao autor, nunca aprovação tácita; ordem acima da alçada sem resposta em N horas → não executa); e medir o *rubber-stamp rate* — taxa de aprovação sem alteração — como indicador de que o gate humano degenerou.

---

## Síntese — os três movimentos que eu faria primeiro

1. **Escrever o mandato** (ponto 6) — é a decisão-mãe que está faltando e muda os números de todos os outros documentos.
2. **Meta-modelo estatístico + calibração** (pontos 1–2) — é a mudança de arquitetura com maior respaldo teórico: o debate gera evidência, a estatística decide o tamanho. Sem isso, o sistema é um comitê eloquente dimensionando posições com números não calibrados.
3. **Arquivo point-in-time + validação prospectiva desde o dia 1** (ponto 4) — é irreversível: cada dia sem arquivar é um dia de backtest futuro perdido. Tudo o mais pode ser adicionado depois; isso não.

*Documento de revisão — v1. Crítica produzida deliberadamente contra o design vigente; os pontos acatados devem ser incorporados aos documentos principais um a um, com a mesma disciplina de versionamento.*
