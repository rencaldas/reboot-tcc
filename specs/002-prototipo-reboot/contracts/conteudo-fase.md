# Contrato: conteúdo das fases

**Feature**: `002-prototipo-reboot` | **Requisitos**: FR-003, FR-005, FR-008 a FR-011, FR-014, FR-017, SC-006

O conteúdo fica em arquivos TypeScript tipados em `apps/reboot/src/conteudo/`, separados do código de regra
e de interface. Só é lido em tempo de execução, nunca gerado. Os tipos seguem o
[modelo de dados](../data-model.md).

## Fluxo de aprovação

1. A IA rascunha o conteúdo nesses arquivos, numa branch da feature.
2. Os autores revisam na PR. Nenhum conteúdo entra na `main` sem aprovação (clarificação de 2026-11-19).
3. Todo fato histórico tem fonte verificada pelos autores antes do merge (FR-014, Princípio VII).

## Invariantes verificadas por teste

Cada invariante tem um teste automatizado que roda no CI. Um conteúdo inválido quebra o build, não o jogo.

**Estrutura:**

- Existem 2 etapas, `cor` antes de `canos`, cada uma com exatamente 2 fases.
- Os `id` de fase são únicos.
- A 1ª fase de cada etapa tem `limiteErros: null` e `podePular: true`; a 2ª tem `limiteErros: 3` e
  `podePular: false`.
- Toda fase tem exatamente 3 dicas não vazias.

**Fases de cor:**

- Todo canal do `alvo` é inteiro entre 0 e 255.
- Toda fase de cor tem `descricaoAlvo` não vazia e sem algarismos, para não revelar os valores (FR-005).

**Fases de canos:**

- A 1ª fase de canos é `prever` e a 2ª é `escolher`.
- Toda peça fixa está dentro das dimensões e não há duas peças fixas na mesma célula.
- `combinacoes` cobre todas as combinações possíveis das entradas, sem repetição.
- Uma fase `prever` tem exatamente uma junção na bandeja.
- Uma fase `escolher` tem junções de mais de um tipo na bandeja.

**Soluções (SC-006):**

- Cada fase de canos tem ao menos uma solução de referência, mantida junto dos testes, e todas são
  aceitas pela validação.
- Cada fase de canos tem soluções incorretas de teste, e todas são rejeitadas. Entre elas, obrigatoriamente:
  - caminho ligado com a junção errada (FR-010);
  - caminho aberto;
  - no tipo `prever`, previsões erradas sobre um circuito correto.
- Uma solução alternativa correta, diferente da de referência, é aceita.

**Fatos históricos:**

- Todo fato tem `fonte` com título, autoria, URL e data de acesso preenchidos.
