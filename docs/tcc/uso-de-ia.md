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
