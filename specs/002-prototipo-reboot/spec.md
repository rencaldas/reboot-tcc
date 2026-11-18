# Feature Specification: Protótipo jogável do Reboot

**Feature Branch**: `feat/002-prototipo-reboot`

**Created**: 2026-11-04

**Status**: Draft

**Input**: User description: "Protótipo jogável do Reboot, jogo web de puzzles em que iniciantes aprendem como o computador funciona, com um puzzle de cor RGB e um puzzle de canos em que junções se comportam como portas lógicas, jogável em cerca de 5 minutos por um link, sem cadastro."

## Clarifications

### Session 2026-11-17

- Q: Quantas fases o protótipo tem? → A: 4: 2 de cor e 2 de canos. Mais fases e conceitos entram depois, na linha do tempo que vai do binário à criação de vídeo.
- Q: Há prazo para mostrar ao orientador? → A: Não.
- Q: "Reboot" é o nome definitivo? → A: Sim.
- Q: As fases de canos alternam entre "prever o resultado" e "escolher a junção"? → A: Sim, alternam. Na de prever, o jogador marca a saída esperada de cada combinação antes de o sinal correr.
- Q: Quantas entradas cada junção tem? → A: NOT tem 1 entrada; AND e OR têm 2.
- Q: O que o sinal representa visualmente? → A: Energia.
- Q: Quantos níveis de dica cada fase tem? → A: 3 níveis.
- Q: O progresso some ao recarregar a página? → A: Não. Fica salvo no próprio navegador, sem dado pessoal.
- Q: Qual o estilo visual? → A: Desenhado à mão, bonito, com personalidade e satisfatório de ver, inspirado no estilo de Hollow Knight, sem copiar arte, personagens ou identidade (FR-016).
- Q: O jogo terá som? → A: Sim, com efeitos e música. Os arquivos de áudio ainda não existem.
- Q: Onde o jogo fica hospedado? → A: Site estático no GitHub Pages. Se surgir algum problema, o autor decide a mudança.
- Q: Qual a orientação no celular? → A: Retrato.
- Q: A acessibilidade inclui leitor de tela? → A: Sim, além de teclado, toque e contraste/foco no nível AA da WCAG.
- Q: Como medir se o testador aprendeu os conceitos? → A: Com um quiz no encerramento, dentro do jogo, sem salvar as respostas.
- Q: Quem são os 3 testadores? → A: 3 amigos, da área de computação ou de fora. O perfil ainda vai ser decidido.

### Session 2026-11-18

- Q: Como funciona a premissa narrativa do protótipo? → A: O mundo apagou e cada fase concluída devolve algo visível a ele, com um personagem guia que comenta o progresso do jogador.
- Q: Como o jogador interage com os canos? → A: Arrasta peças de uma bandeja para a grade, monta o caminho e, conforme o tipo da fase, escolhe a junção ou prevê as saídas.
- Q: De onde virão a arte e o áudio do protótipo? → A: No início, placeholders simples. A arte e o áudio finais virão depois, de assets livres de terceiros e de assets gerados por IA.
- Q: Quão perto da cor-alvo o jogador precisa chegar? → A: Valor exato nos três canais.
- Q: Como a cor-alvo é apresentada ao jogador? → A: Lado a lado com a cor do jogador. Ao apertar "Confirmar cor", cada canal errado recebe só "maior" ou "menor" em relação ao número inserido, sem revelar o valor.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Religar as cores (Priority: P1)

Um iniciante abre o jogo e descobre que o mundo digital apagou. Para devolver as cores, ajusta os canais vermelho, verde e azul até obter as cores pedidas. Ao final, entende que um pixel é descrito por três valores e sabe o que significa uma cor como (255, 0, 0).

**Why this priority**: É a experiência mais visual e rápida de entender, e sozinha já demonstra a proposta do jogo ao orientador.

**Independent Test**: Jogar só esta etapa, do início ao fim, e responder corretamente o que significa (255, 0, 0).

**Acceptance Scenarios**:

1. **Given** uma fase de cor, **When** o jogador ajusta os canais, **Then** a cor dele, exibida lado a lado com a cor-alvo, e os três valores são atualizados imediatamente.
2. **Given** uma cor com os três canais iguais aos da cor-alvo, **When** o jogador confirma, **Then** a fase é concluída e o conceito praticado é explicado em poucas frases.
3. **Given** uma cor com algum canal diferente da cor-alvo, **When** o jogador aperta "Confirmar cor", **Then** cada canal errado recebe "maior" ou "menor", indicando se o valor certo está acima ou abaixo do inserido, sem revelá-lo, e o jogador pode tentar de novo.
4. **Given** a última fase de cor, **When** é concluída, **Then** o jogo mostra uma imagem formada por pixels e relaciona cada pixel aos seus três valores.

---

### User Story 2 - Fazer o sinal passar pelos canos (Priority: P2)

O jogador organiza canos e junções para levar um sinal até a saída. Cada junção se comporta como uma porta lógica, e ele precisa prever o resultado ou escolher a lógica certa. Ao final, o jogo revela que as junções usadas eram portas AND, OR e NOT.

**Why this priority**: Mostra a mecânica como forma de aprender lógica, mas o protótipo ainda pode ser demonstrado sem ela.

**Independent Test**: Jogar só esta etapa, do início ao fim, e prever corretamente a saída de uma combinação de entradas que não apareceu nas fases.

**Acceptance Scenarios**:

1. **Given** uma fase de canos, **When** o jogador arrasta uma peça da bandeja para a grade, ou a move dentro dela, **Then** o caminho do sinal é atualizado de forma visível.
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

- Ao recarregar ou fechar a página, o progresso continua salvo no navegador. Se o dado salvo estiver ausente ou corrompido, o jogo recomeça do início sem erro, e o jogador pode apagar o progresso para recomeçar.
- Quem travar em uma fase recebe dicas graduais e pode pular a fase, para não abandonar a demonstração.
- Em tela de celular, o jogo é projetado para a orientação retrato, com controles utilizáveis por toque.
- Quem não distingue bem as cores consegue resolver as fases usando os valores numéricos.
- O estado dos canos nunca depende só da cor: o sinal também é indicado por forma, movimento ou texto.
- Uma peça solta fora da grade ou em posição inválida volta à bandeja, sem quebrar o caminho já montado.
- Uma solução correta diferente da prevista é aceita, porque a validação considera o comportamento, não um único arranjo.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O jogo MUST abrir por um link em navegadores atuais de computador e celular, sem cadastro, instalação ou pagamento, publicado como site estático no GitHub Pages.
- **FR-002**: O jogo MUST apresentar a premissa de que o mundo digital apagou e o jogador o religa, com opção de pular. Cada fase concluída MUST devolver algo visível ao mundo, e um personagem guia original MUST comentar o progresso em falas curtas, que podem ser puladas.
- **FR-003**: O jogo MUST oferecer uma etapa de cor antes da etapa de canos, e permitir pular fases sem bloquear o avanço.
- **FR-004**: A etapa de cor MUST permitir ajustar três canais, de 0 a 255, com resultado e valores visíveis em tempo real, e MUST oferecer ajuste de 1 em 1 e digitação do valor, para que o valor exato seja alcançável também no celular.
- **FR-005**: Cada fase de cor MUST ter uma cor-alvo definida exibida lado a lado com a cor atual do jogador, e só ser concluída quando os três canais forem exatamente iguais aos dela. A conferência MUST acontecer quando o jogador aperta "Confirmar cor", e cada canal errado MUST receber apenas "maior" ou "menor" em relação ao valor inserido.
- **FR-006**: A etapa de cor MUST relacionar os valores RGB a pixels de uma imagem.
- **FR-007**: A etapa de canos MUST permitir arrastar peças de uma bandeja para uma grade, movê-las e devolvê-las à bandeja, e mostrar o caminho do sinal, representado como energia. Toda ação de arrastar MUST ter alternativa por seleção, para teclado, toque simples e leitor de tela.
- **FR-008**: A etapa de canos MUST incluir junções com o comportamento das portas AND e OR, de 2 entradas, e NOT, de 1 entrada.
- **FR-009**: A validação de canos MUST testar todas as combinações de entrada definidas na fase e aceitar qualquer solução que produza as saídas corretas.
- **FR-010**: As fases de canos MUST alternar entre dois tipos: prever, em que o jogador marca a saída esperada de cada combinação antes de o sinal correr, e escolher, em que ele escolhe a junção certa. Ligar a entrada à saída não pode bastar.
- **FR-011**: O jogo MUST dar retorno específico em caso de erro, sem revelar a resposta na primeira tentativa, e oferecer 3 níveis de dica por fase.
- **FR-012**: Cada etapa MUST explicar seu conceito em poucas frases, depois de o jogador praticá-lo.
- **FR-013**: O encerramento MUST resumir os conceitos aprendidos, identificar a experiência como protótipo e apresentar um quiz curto: o significado de (255, 0, 0) e a saída de uma porta lógica numa combinação nova. As respostas do quiz MUST NOT ser salvas nem enviadas.
- **FR-014**: Todo fato histórico exibido MUST ter fonte registrada e consultável.
- **FR-015**: O jogo MUST NOT ter compras, loot boxes, publicidade, cadastro ou coleta de dados pessoais.
- **FR-016**: O jogo MUST NOT usar nome, fases, arte ou outra identidade de jogos existentes.
- **FR-017**: Fases, soluções e textos MUST ser determinísticos e identificados por uma versão visível do protótipo.
- **FR-018**: Nenhuma informação MUST depender apenas de cor, e o jogo MUST poder ser usado por toque, por teclado e por leitor de tela, com contraste e foco no nível AA da WCAG.
- **FR-019**: O visual final MUST ser desenhado à mão, com personalidade e animações satisfatórias, inspirado no estilo de Hollow Knight sem reproduzir arte, personagens ou identidade desse jogo (FR-016).
- **FR-020**: O jogo MUST ter efeitos sonoros e música, provisórios no protótipo, com controle para silenciar.
- **FR-021**: O progresso MUST ser salvo apenas no navegador do jogador, sem identificá-lo, e poder ser apagado pelo próprio jogador.
- **FR-022**: O protótipo MUST usar arte e áudio provisórios, feitos para o projeto. Quando entrarem assets finais, os de terceiros MUST ter licença verificada na fonte que permita uso e redistribuição, registrada em `ATTRIBUTIONS.md`, e os gerados por IA MUST ser declarados no registro de uso de IA.

### Key Entities

- **Etapa**: conjunto de fases sobre um conceito, com sua explicação e sua ordem na experiência.
- **Fase de cor**: cor-alvo, dicas e explicação.
- **Fase de canos**: grade, peças disponíveis na bandeja, entradas, saídas esperadas, tipo (prever ou escolher) e dicas.
- **Junção**: peça com um comportamento lógico (AND, OR ou NOT).
- **Personagem guia**: personagem original que apresenta a premissa e comenta o progresso, sem identidade de outros jogos.
- **Fato histórico**: texto exibido no jogo, associado à sua fonte.
- **Progresso salvo**: fases concluídas ou puladas, guardadas no navegador do jogador, sem identificá-lo.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Quem nunca viu o jogo consegue começar a jogar em até 30 segundos depois de abrir o link.
- **SC-002**: A experiência completa leva cerca de 5 minutos para a maioria dos testadores, e no máximo 10 minutos.
- **SC-003**: Em um teste informal com 3 amigos do autor (perfil a definir), ao menos 2 concluem as duas etapas sem ajuda.
- **SC-004**: No quiz do encerramento, ao menos 2 de 3 testadores acertam o que significa (255, 0, 0).
- **SC-005**: No quiz do encerramento, ao menos 2 de 3 testadores acertam a saída de uma porta lógica numa combinação nova.
- **SC-006**: Todas as soluções de referência são aceitas, e todas as soluções incorretas de teste são rejeitadas.
- **SC-007**: O jogo é concluído do início ao fim em um computador e em um celular na orientação retrato.

## Assumptions

- O protótipo serve para demonstração e teste informal. Ele não coleta dados para a pesquisa, e o teste com conhecidos não entra como resultado do TCC.
- O idioma é o português.
- Progresso sincronizado entre aparelhos, contas, placar, plataforma, publicação de outros jogos e os demais capítulos ficam fora de escopo.
- O protótipo tem 4 fases: 2 de cor e 2 de canos. As fases seguintes da linha do tempo (binário, hexadecimal, frames, áudio, vídeo etc.) ficam para features futuras.
- Não há prazo fixo para a demonstração ao orientador.
- "Reboot" é o nome definitivo do jogo.
- A cor vem antes dos canos, embora a lógica venha antes da imagem na ordem final dos capítulos, porque o protótipo prioriza a compreensão imediata.
- Fatos históricos podem ser poucos ou nenhum no protótipo, mas qualquer um exibido segue o FR-014.
- O protótipo usa arte e áudio provisórios. O estilo desenhado à mão do FR-019 vale para a arte final, que virá de assets livres de terceiros e de assets gerados por IA em uma feature futura.
