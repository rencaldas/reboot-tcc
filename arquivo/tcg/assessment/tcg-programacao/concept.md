# Concept: TCG de programação com catálogo automático como TCC

- **Slug**: tcg-programacao
- **Created**: 2026-09-27
- **Recommended option**: B — MVP focado no catálogo, jogo mínimo jogável

## Options

### Option A — Motor de terceiros + catálogo automático
- **Sketch**: Usar um framework de regras de TCG já existente e testado (ex.: um motor open-source tipo o `kata-tcg` encontrado no research, ou similar) para a camada de partida/regras, e concentrar todo o esforço de engenharia próprio na geração automática do catálogo (pipeline, raridade, atributos) e na camada de conformidade legal (boosters, RNG, LGPD). A mecânica de elementos/HP-ataque/passivas já decidida seria implementada como configuração/dados sobre esse motor, não como um motor customizado do zero.
- **Appetite**: medium
- **Trade-offs**: ganha tempo e reduz risco de bugs de regra de jogo (motor já testado por terceiros); sacrifica controle fino sobre a mecânica específica já decidida (elementos, passivas de raridade SR+) e adiciona uma dependência de licença/manutenção de um projeto de terceiro que pode não ser mantido.
- **Rabbit holes**: adaptar um motor genérico de TCG para encaixar a mecânica de elementos + passivas condicionadas por raridade pode acabar exigindo tanto retrabalho quanto construir do zero, se o motor escolhido for rígido demais nesse ponto — e não há garantia disso até tentar.

### Option B — MVP focado no catálogo, jogo mínimo jogável
- **Sketch**: O pipeline de geração de catálogo é o núcleo do projeto (com testes, relatório de distribuição, ADRs), acompanhado de um jogo construído do zero mas deliberadamente simples — só o suficiente para provar o catálogo em uso real: abrir booster, ver coleção, uma partida básica (PvE ou PvP simplificado) usando os elementos/HP/ataque/passivas já decididos. Sem app mobile nativo robusto (PWA instalável basta), sem troca de cartas ou chat entre jogadores no MVP.
- **Appetite**: medium-to-large (a maior parte do tempo disponível até a defesa)
- **Trade-offs**: ganha foco total na contribuição acadêmica mais defensável (pipeline + avaliação quantitativa do catálogo, enquadramento a); sacrifica profundidade de gameplay — a "partida" fica deliberadamente rasa, o que já é aceito pelo próprio `PROMPT_TCC.md` §7 ("Pode ficar reduzida no MVP").
- **Rabbit holes**: a tentação de aprofundar demais a mecânica de partida além do que o cronograma permite; integração com fontes de dados externas sem API oficial (DevDocs) e com rate limit (Stack Exchange) pode consumir mais tempo do que o previsto.

### Option C — Produto completo + avaliação de aprendizagem
- **Sketch**: Tudo do Option B, mais um instrumento formal de avaliação de aprendizagem (ex.: pré/pós-teste) aplicado a estudantes reais, com TCLE e possível submissão ao CEP, para sustentar uma contribuição de "gamificação no ensino de programação" (enquadramento b) com dados empíricos, não só com a existência do jogo.
- **Appetite**: large
- **Trade-offs**: ganha uma contribuição de pesquisa com dados empíricos sobre aprendizagem, potencialmente mais impactante academicamente; sacrifica tempo (recrutamento, aprovação ética, aplicação, análise estatística) e depende de terceiros fora do controle de Renato (CEP, disponibilidade de participantes).
- **Rabbit holes**: aprovação ética pode não terminar a tempo do cronograma do TCC; validade estatística de um estudo pequeno e de curto prazo (típico de TCC de graduação) é um alvo fácil de crítica pela banca; a mecânica de booster/raridade (mesmo gratuita) pode enviesar a percepção de "aprendizagem" vs. "diversão/coleção" nos resultados.

## Recommendation

**Option B.** É a que melhor aproveita o que já está resolvido tecnicamente (regras de raridade, fontes de dados mapeadas, constituição ratificada) e se alinha ao enquadramento de pesquisa (a), que depende menos de terceiros fora do controle de Renato (CEP, participantes, prazos de aprovação ética) do que o enquadramento (b) — ver `research.md`, "Evidence Against the Idea", sobre a literatura de gamificação não ser unânime e exigir instrumento de avaliação rigoroso. Option B não fecha a porta para (b): se o orientador preferir esse enquadramento, o mesmo MVP pode ganhar a camada de avaliação de aprendizagem depois (evoluindo para Option C), sem descartar o trabalho já feito. Option A foi descartada como recomendação principal porque a mecânica de elementos/HP-ataque/passivas já foi decidida de forma bem específica (ver `PROMPT_TCC.md` e o diário de bordo) — adaptar um motor genérico de terceiros a essa mecânica específica é um rabbit hole real, com ganho de tempo incerto.

## Out of Scope (for the recommended option)

- Tudo já listado em `problem.md` → Non-Goals (booster por anúncio, troca/venda por dinheiro real, monetização além de cosméticos/passe).
- Motor de jogo de terceiros — decidido construir a camada de partida sob controle próprio, mesmo que mínima.
- Avaliação de aprendizagem formal com pessoas — só entra se o orientador exigir o enquadramento (b), promovendo o projeto para Option C.
- App mobile nativo em loja — PWA instalável é o suficiente para o MVP.
- Troca de cartas ou chat entre jogadores — features que exigiriam as salvaguardas do art. 21 do ECA Digital, fora do MVP.

## Assumptions to Validate

- O orientador aceita o enquadramento (a) — geração automática de catálogo com avaliação quantitativa — como pergunta de pesquisa válida para o Projeto Técnico da UVA.
- O volume de dados das fontes públicas (Linguist, DevDocs, Stack Exchange) é suficiente para gerar "milhares" de cartas dentro dos filtros de qualidade (mínimo de perguntas por tipo) já no MVP, sem precisar das fontes candidatas (Wikidata, MDN) ainda.
- Um PWA instalável é suficiente para atender à exigência de "app" do projeto, sem precisar de loja de app nativa dentro do prazo até a defesa.
- O rate limit do Stack Exchange (~300/dia sem key, 10 mil/dia com key gratuita) é suficiente para gerar e regenerar o catálogo repetidamente durante o desenvolvimento sem gargalo.
- Uma "partida básica" (PvE ou PvP simplificado) é suficiente para demonstrar o catálogo em uso real perante a banca, sem exigir deckbuilding ou balanceamento competitivo completo.
