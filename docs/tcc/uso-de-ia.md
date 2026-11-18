# Registro de uso de IA generativa

Registro de cada uso de IA generativa (Claude Code) no desenvolvimento deste TCC: o que a IA fez, em qual seção/fase, e como Renato revisou. Este registro atende à política da UVA sobre uso de IA (ver `requisitos-academicos.md`, item 5 — ainda pendente de confirmação formal) e vira declaração no capítulo de Metodologia da monografia/documentação final.

Formato de cada entrada: data, fase/feature, o que foi feito, o que a IA gerou vs. o que o autor decidiu, como foi revisado.

---

## 2026-09-27 — Fase 0 (setup)

**O que foi feito:** Claude Code (modelo Sonnet 5, via extensão VSCode) executou o setup inicial do projeto a partir do roteiro em `PROMPT_TCC.md`:
- Instalação do `specify-cli` (Spec Kit) via `uv`.
- `specify init tcg-dev --integration claude --script sh --non-interactive`.
- Instalação das extensões `bug` e `assess` do Spec Kit.
- Criação da estrutura de diretórios proposta na seção 5.3 do `PROMPT_TCC.md`.
- Redação de `CLAUDE.md` (cópia literal das regras da seção 2 do `PROMPT_TCC.md`) e de `README.md`/`ATTRIBUTIONS.md` iniciais.
- Registro das respostas de Renato às perguntas de requisitos acadêmicos (seção 5.1) em `requisitos-academicos.md`.

**Decisões do autor (não da IA):** todas as respostas de conteúdo em `requisitos-academicos.md` (orientador, formato do trabalho, pendências, pergunta de pesquisa) vieram diretamente de Renato, em texto livre no chat — a IA apenas organizou e registrou. A visibilidade do repositório GitHub (privado) e a mecânica de batalha do jogo (elementos, vida/ataque, passivas por raridade) também foram decisões e ideias do autor, transcritas pela IA.

**Revisão:** Renato acompanhou a execução dos comandos em tempo real na sessão e revisou o conteúdo de cada arquivo gerado antes do commit inicial.

---

## 2026-09-27 — Fase 2, feature `001-catalogo-cartas`: specify e clarify

**O que foi feito:** Claude Code (modelo Opus 5.5, via extensão VSCode) executou `/speckit-specify` e `/speckit-clarify` seguindo manualmente os `SKILL.md` do Spec Kit. As skills não foram carregadas pela sessão, que foi aberta na pasta pai do repositório.
- Redigiu `specs/001-catalogo-cartas/spec.md` a partir do argumento do `PROMPT_TCC.md` §7.1 e do complemento do autor sobre atributos, elementos e passivas.
- Gerou o checklist de qualidade da spec (`checklists/requirements.md`), que é parte automática do `/speckit-specify` e não um checklist customizado do autor.
- Formulou as perguntas de clarificação: 2 no specify e 12 no clarify, cada uma com opções e uma recomendação. Listou também 12 premissas para o autor validar.
- Integrou as respostas à spec (seção `Clarifications` e requisitos afetados).

**Decisões do autor (não da IA):** todas as respostas. Em algumas, o autor foi além das opções oferecidas: volume mínimo definido pelo topo de cada tipo, e não pelos 1.000 sugeridos; passiva "única" com variação obrigatória em pelo menos um detalhe; revisão de todas as cartas SR+ com ajuste. Nos casos em que o autor aceitou a recomendação da IA (ex.: nomes dos seis elementos, regra de elemento por classe HTTP, cortes iniciais de raridade), isso ficou explícito na pergunta. Duas interpretações da IA sobre respostas do autor (D1 e D12) foram submetidas a ele e confirmadas.

**Revisão:** o autor respondeu a cada pergunta e validou as 12 premissas e as 2 interpretações por meio de perguntas estruturadas na sessão. A leitura integral do `spec.md` pelo autor não foi registrada nesta sessão e fica pendente antes da PR da feature.

---

## 2026-11-04 — Brainstorm e mudança de conceito

**O que foi feito:** Claude Code (modelo Opus 5.5, via extensão VSCode) participou do brainstorm, sugeriu a reformulação da tese, o fio condutor por camadas, o recorte e a estrutura de capítulos. Também arquivou o material do TCG e redigiu as atualizações de `PROMPT_TCC.md`, `CLAUDE.md`, `README.md`, `ATTRIBUTIONS.md` e da constituição.

**Decisões do autor (não da IA):** a mudança de conceito, a ideia do jogo e sua linha do transistor ao vídeo, o jogo como protagonista, o puzzle de cor, as portas lógicas como canos e a guarda do TCG. Referências citadas no brainstorm vieram de memória, não foram verificadas e não entraram no `.bib`.

**Revisão:** pendente; o autor revisará as alterações na PR.

---

## 2026-11-18 — Preenchimento do template DOCX da UVA

**O que foi feito:** Claude Code (modelo Opus 5.5, via extensão VSCode) preencheu o template da UVA em `docs/tcc/monografia/Reboot_Projeto_Tecnico.docx`:
- elementos pré-textuais com os dados fornecidos pelos autores;
- estrutura de capítulos, sumário e lista de siglas;
- rascunhos de resumo, abstract e dos capítulos 1 a 5, marcados como `[RASCUNHO-IA]`;
- capítulos 6 e 7 só com tópicos, marcados como `[A REDIGIR]`;
- referências levantadas e conferidas em busca na web, listadas em `referencias-verificadas.md`. Afirmações não confirmadas nos resumos das fontes estão marcadas com `[VERIFICAR]`.

**Decisões dos autores (não da IA):** título, coautoria, ordem alfabética dos autores, titulação do orientador, ano, formato Projeto Técnico, adoção do template DOCX e adiamento de dedicatória, agradecimentos e epígrafe.

**Revisão:** pendente. Todo texto marcado como rascunho será reescrito pelos autores.
