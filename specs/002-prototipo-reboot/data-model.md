# Data Model: Protótipo jogável do Reboot

**Feature**: `002-prototipo-reboot` | **Data**: 2026-11-19 | **Plano**: [plan.md](plan.md)

Todo o modelo vive no cliente. Há duas naturezas de dado:

- **Conteúdo**: fases, falas, dicas e fatos. É estático, versionado no repositório e só lido em tempo de
  execução. O formato está no [contrato de conteúdo](contracts/conteudo-fase.md).
- **Estado**: o que o jogador fez. Parte vive só na memória (a grade em montagem, os valores dos canais) e
  parte é salva no navegador (o progresso). O formato salvo está no
  [contrato do progresso](contracts/progresso-salvo.md).

## Tipos básicos

| Tipo | Definição |
|---|---|
| `Bit` | `0` ou `1`. O sinal de energia ligado ou desligado. |
| `Canal` | Inteiro de 0 a 255. |
| `Cor` | `{ r: Canal, g: Canal, b: Canal }` |
| `Direcao` | `'norte' \| 'leste' \| 'sul' \| 'oeste'` |
| `Posicao` | `{ linha: number, coluna: number }`, a partir de 0. |
| `TipoPorta` | `'AND' \| 'OR' \| 'NOT'` |

## Conteúdo

### Etapa

| Campo | Tipo | Regra |
|---|---|---|
| `id` | `'cor' \| 'canos'` | Único. |
| `titulo` | texto | |
| `fases` | lista de `id` de fase | Exatamente 2, na ordem de jogo (FR-003). |
| `explicacao` | lista de frases | Mostrada depois da prática (FR-012). |

A etapa `cor` vem antes da `canos` (FR-003).

### Fase (campos comuns)

| Campo | Tipo | Regra |
|---|---|---|
| `id` | texto | Único e estável entre versões, porque é a chave do progresso salvo. |
| `etapa` | `id` de etapa | |
| `limiteErros` | número ou `null` | Confirmações erradas permitidas. `null` na 1ª fase da etapa; `3` na 2ª, e a 4ª confirmação errada causa a derrota (FR-003). |
| `podePular` | booleano | `true` só quando `limiteErros` é `null` (FR-003). |
| `dicas` | 3 textos | Exatamente 3, do mais vago ao mais direto (FR-011). |
| `devolveAoMundo` | texto | O que reaparece no mundo ao concluir (FR-002). |
| `falaGuia` | `{ inicio, conclusao }` | Falas curtas e puláveis (FR-002). |

### Fase de cor

| Campo | Tipo | Regra |
|---|---|---|
| `alvo` | `Cor` | Valor exato a alcançar (FR-005). |
| `descricaoAlvo` | texto | Descrição da cor-alvo em palavras, sem números (FR-005, FR-018). |

A última fase de cor também traz `imagemPixels`: uma matriz pequena de `Cor` usada para relacionar pixels
aos três valores (FR-006, US1 cenário 4).

### Fase de canos

| Campo | Tipo | Regra |
|---|---|---|
| `tipo` | `'prever' \| 'escolher'` | Prever é a 1ª fase da etapa; escolher, a 2ª (clarificação de 2026-11-19). |
| `dimensoes` | `{ linhas, colunas }` | |
| `fixas` | lista de `PecaPosicionada` | Fontes e saída, que o jogador não move. |
| `bandeja` | lista de `Peca` | Peças disponíveis para arrastar. |
| `entradas` | lista de rótulos de fonte | Ex.: `['A', 'B']`. |
| `combinacoes` | lista de `{ entradas: Bit[], saidaEsperada: Bit }` | Todas as combinações testadas (FR-009). |

**Regras por tipo:**

- **Prever**: a bandeja traz exatamente uma junção, a definida pela fase.
- **Escolher**: a bandeja traz junções de mais de um tipo, e só a certa produz as saídas esperadas. Ligar a
  entrada à saída nunca basta (FR-010).

### Peça

| Variante | Campos | Portas |
|---|---|---|
| `fonte` | `rotulo`, `saida: Direcao` | Emite o bit da sua entrada na combinação atual. |
| `saida` | `entrada: Direcao` | Recebe o bit final. |
| `fio` | `portas: [Direcao, Direcao]` | Reta ou curva; liga as duas portas. |
| `juncao` | `porta: TipoPorta`, `entradas: Direcao[]`, `saida: Direcao` | AND e OR têm 2 entradas; NOT tem 1 (FR-008). |

A orientação de cada peça vem definida no conteúdo e o jogador não gira peças. `PecaPosicionada` é uma
`Peca` com uma `Posicao`.

### Fato histórico

| Campo | Tipo | Regra |
|---|---|---|
| `id` | texto | |
| `texto` | texto | |
| `fonte` | `{ titulo, autoria, url, acessadoEm }` | Obrigatória e verificada pelos autores (FR-014). |

## Estado em memória

### Tentativa de cor

`{ atual: Cor, ultimaConferencia: Conferencia | null }`. Toda fase de cor começa, e recomeça depois de uma
derrota, com `atual = { r: 0, g: 0, b: 0 }` e sem conferência (FR-004). A conferência de cada canal é
`'certo' | 'maior' | 'menor'`:

- `'maior'`: o valor certo está acima do inserido.
- `'menor'`: o valor certo está abaixo do inserido.

O valor certo nunca é exposto (FR-005).

### Montagem de canos

`{ grade: Map<Posicao, PecaPosicionada>, bandeja: Peca[], selecionada: Peca | null, previsoes: (Bit | null)[] | null }`

- Soltar uma peça fora da grade, ou numa célula ocupada por peça fixa, devolve a peça à bandeja sem alterar
  o restante da grade.
- Em fase do tipo `prever`, `previsoes` tem um bit ou "em branco" por combinação. Confirmar com alguma em
  branco é uma confirmação incompleta (ver abaixo).

### Resultado do teste de canos

Por combinação: `{ entradas, esperada, obtida: Bit | 'desconectado', passou }`. A fase passa quando todas
as combinações passam. No tipo `prever`, também exige que cada previsão seja igual à `obtida`.

A confirmação tem três desfechos:

- **incompleta**: alguma combinação dá `'desconectado'`, ou há previsão em branco. Não conta como erro
  (FR-009).
- **errada**: está completa, mas alguma combinação ou previsão falha. Conta como erro.
- **certa**: tudo passa.

### Contador da fase

`{ faseId, errosConfirmacao }`

## Estado salvo (progresso)

| Campo | Tipo |
|---|---|
| `versaoEsquema` | `1` |
| `fases` | mapa de `id` de fase para `'concluida' \| 'pulada'` |
| `emAndamento` | `{ faseId, errosConfirmacao } \| null` |
| `somMudo` | booleano |
| `semLimiteCor` | booleano: opção de acessibilidade que retira o limite das fases de cor (FR-003, FR-018) |

Nenhum campo identifica o jogador (FR-021). Formato e regras de leitura estão no
[contrato do progresso](contracts/progresso-salvo.md).

## Transições de estado

### Fase

```text
bloqueada ──(fase anterior concluída ou pulada)──▶ disponível ──(abrir)──▶ em andamento

em andamento ──(confirmação incompleta: caminho aberto ou previsão em branco)─▶ em andamento (sem mudança)
em andamento ──(confirmação certa)──────────────────────────────▶ concluída
em andamento ──(pular; só se podePular)─────────────────────────▶ pulada
em andamento ──(confirmação errada; sem limite, ou erros < limite)─▶ em andamento (erros + 1)
em andamento ──(confirmação errada com erros = limite)────────────▶ perdida
perdida ──(automático)──────────────────────────────────────────▶ em andamento (erros = 0, montagem zerada)
```

- Com limite 3, da 1ª à 3ª confirmação errada a fase continua, e na 4ª o jogador perde (FR-003).
- Com `semLimiteCor` ligado, o limite efetivo das fases de cor é `null`. O fato de a fase poder ser pulada
  continua a vir do conteúdo (`podePular`), então a 2ª fase de cor segue sem poder ser pulada.
- Perder não altera as fases já concluídas.
- As dicas liberadas são derivadas do contador: `dicasLiberadas = min(errosConfirmacao, 3)`. Nenhuma dica
  antes do 1º erro; a derrota zera o contador e, com ele, as dicas (FR-011). Como o contador fica salvo, as
  dicas liberadas continuam disponíveis ao recarregar a página no meio da fase.

### Jornada

```text
abertura (premissa pulável)
  → etapa cor: fase cor-1 → fase cor-2 → imagem de pixels → explicação
  → etapa canos: fase canos-1 (prever) → fase canos-2 (escolher) → revelação das portas → explicação
  → encerramento (resumo, aviso de protótipo, quiz não salvo)
```

Ao reabrir o jogo, ele retoma na primeira fase que não está concluída nem pulada.
