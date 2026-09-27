# Idea Intake: TCG web/app de tema de programação com catálogo gerado automaticamente

- **Slug**: tcg-programacao
- **Created**: 2026-09-27
- **Source**: pasted text — texto original de Renato, preservado em `PROMPT_TCC.md` §3
- **Type**: new-capability

## Idea (as captured)

> "É possível fazer um game web e app de um jogo de TCG, voltado para programação? O usuario recebe cartinhas através de Boosters, e terá radidades de cartas de R até UR, LR será uma raridade mais rara ainda. Ajustar raridade por importancia de carta, exemplo: Python, C, Java (UR) algo assim. R (funções de cada linguagem, tipo printf(), scan, etc) Terá cartas de tudo que envolve programação, tipo Erro 404, Erro 403, Post, Get. etc. Entendeu? Como podemos fazer? Existe uma api com todos esses dados, para criar automaticamente cada carta? seria muito demorado criar uma carta um por um. Me mostre um metodo que consuma todos os dados pra criar milhares de cartas pro TCG."

## Restated

Construir um jogo de cartas colecionáveis (TCG), web e app, cujo tema é o universo da programação — linguagens, funções, códigos de status HTTP, métodos HTTP, entre outros elementos técnicos. Jogadores obtêm cartas por meio de boosters; a raridade de cada carta reflete a importância do elemento que ela representa. Como criar cada carta manualmente é inviável em escala (milhares de cartas), o catálogo deve ser gerado automaticamente a partir de fontes de dados públicas.

## Origin & Context

- **Raised by**: Renato de Azevedo Caldas, graduando em Ciência da Computação (UVA).
- **Trigger**: necessidade de um TCC (Projeto Técnico) que combine um produto de software com uma contribuição/questão de pesquisa academicamente válida.

## Decisões já tomadas antes deste intake (contexto, não parte da ideia crua)

Para não perder o trabalho já feito na Fase 0/1 deste projeto, registro aqui o que já foi decidido e que as próximas etapas do assessment (`research`, `define`, `shape`, `decide`) devem tratar como restrição, não como algo a redescobrir:

- Fontes de dados já mapeadas: GitHub Linguist, stdlib `http` do Python, DevDocs (sem API oficial, cache obrigatório), Stack Exchange API (rate-limited). Ver `PROMPT_TCC.md` §4.1.
- Regras de raridade por percentil de popularidade (contagem de perguntas no Stack Overflow), com faixa fixa de raridades permitidas por tipo de carta, e LR como curadoria manual. Ver `PROMPT_TCC.md` §4.1.
- Restrição legal não-negociável: boosters 100% gratuitos, sem caminho pago, RNG server-side (Lei 15.211/2025 — ECA Digital). Ver `PROMPT_TCC.md` §4.3 e Princípio VI da constituição.
- Mecânica de batalha em linhas gerais: elementos com efetividade tipo pedra-papel-tesoura (mínimo 6, temáticos de programação), vida/ataque, passivas de raridade SR+ ligadas ao ataque (ex.: carta SSR do HTTP 401 impede o oponente de jogar por 1 turno).
- Constituição de engenharia já ratificada (10 princípios, `.specify/memory/constitution.md` v1.0.0).

## First-Glance Unknowns

- [NEEDS CLARIFICATION: qual enquadramento de pergunta de pesquisa a banca vai aceitar — (a) geração automática de catálogo com avaliação quantitativa, ou (b) gamificação no ensino de programação com avaliação de aprendizagem? Isso muda o que a etapa `define` deste assessment deve considerar "sucesso".]
- [NEEDS CLARIFICATION: a escala de raridade é exatamente R → SR → SSR → UR → LR, ou há mais graus? Usada informalmente nas decisões já tomadas, mas nunca confirmada explicitamente.]
- [NEEDS CLARIFICATION: como funciona uma partida — estrutura de turnos, condição de vitória, deckbuilding? Só a mecânica de dano/elemento/passiva foi decidida, não a partida como um todo.]
- [NEEDS CLARIFICATION: o jogo pretende ensinar programação de fato, ou só usa o tema/estética? Depende diretamente da decisão da pergunta de pesquisa acima.]
- [NEEDS CLARIFICATION: qual stack técnica (backend, frontend, mobile) — ainda é proposta, não decisão, ver `PROMPT_TCC.md` §7.3.]

## Próximo passo sugerido

`/speckit-assess-research slug=tcg-programacao` — a etapa de pesquisa pode buscar: TCGs e plataformas educativas de programação existentes (trabalhos relacionados), evidência sobre uso de popularidade do Stack Overflow como métrica, e precedentes de disputas legais sobre loot boxes no Brasil (ADI no STF mencionada em `PROMPT_TCC.md` §4.3).
