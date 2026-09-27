# tcg-dev

TCG (jogo de cartas colecionáveis) web + app com tema de programação, com catálogo de milhares de cartas gerado automaticamente a partir de dados públicos. Projeto Técnico (TCC) de Renato de Azevedo Caldas — Ciência da Computação, Universidade Veiga de Almeida (UVA).

## Status

Em fase de setup (Fase 0 do processo descrito em [`PROMPT_TCC.md`](PROMPT_TCC.md)). Sem código de produção ainda.

## Processo

O projeto segue Spec-Driven Development (SDD) com o [GitHub Spec Kit](https://github.com/github/spec-kit). Toda decisão de produto e arquitetura nasce como spec em `specs/`, plano em `.specify/`, ou ADR em `docs/adr/`, antes de virar código.

Ver [`PROMPT_TCC.md`](PROMPT_TCC.md) para o roteiro completo (fases, backlog de features, requisitos acadêmicos) e [`CLAUDE.md`](CLAUDE.md) para as regras de trabalho.

## Estrutura

```
tcg-dev/
├── .specify/              # Spec Kit (constituição, templates, extensões)
├── specs/                 # artefatos SDD por feature (gerados pelo Spec Kit)
├── pipeline/              # geração do catálogo de cartas
├── apps/                  # web, api, mobile (definido no /speckit-plan)
├── reference/poc/         # provas de conceito (não é código de produção)
├── docs/
│   ├── adr/               # decisões de arquitetura
│   ├── arquitetura/       # C4, ER, sequência (Mermaid)
│   ├── api/               # OpenAPI
│   ├── manual-usuario/
│   └── tcc/               # monografia, referências, diário de bordo
├── ATTRIBUTIONS.md
├── CLAUDE.md
└── README.md
```

## Licença

A definir (ver `LICENSE`, considerando as licenças das fontes de dados usadas — ver `ATTRIBUTIONS.md`).
