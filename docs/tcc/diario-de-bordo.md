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

## 2026-09-27 — Fase 0.4 (opcional): avaliação da ideia (`/speckit-assess-*`)

Rodado fora de ordem em relação ao `PROMPT_TCC.md` (a avaliação de ideia é §5.4, Fase 0; a constituição já tinha sido ratificada como Fase 1) — sequência escolhida por Renato nesta sessão. Sem impacto: os artefatos são independentes.

**Decisões:**
- Pipeline completo `intake → research → define → shape → decide` executado para `slug=tcg-programacao`, artefatos em `.specify/assessments/tcg-programacao/`.
- Pesquisa web confirmou uma informação que antes só estava no `PROMPT_TCC.md` sem verificação: existe de fato uma ADI no STF contra o ECA Digital (**ADI 7999**, partido Missão, relator Min. Luiz Fux, contestando a proibição de loot boxes e a verificação de idade biométrica).
- Pesquisa também encontrou 4 precedentes diretos de "jogo de cartas + programação" já existentes no Brasil (TechDex TCG, AlgoCards, Cartas Programáveis/SP, Logirunner) — nenhum combina geração automática de catálogo em escala com avaliação acadêmica do pipeline, o que sustenta o enquadramento de pesquisa (a) como diferencial.
- Veredito: **go**, qualificado — recomendada a Option B do `concept.md` (pipeline de catálogo como núcleo + jogo mínimo jogável), que serve tanto ao enquadramento de pesquisa (a) quanto (b), sem bloquear o `/speckit-specify` de `catalogo-cartas` enquanto a decisão entre (a) e (b) continua pendente com o orientador.

**Problemas encontrados:**
- Mesma limitação da constituição: skills `speckit-assess-*` instaladas mas não reconhecidas pelo tool de skills da sessão; contornado seguindo manualmente cada `SKILL.md`.
- Tentativa de acessar `portal.stf.jus.br` diretamente (fonte primária da ADI) retornou HTTP 403; a confirmação da ADI 7999 ficou em fontes jornalísticas jurídicas (Conjur, Gazeta do Povo), não no inteiro teor do processo — registrado como confidence "medium" em `research.md`.

**Tempo gasto:** não cronometrado.

**Métricas:** N/A (avaliação de ideia, não implementação).
