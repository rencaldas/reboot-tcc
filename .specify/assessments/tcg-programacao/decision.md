# Decision: TCG de programação com catálogo automático como TCC

- **Slug**: tcg-programacao
- **Decided**: 2026-09-27
- **Verdict**: go
- **Artifacts reviewed**: intake.md, research.md, problem.md, concept.md

## Scorecard

| Criterion | Rating | Justification |
|-----------|--------|---------------|
| Problem validity | strong | Criar milhares de cartas manualmente é genuinamente inviável no prazo de um TCC, e "jogo pronto sem pergunta de pesquisa" é um risco acadêmico real e documentado (`problem.md`). |
| Evidence strength | adequate | Prior art (4 projetos independentes), corroboração legal (ADI 7999 confirmada) e literatura de gamificação foram encontrados e citados (`research.md`); mas não há pesquisa de demanda direta, e a métrica central (Stack Overflow) tem viés documentado — não chega a "strong". |
| Value vs. inaction | strong | Sem automação, o produto não existe em escala; sem resolver a pergunta de pesquisa, o TCC arrisca reprovação por falta de contribuição — o custo de não agir é existencial para a entrega. |
| Feasibility / appetite fit | adequate | Option B (recomendada) cabe num cronograma de TCC, mas depende de suposições ainda não validadas (volume de dados suficiente, rate limits de fonte — ver `concept.md`, Assumptions to Validate). |
| Strategic fit | strong | Alinhado aos 10 princípios da constituição já ratificada e ao backlog já esboçado em `PROMPT_TCC.md` §7. |
| Risk posture | adequate | O risco legal (ADI 7999 no STF) está identificado e a mitigação de design (booster 100% gratuito, sem depender de interpretação favorável da lei) é conservadora; mas o risco não está eliminado — depende de um julgamento futuro do STF fora do controle do projeto. O viés de métrica (Stack Overflow) está identificado, mas ainda sem mitigação de design definida. |

## Verdict & Rationale

**Go**, mas qualificado. Problem validity e value vs. inaction são fortes — este projeto não é opcional para o TCC de Renato, e a alternativa (criar cartas manualmente, ou não resolver a pergunta de pesquisa) tem custo alto e conhecido. Evidence strength e feasibility são "adequate", não "strong": há evidência real e citada, mas com lacunas honestas (sem pesquisa de demanda, métrica de popularidade com viés documentado, suposições de volume de dados ainda não testadas). Isso é suficiente para o gate de `go` (exige `adequate+` em ambos, nunca `weak`/`unknown`), mas não para tratar o projeto como livre de risco.

A pergunta de pesquisa final — enquadramento (a) geração automática de catálogo vs. (b) gamificação no ensino — continua sem decisão. Isso **não bloqueia** o `go`: a Option B recomendada em `concept.md` foi escolhida justamente por servir a ambos os enquadramentos (o pipeline de catálogo é pré-requisito de qualquer um dos dois), então a primeira feature do backlog (`catalogo-cartas`) pode ser especificada agora sem esperar essa decisão. Mas a decisão **precisa** ser tomada com o orientador antes de escrever a Introdução/Justificativa da monografia — carregada para frente como questão aberta.

## If go — Handoff to `/speckit-specify`

- **Problem**: criar manualmente milhares de cartas de TCG é inviável no prazo de um TCC, e um jogo pronto por si só não é contribuição de pesquisa avaliável sem um método reprodutível de geração de conteúdo e uma pergunta de pesquisa com critério de avaliação claro.
- **Chosen approach**: Option B — pipeline de geração de catálogo como núcleo do projeto (com testes, relatório de distribuição, ADRs), acompanhado de um jogo mínimo jogável (booster, coleção, partida básica com elementos/HP/ataque/passivas). PWA instalável cobre a exigência de "app"; sem troca/chat entre jogadores no MVP.
- **In scope / out of scope**: ver `concept.md` → Out of Scope. Fora do MVP: motor de jogo de terceiros, avaliação de aprendizagem formal, app mobile nativo em loja, troca/chat entre jogadores, booster por anúncio, venda de cartas por dinheiro real.
- **Success metrics**: distribuição de cartas por tipo/raridade, tempo de geração, taxa de tags sem correspondência, testes automatizados do domínio crítico passando, entrega dentro do prazo da UVA (ver `problem.md` → Success Metrics).
- **Carried-forward open questions**:
  - [NEEDS CLARIFICATION: decisão final da pergunta de pesquisa (a) vs (b) — resolver com o orientador antes da Introdução/Justificativa, não antes do specify de `catalogo-cartas`.]
  - [NEEDS CLARIFICATION: volume real de dados disponível nas fontes públicas é suficiente para "milhares" de cartas dentro dos filtros de qualidade — só se confirma implementando o pipeline.]
  - [NEEDS CLARIFICATION: mitigação (se houver) do viés temporal da métrica Stack Overflow identificado em `research.md`.]
  - [NEEDS CLARIFICATION: datas de entrega, coorientador, banca — `docs/tcc/requisitos-academicos.md`.]

**Próximo passo:** `/speckit-specify` para a feature `catalogo-cartas`, usando o argumento já pronto em `PROMPT_TCC.md` §7.1.
