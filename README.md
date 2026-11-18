# tcg-dev

Jogo web de puzzles em que iniciantes aprendem como o computador funciona, do transistor ao vídeo, publicado em uma plataforma curada de jogos educativos. Projeto Técnico (TCC) de Luiz Fernando Magalhães da Silva e Renato de Azevedo Caldas — Ciência da Computação, Universidade Veiga de Almeida (UVA).

## Status

Conceito refeito e SDD do novo jogo em início. Sem código de produção ainda. O conceito anterior, um TCG, está em [`arquivo/tcg/`](arquivo/tcg/README.md).

## Processo

O projeto segue Spec-Driven Development (SDD) com o [GitHub Spec Kit](https://github.com/github/spec-kit). Toda decisão de produto e arquitetura nasce como spec em `specs/`, plano em `.specify/`, ou ADR em `docs/adr/`, antes de virar código.

Ver [`PROMPT_TCC.md`](PROMPT_TCC.md) para o roteiro completo (fases, backlog de features, requisitos acadêmicos) e [`CLAUDE.md`](CLAUDE.md) para as regras de trabalho.

## Estrutura

```
tcg-dev/
├── .specify/              # Spec Kit (constituição, templates, extensões)
├── specs/                 # artefatos SDD por feature (gerados pelo Spec Kit)
├── apps/                  # aplicações (definido no /speckit-plan)
├── arquivo/               # conceitos fora do projeto ativo; não lido por código
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
