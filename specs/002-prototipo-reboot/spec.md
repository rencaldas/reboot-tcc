# Feature Specification: Protótipo jogável do Reboot

**Feature Branch**: `feat/002-prototipo-reboot`

**Created**: 2026-11-04

**Status**: Draft

**Input**: User description: "Protótipo jogável do Reboot, jogo web de puzzles em que iniciantes aprendem como o computador funciona, com um puzzle de cor RGB e um puzzle de canos em que junções se comportam como portas lógicas, jogável em cerca de 5 minutos por um link, sem cadastro."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Religar as cores (Priority: P1)

Um iniciante abre o jogo e descobre que o mundo digital apagou. Para devolver as cores, ajusta os canais vermelho, verde e azul até obter as cores pedidas. Ao final, entende que um pixel é descrito por três valores e sabe o que significa uma cor como (255, 0, 0).

**Why this priority**: É a experiência mais visual e rápida de entender, e sozinha já demonstra a proposta do jogo ao orientador.

**Independent Test**: Jogar só esta etapa, do início ao fim, e responder corretamente o que significa (255, 0, 0).

**Acceptance Scenarios**:

1. **Given** uma fase de cor com uma cor-alvo, **When** o jogador ajusta os canais, **Then** o resultado e os três valores são atualizados imediatamente.
2. **Given** uma cor dentro da tolerância da fase, **When** o jogador confirma, **Then** a fase é concluída e o conceito praticado é explicado em poucas frases.
3. **Given** uma cor fora da tolerância, **When** o jogador confirma, **Then** o jogo indica o que ajustar, sem revelar a resposta, e permite nova tentativa.
4. **Given** a última fase de cor, **When** é concluída, **Then** o jogo mostra uma imagem formada por pixels e relaciona cada pixel aos seus três valores.

---

### User Story 2 - Fazer o sinal passar pelos canos (Priority: P2)

O jogador organiza canos e junções para levar um sinal até a saída. Cada junção se comporta como uma porta lógica, e ele precisa prever o resultado ou escolher a lógica certa. Ao final, o jogo revela que as junções usadas eram portas AND, OR e NOT.

**Why this priority**: Mostra a mecânica como forma de aprender lógica, mas o protótipo ainda pode ser demonstrado sem ela.

**Independent Test**: Jogar só esta etapa, do início ao fim, e prever corretamente a saída de uma combinação de entradas que não apareceu nas fases.

**Acceptance Scenarios**:

1. **Given** uma fase de canos, **When** o jogador posiciona ou troca uma peça, **Then** o caminho do sinal é atualizado de forma visível.
2. **Given** uma fase com combinações de entrada definidas, **When** o jogador confirma a solução, **Then** o jogo a testa em todas essas combinações e mostra quais passaram e quais falharam.
3. **Given** um caminho ligado que produz a saída errada em alguma combinação, **When** é testado, **Then** a fase não é concluída, porque conectar as peças não basta.
4. **Given** uma etapa concluída, **When** o jogador avança, **Then** o jogo associa cada junção à porta lógica correspondente e à sua regra.

---

### User Story 3 - Abrir e testar em poucos minutos (Priority: P3)

O orientador recebe um link, abre o jogo sem cadastro nem instalação e percorre toda a experiência, com uma narrativa curta ligando as duas etapas e um encerramento que resume o que foi aprendido.

**Why this priority**: É o que transforma os puzzles em uma demonstração completa, mas depende de ao menos uma etapa pronta.

**Independent Test**: Enviar o link a alguém que nunca viu o jogo e observar se essa pessoa conclui a experiência sem ajuda.

**Acceptance Scenarios**:

1. **Given** o link do jogo, **When** é aberto em um navegador atual, **Then** a experiência começa sem cadastro, instalação ou pagamento.
2. **Given** a tela inicial, **When** o jogador começa, **Then** a premissa do mundo apagado é apresentada em poucos segundos e pode ser pulada.
3. **Given** as duas etapas concluídas, **When** o encerramento aparece, **Then** ele resume os conceitos aprendidos e informa que é um protótipo.
4. **Given** um fato histórico na tela, **When** o jogador o consulta, **Then** a fonte correspondente está disponível.

---

### Edge Cases

- Ao recarregar ou fechar a página, o jogador pode perder o progresso, mas consegue recomeçar sem erro.
- Quem travar em uma fase recebe dicas graduais e pode pular a fase, para não abandonar a demonstração.
- Em tela de celular, os controles continuam utilizáveis, com ou sem rotação da tela.
- Quem não distingue bem as cores consegue resolver as fases usando os valores numéricos.
- O estado dos canos nunca depende só da cor: o sinal também é indicado por forma, movimento ou texto.
- Uma solução correta diferente da prevista é aceita, porque a validação considera o comportamento, não um único arranjo.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O jogo MUST abrir por um link em navegadores atuais de computador e celular, sem cadastro, instalação ou pagamento.
- **FR-002**: O jogo MUST apresentar a premissa de que o mundo digital apagou e o jogador o religa, com opção de pular.
- **FR-003**: O jogo MUST oferecer uma etapa de cor antes da etapa de canos, e permitir pular fases sem bloquear o avanço.
- **FR-004**: A etapa de cor MUST permitir ajustar três canais, de 0 a 255, com resultado e valores visíveis em tempo real.
- **FR-005**: Cada fase de cor MUST ter uma cor-alvo e uma tolerância definidas, e ser validada por elas.
- **FR-006**: A etapa de cor MUST relacionar os valores RGB a pixels de uma imagem.
- **FR-007**: A etapa de canos MUST permitir posicionar ou trocar peças e mostrar o caminho do sinal.
- **FR-008**: A etapa de canos MUST incluir junções com o comportamento das portas AND, OR e NOT.
- **FR-009**: A validação de canos MUST testar todas as combinações de entrada definidas na fase e aceitar qualquer solução que produza as saídas corretas.
- **FR-010**: Cada fase de canos MUST exigir que o jogador preveja um resultado ou escolha uma lógica; ligar a entrada à saída não pode bastar.
- **FR-011**: O jogo MUST dar retorno específico em caso de erro, sem revelar a resposta na primeira tentativa, e oferecer dicas graduais.
- **FR-012**: Cada etapa MUST explicar seu conceito em poucas frases, depois de o jogador praticá-lo.
- **FR-013**: O encerramento MUST resumir os conceitos aprendidos e identificar a experiência como protótipo.
- **FR-014**: Todo fato histórico exibido MUST ter fonte registrada e consultável.
- **FR-015**: O jogo MUST NOT ter compras, loot boxes, publicidade, cadastro ou coleta de dados pessoais.
- **FR-016**: O jogo MUST NOT usar nome, fases, arte ou outra identidade de jogos existentes.
- **FR-017**: Fases, soluções e textos MUST ser determinísticos e identificados por uma versão visível do protótipo.
- **FR-018**: Nenhuma informação MUST depender apenas de cor, e o jogo MUST poder ser usado por toque e por teclado.

### Key Entities

- **Etapa**: conjunto de fases sobre um conceito, com sua explicação e sua ordem na experiência.
- **Fase de cor**: cor-alvo, tolerância, dicas e explicação.
- **Fase de canos**: peças disponíveis, entradas, saídas esperadas e dicas.
- **Junção**: peça com um comportamento lógico (AND, OR ou NOT).
- **Fato histórico**: texto exibido no jogo, associado à sua fonte.
- **Progresso da sessão**: fases concluídas ou puladas na sessão atual, sem identificar o jogador.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Quem nunca viu o jogo consegue começar a jogar em até 30 segundos depois de abrir o link.
- **SC-002**: A experiência completa leva cerca de 5 minutos para a maioria dos testadores, e no máximo 10 minutos.
- **SC-003**: Em um teste informal com 3 pessoas, ao menos 2 concluem as duas etapas sem ajuda.
- **SC-004**: Ao final, ao menos 2 de 3 testadores explicam corretamente o que significa (255, 0, 0).
- **SC-005**: Ao final, ao menos 2 de 3 testadores preveem corretamente a saída de uma porta lógica em uma combinação nova.
- **SC-006**: Todas as soluções de referência são aceitas, e todas as soluções incorretas de teste são rejeitadas.
- **SC-007**: O jogo é concluído do início ao fim em um computador e em um celular.

## Assumptions

- O protótipo serve para demonstração e teste informal. Ele não coleta dados para a pesquisa, e o teste com conhecidos não entra como resultado do TCC.
- O idioma é o português.
- Progresso entre sessões, contas, placar, plataforma, publicação de outros jogos e os demais capítulos ficam fora de escopo.
- A quantidade inicial é de 3 a 5 fases no total, com a divisão exata definida no `/speckit-clarify`.
- A cor vem antes dos canos, embora a lógica venha antes da imagem na ordem final dos capítulos, porque o protótipo prioriza a compreensão imediata.
- A narrativa é uma proposta a confirmar no `/speckit-clarify`.
- Fatos históricos podem ser poucos ou nenhum no protótipo, mas qualquer um exibido segue o FR-014.
