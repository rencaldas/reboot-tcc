# Diário de bordo

Uma entrada por fase/feature: decisões tomadas, problemas encontrados, tempo gasto e métricas relevantes. Alimenta o capítulo de Metodologia da monografia/documentação final (ver `PROMPT_TCC.md`, seções 7 e 9.3.4).

---

## 2026-09-27 — Fase 0: requisitos acadêmicos e setup

**Decisões:**
- Formato do trabalho: Projeto Técnico (aceito pela UVA).
- Orientador definido: Prof. Erick da Silva Delvizio.
- Repositório GitHub `tcg-dev` criado como **privado** (decisão do autor: proteger rascunhos e nomes de banca/orientador até divulgação deliberada).
- Mecânica de batalha do TCG decidida em linhas gerais: sistema de elementos com efetividade tipo pedra-papel-tesoura (mínimo 6 elementos temáticos de programação), vida/ataque, e passivas de raridade SR+ ligadas ao ataque (exemplo: carta SSR do status HTTP 401 Unauthorized impede o oponente de jogar por 1 turno). Detalhes completos ficam para o `/speckit-clarify` da feature `partida`.
- Script type do Spec Kit: `sh` (Git Bash disponível no ambiente Windows do autor).

**Pendências levantadas** (ver `requisitos-academicos.md` para a lista completa): coorientador, datas de entrega, banca, limite de páginas, manual da UVA, política formal de IA, decisão final da pergunta de pesquisa, PDFs das normas ABNT.

**Problemas encontrados:**
- `specify init` em modo interativo (sem `--non-interactive`) travou esperando input de teclado que o ambiente não conseguia fornecer; resolvido reexecutando com `--non-interactive --script sh`.

**Tempo gasto:** não cronometrado nesta sessão (retroativo a partir do histórico de mensagens).

**Métricas:** N/A nesta fase (sem código ainda).

---

## 2026-09-27 — Fase 1: Constituição

**Decisões:**
- Constituição ratificada em `.specify/memory/constitution.md`, v1.0.0, com os 10 princípios de `PROMPT_TCC.md` §6, sem alteração de conteúdo (Fase 0 não trouxe nada que exigisse ajuste nos princípios de engenharia).
- Seções opcionais do template padrão do Spec Kit (Additional Constraints / Development Workflow) omitidas deliberadamente — conteúdo já coberto pelos Princípios III, VI, IX e X.
- A partir desta fase, o próprio Princípio X passa a valer para o processo: a constituição foi commitada em branch `docs/constitution` e aberta como PR (#1), não direto na `main`.

**Problemas encontrados:**
- A skill `speckit-constitution` estava instalada em `.claude/skills/` do projeto, mas o tool de skills da sessão não a reconheceu (skill list carregada antes do projeto existir). Contornado seguindo manualmente as instruções do `SKILL.md` (resolver template, preencher placeholders, escrever Sync Impact Report).

**Tempo gasto:** não cronometrado.

**Métricas:** N/A.

---

## 2026-09-27 — Correção: `build_cards.py` não existe

A seção 4.1 original do `PROMPT_TCC.md` afirmava como fato já decidido que havia uma PoC (`build_cards.py`) a ser referenciada em `reference/poc/`. Renato confirmou que esse arquivo nunca existiu de fato ("era um arquivo teste") — a suposição do prompt estava errada. Removida toda menção a ele do `PROMPT_TCC.md` (título e corpo da seção 4.1, árvore de estrutura da seção 5.3). As regras de fontes/raridade/atributos da seção 4.1 permanecem válidas — não dependiam do arquivo, só a referência de implementação que não existe mais. O pipeline da feature `catalogo-cartas` será desenhado do zero a partir dessas regras, sem código de referência prévio.
