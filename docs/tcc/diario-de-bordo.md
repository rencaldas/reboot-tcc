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
