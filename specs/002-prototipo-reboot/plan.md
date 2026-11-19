# Implementation Plan: Protótipo jogável do Reboot

**Branch**: `feat/002-prototipo-reboot` | **Date**: 2026-11-19 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `specs/002-prototipo-reboot/spec.md`

## Summary

O protótipo é um jogo web estático, só no cliente, com 4 fases: 2 de cor RGB e 2 de canos que se comportam
como portas lógicas. Uma narrativa curta liga as fases, e um encerramento traz um quiz que não é salvo.

A abordagem técnica separa três camadas:

- **Domínio**: TypeScript puro, com conferência de cor, simulação do sinal, validação, jornada e progresso.
  Tem testes escritos antes da implementação.
- **Conteúdo**: arquivos tipados e versionados, com invariantes verificadas por teste.
- **Interface**: React no DOM, com arrastar por Pointer Events e alternativa por seleção acessível.

O progresso fica no `localStorage`. O build é publicado no GitHub Pages pelo GitHub Actions. As decisões
estão em [research.md](research.md) e no [ADR 0001](../../docs/adr/0001-stack-do-prototipo.md).

## Technical Context

**Language/Version**: TypeScript (modo estrito) sobre Node 26, fixado em `.nvmrc`.

**Primary Dependencies**: React e Vite. Versões estáveis no momento do setup, fixadas pelo `package-lock.json`.

**Storage**: `localStorage` do navegador, numa única chave versionada ([contrato](contracts/progresso-salvo.md)).

**Testing**: Vitest (domínio em Node; componentes com Testing Library em jsdom) e `axe-core` para
acessibilidade automática.

**Target Platform**: navegadores atuais de computador e celular. No celular, otimizado para retrato, sem
bloquear a paisagem. Site estático no GitHub Pages.

**Project Type**: aplicação web estática (single-page), sem backend.

**Performance Goals**: interação instantânea ao ajustar canais e mover peças. O jogador deve poder começar
em até 30 segundos depois de abrir o link (SC-001), o que pede um bundle pequeno e sem recursos externos.

**Constraints**:

- WCAG 2.2 nível AA, teclado, toque e leitor de tela, sem bloquear a orientação da tela (FR-018).
- Nenhuma requisição a outros domínios (FR-015, Princípio VI).
- Nenhum arquivo de arte ou áudio de terceiros no protótipo (FR-022).
- Determinismo (FR-017).

**Scale/Scope**: 4 fases, 2 etapas, cerca de 5 telas e 1 jogador por navegador. Sem contas nem
servidor.

## Constitution Check

*GATE: verificado antes da Fase 0 e de novo depois do design da Fase 1.*

| Princípio | Como o plano atende | Status |
|---|---|---|
| I. Código limpo e modular | Domínio, conteúdo e interface separados; funções puras pequenas; ESLint e Prettier no CI. | ✅ |
| II. Testes primeiro no domínio crítico | Cor, canos, validação, jornada e progresso têm testes antes do código; invariantes de conteúdo testadas; CI roda os testes em toda PR. A proteção da `main` exigindo o check é configurada pelo autor no GitHub. | ✅ |
| III. Plano antes de código | Este plano, e cada etapa do `/speckit-implement` começa com plano de 3 passos aprovado. | ✅ |
| IV. Sem suposições | Ambiguidades de prever/escolher e de autoria do conteúdo levadas ao autor e registradas na spec (clarificação de 2026-11-19). Conteúdo só entra com aprovação. | ✅ |
| V. Autoridade dos dados | Fonte da verdade: o navegador do jogador (`localStorage`). Nenhum dado de avaliação, publicação ou curadoria nesta feature, então não há servidor. | ✅ |
| VI. Conformidade por design | Sem compras, anúncios, cadastro nem coleta; quiz não salvo; nenhum recurso de terceiros carregado. | ✅ |
| VII. Fontes e licenças | Arte em CSS/SVG e áudio sintetizado, feitos para o projeto; fontes do sistema; fatos só com fonte verificada. | ✅ |
| VIII. Reprodutibilidade | Conteúdo versionado e determinístico; versão visível; Node fixado; `npm ci` com lockfile. | ✅ |
| IX. Rastreabilidade | ADR 0001 registra a stack; diário de bordo ao fim da feature. | ✅ |
| X. Git | Branch `feat/002-prototipo-reboot`, commits atômicos, PR com o template do projeto. | ✅ |

**Reavaliação pós-design**: o modelo de dados e os contratos não introduziram servidor, coleta de dados nem
asset de terceiros. Gate aprovado sem violações.

## Project Structure

### Documentation (this feature)

```text
specs/002-prototipo-reboot/
├── spec.md
├── plan.md              # Este arquivo
├── research.md          # Fase 0
├── data-model.md        # Fase 1
├── quickstart.md        # Fase 1
├── contracts/
│   ├── progresso-salvo.md
│   └── conteudo-fase.md
├── checklists/
│   └── requirements.md
└── tasks.md             # Gerado pelo /speckit-tasks
```

### Source Code (repository root)

```text
apps/reboot/
├── index.html
├── package.json
├── .nvmrc
├── vite.config.ts            # base /reboot-tcc/, injeção da versão
├── src/
│   ├── dominio/              # TypeScript puro, sem React nem APIs do navegador
│   │   ├── cor/              # conferência por canal, limites 0–255
│   │   ├── canos/            # portas lógicas, simulação do sinal, validação
│   │   ├── jornada/          # ordem das fases, limite de erros, pular
│   │   └── progresso/        # leitura, validação e escrita do progresso
│   ├── conteudo/             # etapas, fases, falas, dicas e fatos (rascunho da IA, aprovado pelos autores)
│   ├── interface/
│   │   ├── telas/            # abertura, fase de cor, fase de canos, explicação, encerramento
│   │   ├── componentes/      # seletor de canal, grade, bandeja, guia, dicas, versão
│   │   └── acessibilidade/   # anúncios aria-live e navegação por teclado na grade
│   ├── audio/                # efeitos e música sintetizados (Web Audio)
│   └── main.tsx
└── tests/
    ├── dominio/              # testes do domínio crítico, escritos antes do código
    ├── conteudo/             # invariantes e soluções de referência e incorretas (SC-006)
    └── interface/            # componentes, interação e axe

.github/workflows/
├── ci.yml                    # lint, tipos, testes e build em toda PR
└── pages.yml                 # build e publicação no GitHub Pages a cada merge na main

docs/adr/
└── 0001-stack-do-prototipo.md
```

**Structure Decision**: o app único fica em `apps/reboot/`, decisão do autor que deixa espaço para a
plataforma e o segundo jogo do backlog. Os testes ficam em `tests/`, espelhando `src/` e separados por
camada, para que o domínio crítico seja visível e auditável à parte.

## Complexity Tracking

Sem violações da constituição a justificar.
