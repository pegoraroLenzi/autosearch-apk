# AI-Native Hedge Fund — documentos de projeto

Projeto de um fundo de investimento **AI-native** para a B3 (small/mid caps negligenciadas), operado por um gestor único com agentes de IA no núcleo do processo — em fase de **definição de escopo concluída, pré-implementação**.

## Ordem de leitura

| # | Documento | O que é |
|---|---|---|
| 1 | [`MANDATO.md`](MANDATO.md) | Documento-mãe: estrutura (PF, capital próprio), long-only, réguas (CDI piso + Ibovespa), drawdown 20%, vol-alvo 18%, orçamento de 15h/semana do gestor |
| 2 | [`FUNDAMENTACAO-TEORICA.md`](FUNDAMENTACAO-TEORICA.md) | As bases acadêmicas e práticas de cada componente do desenho |
| 3 | [`PROJETO-AI-NATIVE-HEDGE-FUND.md`](PROJETO-AI-NATIVE-HEDGE-FUND.md) | Arquitetura completa: fontes e ingestão, agentes, livros, comitê, calibração, motor de risco, execução, governança, custos, riscos e fraquezas |
| 4 | [`IMPLEMENTACAO-VALIDACAO-GOLIVE.md`](IMPLEMENTACAO-VALIDACAO-GOLIVE.md) | Como construir e validar: 10 sprints (dados primeiro), pirâmide de testes, evals, 3 gates, checklist de go-live (~13 meses) |
| 5 | [`REVISAO-CRITICA.md`](REVISAO-CRITICA.md) | As 11 críticas estruturais + Red-team #1 — todas com decisão do gestor registrada |
| 6 | [`PREPARACAO-CONTEUDO.md`](PREPARACAO-CONTEUDO.md) | Trabalho humano pré-código: template dos dossiês setoriais, shortlist do universo, especificação dos golden sets |
| 7 | [`REVISAO-EXTERNA-PACOTE.md`](REVISAO-EXTERNA-PACOTE.md) | Pacote pronto para a revisão por outra família de IA (Red-team #2, pendente) |
| — | [`ANALISE-COMPETITIVA.md`](ANALISE-COMPETITIVA.md) | Análise competitiva de apoio |

**Estado atual:** escopo fechado e internamente consistente; próximo passo é o Sprint 1 (ligar o arquivo point-in-time + taxonomia B3 + screening do universo inicial: 2 setores, ~10 empresas).

---

*Arquivo não relacionado ao projeto: [AutoSearch.apk](AutoSearch.apk?raw=1) (download direto).*
