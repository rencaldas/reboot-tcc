# Problem Definition: TCG de programação com catálogo automático como TCC

- **Slug**: tcg-programacao
- **Created**: 2026-09-27
- **Inputs used**: intake.md, research.md

## Problem Statement

Criar manualmente um catálogo de milhares de cartas de um TCG é inviável dentro do tempo de um TCC de graduação. Mesmo que fosse viável, um jogo pronto, por si só, não é uma contribuição de pesquisa academicamente avaliável — falta tanto um método reprodutível para gerar o conteúdo em escala quanto uma pergunta de pesquisa com critério de avaliação claro que a banca possa julgar.

## Affected Users & Stakeholders

- **Usuários — jogadores em potencial**: quem jogaria o TCG final; afetados indiretamente pela qualidade e legalidade do produto (raridade, boosters gratuitos, ausência de caminho pago). [NEEDS CLARIFICATION: nenhuma pesquisa de usuário foi feita — ver `research.md`, Users & Demand]
- **Usuários — estudantes de programação**: só relevantes se o enquadramento de pesquisa (b) (gamificação no ensino) for escolhido; afetados diretamente pelo valor educacional do jogo, não só pelo entretenimento.
- **Stakeholder — Renato de Azevedo Caldas (autor/mantenedor)**: decide escopo, arquitetura e cronograma; responsável final pela entrega e pela autoria do texto acadêmico.
- **Stakeholder — Prof. Erick da Silva Delvizio (orientador)**: aprova a pergunta de pesquisa e a metodologia; avalia o andamento academicamente.
- **Stakeholder — banca examinadora**: avalia o trabalho final; composição ainda não definida (`requisitos-academicos.md`).
- **Stakeholder — UVA (instituição)**: exige conformidade com o formato "Projeto Técnico" e com as normas de formatação aplicáveis.
- **Stakeholder regulatório — legislador/STF**: a Lei 15.211/2025 (ECA Digital) e a ADI 7999 em curso no STF (ver `research.md`) constrangem diretamente o design do sistema de boosters — este stakeholder não decide o projeto, mas pode mudar as regras do jogo (literalmente) antes da defesa.

## Goals

- Gerar um catálogo de milhares de cartas de forma automática, determinística e reproduzível, a partir de fontes de dados públicas (GitHub Linguist, stdlib `http`, DevDocs, Stack Exchange).
- Produzir uma contribuição acadêmica avaliável, alinhada a uma pergunta de pesquisa aprovada pelo orientador.
- Entregar um jogo jogável (web + app) em conformidade legal (ECA Digital, LGPD) até a data de defesa.
- Manter rastreabilidade do processo de desenvolvimento (SDD com IA) para alimentar o capítulo de Metodologia.

## Non-Goals

- Booster liberado por anúncio (fora do MVP, zona cinzenta — `PROMPT_TCC.md` §4.3).
- Troca ou venda de cartas por dinheiro real (fora do MVP, zona cinzenta — idem).
- Qualquer monetização além de cosméticos/passe de recompensas visíveis, e só se houver tempo depois do MVP.
- Avaliação de aprendizagem com pessoas, a menos que o enquadramento (b) seja formalmente escolhido — e nesse caso, sujeita a aprovação prévia do orientador e, possivelmente, do CEP (Resolução CNS 510/2016).
- Comparação direta e detalhada com o código do TechDex TCG ou outros precedentes — não há acesso a esse código, só à descrição pública (ver `research.md`).

## Success Metrics

- Distribuição de cartas por tipo e raridade, tempo de geração do pipeline, taxa de tags sem correspondência — métricas já previstas em `PROMPT_TCC.md` §9.3.6 (baseline: inexistente, pipeline ainda não implementado).
- Testes automatizados passando no domínio crítico — pipeline, raridade, RNG de booster, economia (baseline: 0%, nenhum código escrito ainda).
- Entrega dentro do prazo da UVA (baseline: datas ainda pendentes — `requisitos-academicos.md`).
- **Se o enquadramento (b) for escolhido**: instrumento de avaliação de aprendizagem aplicado e analisado (baseline: nenhum instrumento definido; qualitativo até existir um).

## Cost of Inaction

Sem automação do catálogo, Renato não consegue gerar milhares de cartas manualmente dentro do cronograma de um TCC de graduação — o produto simplesmente não existe em escala suficiente. Sem uma pergunta de pesquisa resolvida, o projeto corre o risco de ser julgado pela banca como "produto sem contribuição científica", independentemente da qualidade de engenharia. Sem tratar a conformidade legal (ECA Digital) desde o início, uma mudança de escopo tardia para remover mecânicas de booster já implementadas seria retrabalho caro.

## Open Questions

- [NEEDS CLARIFICATION: decisão final entre os enquadramentos de pesquisa (a) geração automática de catálogo, e (b) gamificação no ensino — ver `intake.md` e `research.md`.]
- [NEEDS CLARIFICATION: datas de entrega, coorientador e composição da banca — `requisitos-academicos.md`.]
- [NEEDS CLARIFICATION: se (b) for escolhido, qual instrumento de avaliação de aprendizagem, e se isso exige submissão ao CEP.]
- [NEEDS CLARIFICATION: como mitigar (ou se é preciso mitigar) o viés temporal da métrica de popularidade via Stack Overflow identificado em `research.md`.]
