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

---

## 2026-09-27 — Fase 2: feature `001-catalogo-cartas` (em andamento: specify e clarify)

**Decisões:**
- Ordem do backlog de features (`PROMPT_TCC.md` §7) confirmada pelo autor sem alteração: `catalogo-cartas` → `contas-e-colecao` → `boosters` → `arte-cartas` → `partida` → `app-mobile` → `conteudo-educativo`.
- Atributos da carta: ataque, defesa e HP; dano crítico e chance de crítico ficam como extensão futura. Toda carta tem um elemento; cartas SR+ têm passiva única.
- `/speckit-specify` e `/speckit-clarify` concluídos: 14 clarificações e 12 premissas confirmadas, todas registradas na seção `Clarifications` de `specs/001-catalogo-cartas/spec.md`. As de maior impacto:
  - textos (descrições e passivas) gerados por IA nesta feature, com revisão individual das cartas SR+ e por amostragem (10% por lote) das descrições R;
  - seis elementos (Lógica, Dados, Rede, Sistema, Segurança, Interface), atribuídos por curadoria manual, por regra temática (status HTTP pela classe) ou por distribuição equilibrada;
  - sem volume mínimo total de cartas: a 1ª versão publicada precisa ter pelo menos as 20 mais populares de cada tipo;
  - a passiva pode ser corrigida até a feature `partida` convergir, e depois congela.
- Branch da feature: `feat/001-catalogo-cartas`. O Spec Kit não criou a branch numerada sozinho porque a extensão `git` não está instalada, então o nome combina o prefixo do Princípio X com o número do Spec Kit.

**Problemas encontrados:**
- A decisão de gerar os textos por IA (Q2 = B) entrava em conflito com a reprodutibilidade do catálogo (FR-017), porque a saída de IA não é determinística. Resolvido na própria spec (FR-036): o texto gerado passa a ser entrada armazenada e nunca é gerado de novo automaticamente.
- Skills `speckit-*` de novo não reconhecidas pela sessão, que foi aberta na pasta pai `C:\Progamer\TCC`; contornado seguindo os `SKILL.md` manualmente.

**Tempo gasto:** não cronometrado.

**Métricas:** spec com 5 user stories, 43 requisitos funcionais (FR-001 a FR-040, mais FR-006a, FR-015a e FR-020a) e 14 critérios de sucesso.
