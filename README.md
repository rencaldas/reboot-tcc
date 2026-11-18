# Reboot

Jogo web de puzzles em que iniciantes aprendem como o computador funciona, do transistor ao vídeo. O mundo digital se apagou, e o jogador o religa camada por camada.

Projeto Técnico (TCC) de Ciência da Computação na Universidade Veiga de Almeida (UVA), com conclusão prevista para 2027.

- **Autores:** Luiz Fernando Magalhães da Silva e Renato de Azevedo Caldas
- **Orientador:** Prof. DSc. Erick da Silva Delvizio
- **Título do trabalho:** Jogos digitais no aprendizado de computação: desenvolvimento e avaliação do Reboot

## Pergunta de pesquisa

Em que condições um jogo digital cujo conteúdo é integrado à mecânica favorece a compreensão de fundamentos da computação por iniciantes? O trabalho não presume que jogos sejam sempre positivos.

## Status

| Frente | Situação |
|---|---|
| Conceito e constituição (v2.0.0) | Concluídos |
| Feature `002-prototipo-reboot` | Spec e clarify concluídos; próximo passo: `/speckit-plan` |
| Documento do TCC | Template da UVA preenchido com estrutura e rascunhos a reescrever |
| Código | Ainda não iniciado |

O protótipo terá 4 fases: 2 de cor RGB e 2 de portas lógicas representadas como canos de energia. Ver [`specs/002-prototipo-reboot/spec.md`](specs/002-prototipo-reboot/spec.md).

## Como começar

1. Peça acesso ao repositório, que é privado, e clone:

   ```bash
   git clone https://github.com/rencaldas/reboot-tcc.git
   ```

2. Abra a pasta no Claude Code. As skills do Spec Kit (`/speckit-*`) já estão versionadas em `.claude/skills`, e os scripts em `.specify/scripts`.
3. Leia, nesta ordem:
   - [`CLAUDE.md`](CLAUDE.md): regras de trabalho, obrigatórias para pessoas e para a IA;
   - [`PROMPT_TCC.md`](PROMPT_TCC.md): especificação completa e decisões;
   - [`.specify/memory/constitution.md`](.specify/memory/constitution.md): princípios do projeto.

No Windows, use o Git Bash para os scripts do Spec Kit.

## Processo

O projeto segue Spec-Driven Development com o [GitHub Spec Kit](https://github.com/github/spec-kit). Cada feature passa por:

`/speckit-specify` → `/speckit-clarify` → `/speckit-plan` → `/speckit-checklist` → `/speckit-tasks` → `/speckit-analyze` → `/speckit-implement` → `/speckit-converge`

Regras principais:

- Nada de commit direto na `main`. Use branches `feat/`, `fix/`, `chore/` ou `docs/` e abra PR com o template do `CLAUDE.md`.
- Commits em Conventional Commits.
- Plano de 3 passos aprovado antes de qualquer código.
- Todo uso de IA é registrado em [`docs/tcc/uso-de-ia.md`](docs/tcc/uso-de-ia.md), e cada feature entra no [diário de bordo](docs/tcc/diario-de-bordo.md).

## Documentos do TCC

| Documento | Conteúdo |
|---|---|
| [`docs/tcc/monografia/Reboot_Projeto_Tecnico.docx`](docs/tcc/monografia/Reboot_Projeto_Tecnico.docx) | Documento no template da UVA. Ao abrir no Word, aceite atualizar os campos |
| [`docs/tcc/referencias-verificadas.md`](docs/tcc/referencias-verificadas.md) | Referências com DOI, ISBN ou URL e pendências de verificação |
| [`docs/tcc/requisitos-academicos.md`](docs/tcc/requisitos-academicos.md) | Formato, orientação e pendências acadêmicas |
| [`docs/tcc/diario-de-bordo.md`](docs/tcc/diario-de-bordo.md) | Decisões e problemas por feature |
| [`docs/tcc/uso-de-ia.md`](docs/tcc/uso-de-ia.md) | O que a IA fez e o que os autores decidiram |

No DOCX, trechos marcados como `[RASCUNHO-IA]` devem ser reescritos pelos autores, `[A REDIGIR]` indica seções só com tópicos e `[VERIFICAR]` indica afirmações a conferir na fonte.

## Estrutura

```
reboot-tcc/
├── .claude/skills/        # skills do Spec Kit para o Claude Code
├── .specify/              # Spec Kit: constituição, templates, scripts, extensões
├── specs/                 # artefatos SDD por feature
├── apps/                  # aplicações (definido no /speckit-plan)
├── arquivo/               # conceitos fora do projeto ativo, como o TCG; não lido por código
├── reference/poc/         # provas de conceito (não é código de produção)
├── docs/
│   ├── adr/               # decisões de arquitetura
│   ├── arquitetura/       # diagramas
│   ├── api/
│   ├── manual-usuario/
│   └── tcc/               # documento do TCC, referências, diário e uso de IA
├── ATTRIBUTIONS.md
├── CLAUDE.md
├── PROMPT_TCC.md
└── README.md
```

## Conceito anterior

O projeto começou como um jogo de cartas colecionáveis (TCG) sobre programação. Ele será um projeto separado no futuro e não faz parte do TCC. A documentação está guardada em [`arquivo/tcg/`](arquivo/tcg/README.md) e na tag `arquivo/tcg-v0`.

## Licença

A definir, considerando as licenças registradas em [`ATTRIBUTIONS.md`](ATTRIBUTIONS.md).
