# ADR 0001: Stack do protótipo do Reboot

**Status**: Aceito

**Data**: 2026-11-19

**Feature**: [`002-prototipo-reboot`](../../specs/002-prototipo-reboot/plan.md)

## Contexto

O protótipo do Reboot é um jogo web com 4 fases de puzzle: duas de cor RGB e duas de canos que se
comportam como portas lógicas. A spec exige:

- abrir por um link, sem cadastro nem instalação;
- funcionar por toque, teclado e leitor de tela, no nível AA da WCAG;
- ter alternativa por seleção a toda ação de arrastar;
- ser determinístico e versionado;
- ter testes escritos antes da implementação no domínio crítico.

Os autores já conhecem TypeScript e React, e precisam explicar o código na defesa.

## Decisão

- **Linguagem**: TypeScript em modo estrito.
- **Interface**: React, renderizando elementos do DOM.
- **Build**: Vite, com saída estática.
- **Testes**: Vitest, com Testing Library e `axe-core` nos testes de componente.
- **Organização**: regras dos puzzles, jornada e progresso em TypeScript puro, sem dependência de React,
  em `apps/reboot/src/dominio/`.
- **Hospedagem**: GitHub Pages, publicado pelo GitHub Actions. Sem backend: o progresso fica no
  `localStorage` do navegador.

## Alternativas consideradas

- **Phaser (canvas)**: melhor para animação e física. Rejeitado porque o navegador não enxerga o
  conteúdo de um canvas, então teclado, foco e leitor de tela teriam de ser construídos à mão. Além
  disso, os puzzles do Reboot são de lógica e não precisam de física.
- **TypeScript sem framework**: menos dependências. Rejeitado por exigir mais código manual para
  atualizar a tela.

## Consequências

- **Positivas**:
  - A acessibilidade vem em grande parte do próprio navegador.
  - As regras ficam testáveis de forma isolada, como exige o Princípio II da constituição.
  - Não há custo de hospedagem.
- **Negativas**:
  - Animações elaboradas (para a arte final do FR-019) dependem de CSS e SVG, e podem exigir uma
    biblioteca de animação numa feature futura.
  - Sem backend, nenhum dado pode servir de base para a avaliação de aprendizado. A feature
    `avaliacao-aprendizado` terá de rever essa decisão (Princípio V).
- **Revisão**: se o GitHub Pages trouxer algum problema, os autores decidem a mudança de hospedagem
  num novo ADR.
