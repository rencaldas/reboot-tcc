# Feature Specification: Catálogo de cartas gerado automaticamente

**Feature Branch**: `feat/001-catalogo-cartas`

**Created**: 2026-09-27

**Status**: Draft

**Input**: User description: "Pipeline de geração do catálogo de cartas de um TCG de programação. Como mantenedor do jogo, quero gerar automaticamente milhares de cartas a partir de fontes públicas, porque criar carta por carta é inviável. Cada carta representa um elemento do universo da programação: linguagens, status HTTP (ex.: 404, 403), métodos HTTP (ex.: GET, POST) e funções de linguagens (ex.: printf, scanf, print). A raridade reflete a importância do elemento, medida por um indicador de popularidade comparável entre tipos, e cada tipo tem uma faixa de raridades permitida: linguagens muito importantes (Python, C, Java) chegam a UR; funções ficam nas raridades baixas; LR é escolhida manualmente e é mais rara que UR. Cada carta tem atributos de jogo que dependem da raridade e não mudam entre execuções. Rodar o pipeline de novo com os mesmos dados produz exatamente o mesmo catálogo, e cartas já existentes nunca mudam de identidade nem são apagadas se já estiverem na coleção de algum jogador. O pipeline gera um relatório com a distribuição de cartas por tipo e raridade e lista as cartas que precisam de revisão manual (ex.: nomes genéricos com popularidade inflada). O catálogo não contém texto copiado das fontes nem logos de terceiros e registra a origem de cada carta. Uma fonte que falhar não derruba a execução inteira: o relatório indica o que ficou de fora." — Complemento do autor (2026-09-27): "Cartas terão ataque, defesa e HP. Futuramente podemos colocar dano crítico e chance de crítico etc. Elementos todas as cartas terão; a partir de SR, cada carta terá uma passiva única."

## Clarifications

### Session 2026-09-27

- Q: (specify) Como cada carta recebe o seu elemento? → A: Atribuição mista: a escolha manual prevalece; sem ela, vale a regra temática onde os dados permitem; no resto, distribuição determinística equilibrada.
- Q: (specify) Esta feature produz os textos das cartas (descrição e passiva de SR+)? → A: Sim. Rascunhos em lote por IA, com revisão do autor; carta sem texto aprovado não entra em versão publicada.
- Q: Qual o volume mínimo de cartas na primeira versão publicada? → A: Não há mínimo de 1.000; a versão precisa ter pelo menos as cartas mais populares de cada tipo.
- Q: Como os textos são revisados? → A: Passivas e descrições de cartas SR ou superior, uma a uma, ajustando quando necessário; descrições de cartas R por amostragem (10% de cada lote; qualquer problema devolve o lote inteiro).
- Q: O que "passiva única" significa? → A: O tipo de efeito pode se repetir entre cartas, mas nunca de forma idêntica: cada passiva difere das outras em pelo menos um detalhe (ex.: mais dano, um status levemente diferente).
- Q: O texto de uma carta publicada pode ser corrigido? → A: A descrição, sempre, com registro; a passiva, só até a feature `partida` convergir; depois disso ela congela.
- Q: Quais são os elementos? → A: Seis: Lógica, Dados, Rede, Sistema, Segurança e Interface. A tabela de efetividade entre eles fica para a feature `partida`.
- Q: Como cada tipo recebe o seu elemento? → A: Status HTTP pela classe (1xx Rede, 2xx Dados, 3xx Rede, 4xx Segurança, 5xx Sistema); Método HTTP, Linguagem e Função por distribuição equilibrada dentro do tipo; as cartas do topo podem ter o elemento escolhido à mão na revisão.
- Q: Qual fatia de cada tipo vai para cada raridade? → A: Como ponto de partida, ajustável até a primeira publicação: Linguagem UR 5%, SSR 20%, SR 75%; Status e Método HTTP SSR 10%, SR 30%, R 60%; Função SR 10%, R 90%.
- Q: O que acontece com itens sem tag no Stack Overflow? → A: Status e métodos HTTP entram todos; os sem tag recebem a menor raridade do tipo e vão para revisão. Linguagens e funções sem tag ficam fora, salvo alias ou inclusão manual.
- Q: Quando se define o mínimo de popularidade de cada tipo? → A: É calibrado com os dados reais antes da primeira publicação e registrado na versão do catálogo.
- Q: Quantas cartas do topo de cada tipo vão para a revisão manual? → A: As 20 mais populares de cada tipo (todas, se o tipo tiver menos de 20).
- Q: Em que idioma ficam as descrições e as passivas? → A: Português no MVP; inglês no futuro.
- Q: Como aparece o nome das cartas de função? → A: Sempre com a linguagem, ex.: `print (Python)`. Formato provisório, ajustável até a primeira publicação.
- Q: As premissas da seção Assumptions (escala e faixas de raridade, LR em qualquer tipo, só 4 tipos no MVP, palavras-chave como Função, funções homônimas distintas, congelamento só após a 1ª publicação, rebalanceamento fora do escopo, sem teto por tipo, viés só declarado, fora de circulação decidido em `boosters`) estão corretas? → A: Sim, todas confirmadas pelo autor, assim como as leituras de "os mais populares" (as 20 mais populares de cada tipo) e do nome de função provisório.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Gerar o catálogo a partir das fontes públicas (Priority: P1)

O mantenedor do jogo executa a geração do catálogo e obtém, sem criar nenhuma carta à mão, todas as cartas que passam nos filtros de qualidade (potencialmente milhares), de quatro tipos: linguagens de programação, status HTTP, métodos HTTP e funções de linguagens. Cada carta sai com nome, tipo, raridade, elemento, atributos de jogo (ataque, defesa e HP) e o registro de onde veio.

**Why this priority**: sem catálogo não existe jogo, e o catálogo gerado automaticamente é o núcleo da contribuição do projeto (Option B da avaliação da ideia, `.specify/assessments/tcg-programacao/concept.md`). Todas as outras features (coleção, boosters, arte, partida) consomem este resultado.

**Independent Test**: executar a geração sobre um conjunto fixo de dados de entrada e verificar que o catálogo resultante tem cartas dos quatro tipos, com todos os campos obrigatórios preenchidos e raridades dentro da faixa de cada tipo.

**Acceptance Scenarios**:

1. **Given** dados de entrada disponíveis para os quatro tipos, **When** o mantenedor executa a geração, **Then** o catálogo contém cartas dos quatro tipos, e cada carta tem identificador, nome, tipo, raridade, elemento, ataque, defesa, HP e origem.
2. **Given** uma linguagem entre as mais populares do seu tipo (ex.: Python), **When** a raridade é calculada, **Then** ela recebe UR, a raridade mais alta permitida a linguagens; **and given** uma função entre as menos populares do seu tipo, **Then** ela recebe R.
3. **Given** qualquer carta gerada sem override manual, **When** sua raridade é conferida, **Then** ela está dentro da faixa permitida ao seu tipo.
4. **Given** uma linguagem ou função cuja popularidade fica abaixo do mínimo do seu tipo, **When** a geração é executada, **Then** o item não vira carta e é contabilizado no relatório como filtrado.
5. **Given** qualquer carta gerada, **When** seus atributos são conferidos, **Then** ataque, defesa e HP estão dentro da faixa definida para a raridade da carta.
6. **Given** um status HTTP sem tag correspondente no Stack Overflow, **When** a geração é executada, **Then** ele vira carta com raridade R e entra na fila de revisão.
7. **Given** um status HTTP da classe 4xx sem escolha manual de elemento, **When** a geração é executada, **Then** a carta recebe o elemento Segurança.

---

### User Story 2 - Regenerar sem quebrar o que já existe (Priority: P1)

O mantenedor roda a geração de novo, seja com os mesmos dados (para reproduzir um resultado), seja com dados atualizados das fontes (para ampliar o catálogo). Com os mesmos dados, o catálogo sai idêntico. Com dados novos, entram cartas novas, mas nenhuma carta já publicada muda de identidade, raridade, elemento ou atributos, e nenhuma é apagada.

**Why this priority**: jogadores acumulam coleções ao longo do tempo; uma carta que muda ou some entre execuções quebra a coleção de quem já a tem (Princípio VIII da constituição). A reprodutibilidade também é exigência de rigor acadêmico para a avaliação quantitativa do catálogo.

**Independent Test**: gerar o catálogo duas vezes com as mesmas entradas e comparar carta a carta; depois gerar com entradas modificadas (um item novo, um item removido, uma popularidade alterada) e verificar que as cartas publicadas não mudaram.

**Acceptance Scenarios**:

1. **Given** as mesmas entradas, a mesma curadoria e os mesmos parâmetros, **When** a geração é executada duas vezes, **Then** os dois catálogos são idênticos em todas as cartas e todos os campos.
2. **Given** uma versão publicada que contém a carta X, **When** uma nova execução recebe uma popularidade diferente para o item de X, **Then** X mantém identificador, nome, tipo, raridade, elemento e atributos.
3. **Given** uma versão publicada que contém a carta X, **When** o item de X deixa de existir na sua fonte, **Then** X continua no catálogo, marcada como fora de circulação, e o relatório a lista.
4. **Given** um item novo na fonte, **When** uma nova execução é feita, **Then** ele vira uma carta com identificador novo, nunca reaproveitando o identificador de outra carta.

---

### User Story 3 - Relatório de geração e fila de revisão manual (Priority: P2)

A cada execução, o mantenedor recebe um relatório que mostra o que foi gerado (distribuição por tipo, raridade e elemento), quanto tempo levou, o que ficou de fora e por quê, o que mudou em relação à versão anterior, e quais cartas precisam de revisão manual, cada uma com o motivo.

**Why this priority**: sem o relatório o catálogo não é auditável, e as métricas previstas para a monografia (`PROMPT_TCC.md` §9.3, Avaliação e resultados) não existem. Mesmo assim, o catálogo da User Story 1 já tem valor antes do relatório.

**Independent Test**: executar a geração sobre entradas com casos conhecidos (um nome genérico, uma fonte indisponível simulada, um item sem correspondência de popularidade) e verificar que cada caso aparece no relatório com o motivo correto.

**Acceptance Scenarios**:

1. **Given** uma execução concluída, **When** o mantenedor abre o relatório, **Then** a contagem por tipo × raridade soma o total de cartas do catálogo.
2. **Given** uma carta cujo nome é genérico e cuja popularidade provavelmente mede o conceito e não o item (ex.: `map`, `print`), **When** a geração é executada, **Then** a carta aparece na fila de revisão com esse motivo.
3. **Given** uma das fontes indisponível, **When** a geração é executada, **Then** a execução termina, as demais fontes são processadas e o relatório indica a fonte que falhou e o que ficou de fora por causa dela.
4. **Given** um tipo com mais de 20 cartas, **When** a geração é executada, **Then** as 20 mais populares desse tipo aparecem na fila de revisão como topo do tipo; **and given** um tipo com menos de 20 cartas, **Then** todas aparecem.

---

### User Story 4 - Textos próprios das cartas, com revisão humana (Priority: P2)

O mantenedor gera em lote, com IA generativa e em português, rascunhos de descrição para todas as cartas e de uma passiva única para cada carta SR ou superior. Depois revisa: as passivas e as descrições das cartas SR ou superior, uma a uma, aprovando, editando ou rejeitando; as descrições das cartas R, por amostragem de cada lote. Só cartas com os textos aprovados entram numa versão publicada. Todo o processo fica registrado: o que foi gerado automaticamente, o que o autor editou, como e quando cada texto foi aprovado.

**Why this priority**: sem textos aprovados nenhuma carta pode ser publicada (FR-037), e a descrição é o que dá identidade à carta perante o jogador. Mesmo assim, a geração e a estabilidade do catálogo (P1) já podem ser testadas em versões de rascunho, sem textos. O registro do processo também alimenta a declaração de uso de IA do TCC (`docs/tcc/uso-de-ia.md`).

**Independent Test**: gerar rascunhos para um conjunto pequeno de cartas (com cartas R e SR+); aprovar, editar e rejeitar rascunhos de cartas SR+ individualmente; revisar por amostragem um lote de descrições de cartas R; publicar uma versão e verificar que só as cartas com textos aprovados entram nela e que cada texto registra seu histórico.

**Acceptance Scenarios**:

1. **Given** cartas sem texto, **When** o mantenedor pede a geração de rascunhos, **Then** cada carta recebe um rascunho de descrição em português, cada carta SR ou superior recebe também um rascunho de passiva, e cartas R não recebem passiva.
2. **Given** um rascunho de uma carta SR ou superior, **When** o mantenedor o aprova, edita ou rejeita, **Then** a decisão fica registrada com data, e um texto editado fica marcado como editado pelo autor.
3. **Given** um lote de descrições de cartas R, **When** o mantenedor revisa uma amostra de 10% do lote sem encontrar problema, **Then** todas as descrições do lote são aprovadas e registradas como aprovadas por amostragem; **and when** encontra qualquer problema na amostra, **Then** o lote inteiro volta para rascunho.
4. **Given** uma carta com algum texto obrigatório ainda não aprovado, **When** uma versão é publicada, **Then** essa carta não entra na versão publicada e o relatório a lista como pendente de texto.
5. **Given** textos já gerados, **When** a geração do catálogo é executada de novo, **Then** nenhum texto é gerado de novo e o catálogo sai idêntico.
6. **Given** uma passiva idêntica a outra já aprovada, **When** o mantenedor tenta aprová-la, **Then** a duplicata é sinalizada e a aprovação só é aceita depois que a passiva for diferenciada em pelo menos um detalhe.
7. **Given** uma carta publicada, **When** o mantenedor corrige a sua descrição, **Then** a correção gera uma nova versão do catálogo e fica registrada no histórico do texto; **and given** que a feature `partida` já convergiu, **When** o mantenedor tenta corrigir uma passiva publicada, **Then** a correção é recusada.

---

### User Story 5 - Curadoria manual aplicada de forma reprodutível (Priority: P3)

O mantenedor registra decisões manuais (promover uma carta a LR, escolher o elemento de uma carta, apontar a tag certa para um nome que não bate com a do Stack Overflow, incluir ou excluir um item, resolver um item da fila de revisão), sempre com o motivo. Toda execução seguinte aplica essas decisões automaticamente e sempre do mesmo jeito.

**Why this priority**: o catálogo automático tem valor antes de qualquer curadoria, mas LR só existe por curadoria, e a fila de revisão precisa de um jeito de ser resolvida sem retrabalho a cada execução.

**Independent Test**: registrar um override para LR, um alias de tag e uma exclusão; regenerar e verificar os três efeitos; regenerar de novo e verificar que o resultado é idêntico.

**Acceptance Scenarios**:

1. **Given** um override para LR registrado para a carta X ainda não publicada, **When** a geração é executada, **Then** X sai com raridade LR, e nenhuma carta sem override sai LR.
2. **Given** um alias que liga o nome de um item a uma tag de popularidade, **When** a geração é executada, **Then** a popularidade do item vem dessa tag e ele deixa de contar como sem correspondência.
3. **Given** uma exclusão manual de um item ainda não publicado, **When** a geração é executada, **Then** o item não vira carta.
4. **Given** um item da fila de revisão marcado como resolvido, **When** uma nova execução é feita, **Then** ele não reaparece na fila.
5. **Given** uma escolha manual de elemento para a carta X ainda não publicada, **When** a geração é executada, **Then** X recebe esse elemento, mesmo que a regra temática ou a distribuição automática indicassem outro.

---

### Edge Cases

- **Funções com o mesmo nome em linguagens diferentes** (ex.: `print`): viram cartas distintas, uma por linguagem, e o nome de cada uma inclui a linguagem (`print (Python)`); se a tag de popularidade for genérica, todas entram na fila de revisão.
- **Empate de popularidade no limite entre duas raridades**: resolvido por uma regra de desempate determinística, que não depende da ordem em que os dados foram lidos.
- **Tipo com poucos itens** (ex.: métodos HTTP): a distribuição por percentil continua produzindo só raridades permitidas ao tipo, o relatório mostra a distribuição real, e todas as cartas do tipo entram na revisão de topo se houver menos de 20.
- **Fonte com falha vs. item que sumiu**: uma falha de fonte nunca é interpretada como desaparecimento de itens; nenhuma carta é marcada como fora de circulação por causa de uma falha.
- **Linguagem ou função sem tag correspondente** (popularidade indeterminável): não vira carta automaticamente, é contada como "sem correspondência" no relatório e pode entrar por alias ou inclusão manual.
- **Status ou método HTTP sem tag correspondente**: vira carta com a menor raridade do tipo (R), fora do cálculo de percentil, e entra na fila de revisão.
- **Identificadores compostos no tipo Função**: só identificadores simples entram; os demais são contabilizados como filtrados.
- **Limite de uso de uma fonte atingido no meio da execução**: a execução para de consultar essa fonte, trata os itens não obtidos como falha de fonte (não como desaparecimento) e os lista no relatório.
- **Curadoria que alteraria uma carta já publicada**: uma exclusão marca a carta como fora de circulação (nunca a remove); uma promoção a LR ou uma troca de elemento não é aplicada e aparece no relatório como curadoria não aplicada.
- **Elemento sem nenhuma carta num tipo com regra temática**: aceito (ex.: nenhum status HTTP recebe Lógica ou Interface pela regra de classe); o relatório mostra a distribuição por elemento de cada tipo.
- **Rascunho de texto que reproduz trecho de documentação conhecido**: a revisão humana rejeita, e a carta recebe um novo rascunho.
- **Carta rejeitada várias vezes**: continua em rascunho, fora das versões publicadas, até que o autor aprove um texto (gerado ou escrito por ele).
- **Lote de descrições R com problema na amostra**: o lote inteiro volta para rascunho, inclusive as descrições fora da amostra.
- **Passiva a corrigir depois que a feature `partida` convergiu**: a correção é recusada; mudar uma passiva congelada é rebalanceamento, fora do escopo desta feature.
- **Dados novos que dariam outra raridade a uma carta publicada**: a carta mantém a raridade publicada e o relatório registra a divergência (insumo para analisar o viés da métrica de popularidade).

## Requirements *(mandatory)*

### Functional Requirements

**Geração e tipos de carta**

- **FR-001**: O sistema DEVE gerar cartas automaticamente, sem criação manual carta a carta, para quatro tipos: Linguagem de programação, Status HTTP, Método HTTP e Função (funções e palavras-chave de linguagens).
- **FR-002**: Cada tipo DEVE ter uma fonte primária que define quais itens existem; as demais fontes apenas complementam dados (ex.: popularidade).
- **FR-003**: A popularidade de cada item DEVE ser medida pela contagem de perguntas da tag correspondente no Stack Overflow, única régua usada para comparar itens de tipos diferentes.
- **FR-004**: Nos tipos Linguagem e Função, itens com popularidade abaixo do mínimo do tipo NÃO DEVEM virar carta. O mínimo de cada tipo DEVE ser calibrado com os dados reais antes da primeira publicação e registrado na versão do catálogo.
- **FR-005**: Nos tipos Linguagem e Função, itens sem tag correspondente NÃO DEVEM virar carta automaticamente e DEVEM ser contabilizados como "sem correspondência". Nos tipos Status HTTP e Método HTTP, todos os itens da lista padronizada DEVEM virar carta; os que não têm tag recebem a menor raridade do tipo e entram na fila de revisão.
- **FR-006**: No tipo Função, só identificadores simples DEVEM virar carta.
- **FR-006a**: O nome de uma carta do tipo Função DEVE incluir a linguagem de origem (ex.: `print (Python)`).

**Raridade**

- **FR-007**: A escala de raridade DEVE ser, em ordem crescente: R, SR, SSR, UR, LR.
- **FR-008**: A raridade de cada carta DEVE ser a posição (percentil) da sua popularidade dentro do seu tipo, calculada só entre os itens com popularidade conhecida e limitada à faixa do tipo: Linguagem de SR a UR; Status HTTP de R a SSR; Método HTTP de R a SSR; Função de R a SR. Os cortes de percentil DEVEM ser parâmetros registrados em cada versão do catálogo, com estes valores iniciais, ajustáveis até a primeira publicação: Linguagem UR 5%, SSR 20%, SR 75%; Status e Método HTTP SSR 10%, SR 30%, R 60%; Função SR 10%, R 90% (a fatia mais popular recebe a raridade mais alta).
- **FR-009**: A raridade LR DEVE existir apenas por curadoria manual e nunca DEVE resultar do cálculo estatístico.
- **FR-010**: Empates de popularidade DEVEM ser resolvidos por uma regra determinística, independente da ordem de leitura dos dados.

**Atributos, elemento e passiva**

- **FR-011**: Toda carta DEVE ter ataque, defesa e HP, com valores dentro da faixa definida para a sua raridade e derivados de forma determinística da identidade da carta (a mesma carta sempre recebe os mesmos valores).
- **FR-012**: As faixas de atributos por raridade DEVEM ser parâmetros registrados em cada versão do catálogo.
- **FR-013**: O modelo de carta DEVE permitir acrescentar atributos no futuro (ex.: dano crítico, chance de crítico) sem alterar os valores dos atributos já existentes.
- **FR-014**: Toda carta DEVE ter exatamente um elemento, entre seis: Lógica, Dados, Rede, Sistema, Segurança e Interface. O elemento é atribuído em três camadas, da de maior precedência para a de menor: (a) a escolha manual (curadoria) para uma carta ainda não publicada; (b) sem escolha manual, no tipo Status HTTP, a regra temática pela classe do código: 1xx Rede, 2xx Dados, 3xx Rede, 4xx Segurança, 5xx Sistema; (c) nos tipos Método HTTP, Linguagem e Função, distribuição determinística e equilibrada entre os seis elementos dentro de cada tipo, a partir da identidade da carta.
- **FR-015**: Toda carta de raridade SR ou superior DEVE ter uma passiva única e temática em relação ao que a carta representa; cartas R NÃO DEVEM ter passiva. "Única" significa que duas passivas podem usar o mesmo tipo de efeito, mas nunca de forma idêntica: cada uma difere das demais em pelo menos um detalhe (ex.: valor de dano, duração, status aplicado). Esta feature produz as passivas e as descrições das cartas (ver "Textos da carta").
- **FR-015a**: A lista de elementos e as regras de atribuição por tipo DEVEM ser parâmetros registrados em cada versão do catálogo.

**Identidade, reprodutibilidade e versões**

- **FR-016**: Cada carta DEVE ter um identificador estável, derivado do seu tipo e da chave do item na fonte primária. Um identificador NUNCA DEVE ser reatribuído a outra carta.
- **FR-017**: Execuções com as mesmas entradas, a mesma curadoria e os mesmos parâmetros DEVEM produzir catálogos idênticos.
- **FR-018**: Os dados obtidos das fontes em cada execução DEVEM ser armazenados e identificados, para que qualquer versão do catálogo possa ser reproduzida sem consultar as fontes de novo.
- **FR-019**: Cada execução que altera o conteúdo do catálogo DEVE gerar uma nova versão numerada; versões anteriores DEVEM ser preservadas sem alteração.
- **FR-020**: Depois que uma versão é marcada como publicada, nenhuma versão posterior PODE alterar identificador, nome, tipo, raridade, elemento ou atributos de uma carta publicada, nem removê-la.
- **FR-020a**: Nos textos de uma carta publicada, a descrição PODE ser corrigida a qualquer momento; a passiva PODE ser corrigida só até a feature `partida` convergir e, depois disso, NÃO PODE mais mudar. Toda correção DEVE gerar uma nova versão do catálogo e ficar registrada no histórico do texto, com o motivo.
- **FR-021**: Quando o item de uma carta publicada deixa de existir na fonte primária, a carta DEVE permanecer no catálogo, marcada como fora de circulação.
- **FR-022**: Curadoria que alteraria uma carta publicada NÃO DEVE ser aplicada silenciosamente: uma exclusão marca a carta como fora de circulação; uma promoção a LR ou uma troca de elemento não é aplicada e é sinalizada no relatório.

**Fontes, conteúdo e licenças**

- **FR-023**: A falha de uma fonte NÃO DEVE interromper a execução: as demais fontes são processadas e o relatório indica o que ficou de fora.
- **FR-024**: A coleta DEVE respeitar os limites de uso publicados de cada fonte e reaproveitar dados já obtidos, em vez de pedi-los de novo.
- **FR-025**: Os termos de uso e a licença de cada fonte DEVEM estar registrados em `ATTRIBUTIONS.md`, verificados na própria fonte, antes de a fonte ser usada no catálogo.
- **FR-026**: Nenhum campo de texto de carta PODE reproduzir texto das fontes, e o catálogo NÃO PODE conter logos ou marcas gráficas de terceiros.
- **FR-027**: Cada carta DEVE registrar sua origem: fonte(s), chave do item na fonte e data da coleta.
- **FR-028**: Cartas do tipo Linguagem DEVEM registrar a cor associada à linguagem na fonte primária, quando existir, para uso posterior na arte das cartas.

**Relatório e revisão**

- **FR-029**: Cada execução DEVE produzir um relatório com: total de cartas; distribuição por tipo × raridade; distribuição por elemento, geral e por tipo; tempo de geração; taxa de itens sem correspondência, por tipo; itens filtrados, por motivo; fontes que falharam e o impacto de cada falha; diferenças em relação à versão anterior (cartas novas e cartas que saíram de circulação); situação dos textos (quantas descrições e passivas estão pendentes, em rascunho, aprovadas ou rejeitadas); e a fila de revisão.
- **FR-030**: A fila de revisão DEVE sinalizar, com motivo: (a) as 20 cartas mais populares de cada tipo (todas, se o tipo tiver menos de 20); (b) cartas de nome genérico cuja popularidade provavelmente mede o conceito e não o item; (c) cartas publicadas cuja raridade recalculada seria diferente da publicada; (d) curadoria não aplicada (FR-022); (e) status e métodos HTTP sem tag correspondente (FR-005).

**Curadoria**

- **FR-031**: O mantenedor DEVE poder registrar, sempre com motivo: overrides para LR, escolha manual de elemento, aliases de nome para tag, inclusões e exclusões manuais, e decisões sobre itens da fila de revisão. Toda execução DEVE aplicar essas decisões de forma determinística.
- **FR-032**: Um item da fila de revisão marcado como resolvido NÃO DEVE reaparecer na fila em execuções seguintes.

**Textos da carta**

- **FR-033**: O sistema DEVE gerar em lote, com IA generativa, rascunhos próprios de descrição para toda carta e de passiva para toda carta SR ou superior, sem reproduzir texto das fontes (FR-026).
- **FR-034**: O mantenedor DEVE poder aprovar, editar ou rejeitar cada rascunho. Uma carta com rascunho rejeitado pode receber um novo rascunho, gerado só para ela.
- **FR-035**: Cada texto DEVE registrar sua situação (pendente, rascunho, aprovado ou rejeitado) e seu histórico: se foi gerado automaticamente ou escrito/editado pelo autor, qual processo de geração foi usado, a data e a decisão da revisão, se a aprovação foi individual ou por amostragem (com o lote correspondente), e as correções posteriores (FR-020a).
- **FR-036**: Um texto gerado DEVE ser armazenado e reaproveitado nas execuções seguintes, e nunca gerado de novo automaticamente. Uma nova geração só ocorre para cartas sem texto ou por pedido explícito do mantenedor para uma carta específica.
- **FR-037**: Só cartas com todos os textos obrigatórios aprovados (descrição; e passiva, se SR ou superior) PODEM entrar numa versão publicada. As demais ficam em rascunho, e o relatório as lista como pendentes de texto.
- **FR-038**: Nenhuma passiva aprovada PODE ser idêntica a outra. Passivas com texto igual DEVEM ser bloqueadas automaticamente; a revisão confirma que passivas com o mesmo tipo de efeito diferem em pelo menos um detalhe (FR-015).
- **FR-039**: A revisão DEVE seguir esta política: passivas e descrições de cartas SR ou superior são revisadas uma a uma; descrições de cartas R são revisadas por lote, com amostra de 10% do lote. Se a amostra não tiver problema, o lote inteiro é aprovado; se tiver qualquer problema, o lote inteiro volta para rascunho.
- **FR-040**: Os textos DEVEM ser escritos em português no MVP. O modelo de carta DEVE permitir acrescentar outros idiomas no futuro (ex.: inglês) sem alterar os textos existentes.

### Key Entities *(include if feature involves data)*

- **Carta**: unidade colecionável do jogo. Tem identificador estável, nome (nas funções, acompanhado da linguagem), tipo, raridade, elemento, atributos (ataque, defesa, HP, extensível), passiva (apenas SR ou superior), origem, situação (em circulação ou fora de circulação) e a versão do catálogo em que foi publicada pela primeira vez.
- **Tipo de carta**: categoria de origem do item (Linguagem, Status HTTP, Método HTTP, Função). Define a fonte primária, a faixa de raridades permitida, os cortes de percentil, o mínimo de popularidade e a regra de elemento. Não se confunde com Elemento.
- **Elemento**: afinidade de jogo; um de seis (Lógica, Dados, Rede, Sistema, Segurança, Interface). É usado pela mecânica de efetividade entre elementos, cuja tabela e regras pertencem à feature `partida`. Toda carta tem exatamente um.
- **Raridade**: R, SR, SSR, UR e LR, em ordem crescente, cada uma com sua faixa de atributos.
- **Passiva**: efeito especial de uma carta SR ou superior, temático em relação ao que a carta representa (ex.: a carta do status 401 Unauthorized impede o oponente de jogar por 1 turno). Pode repetir o tipo de efeito de outra passiva, mas não de forma idêntica. Nesta feature, a passiva é um texto aprovado; a regra executável correspondente pertence à feature `partida`.
- **Texto da carta**: descrição (toda carta) ou passiva (SR ou superior), em português no MVP. Tem situação (pendente, rascunho, aprovado ou rejeitado) e histórico (origem automática ou autoral, processo de geração usado, data e decisão da revisão, aprovação individual ou por amostragem, correções posteriores).
- **Lote de revisão**: conjunto de descrições de cartas R revisado por amostragem. Registra quais descrições contém, qual amostra foi lida, o resultado e a data.
- **Fonte de dados**: provedor externo de itens ou de popularidade, com termos de uso, licença e limites de uso.
- **Coleta**: cópia dos dados obtidos das fontes numa execução, identificada, que permite reproduzir a versão gerada a partir dela.
- **Curadoria**: conjunto de decisões manuais (overrides LR, escolhas de elemento, aliases, inclusões, exclusões e resoluções de revisão), cada uma com motivo.
- **Versão do catálogo**: conjunto imutável de cartas, junto com os parâmetros (elementos e regras de atribuição, cortes de percentil, faixas de atributos, mínimos por tipo), a curadoria e a coleta que a produziram. Pode ser rascunho ou publicada.
- **Relatório de geração**: resumo de uma execução (FR-029).
- **Item de revisão**: carta sinalizada, com motivo, situação (pendente ou resolvido) e a decisão tomada.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A primeira versão publicada contém, no mínimo, as 20 cartas mais populares de cada tipo (todas, se o tipo tiver menos de 20), com textos aprovados. Não há volume mínimo total; o volume gerado (todas as cartas que passam nos filtros) é medido no relatório.
- **SC-002**: Duas execuções com as mesmas entradas, curadoria e parâmetros produzem catálogos idênticos em 100% das cartas e dos campos.
- **SC-003**: 100% das cartas têm raridade dentro da faixa do seu tipo, ou LR com override registrado; 0 cartas LR sem override.
- **SC-004**: 100% das cartas de qualquer versão publicada existem em todas as versões posteriores, com identificador, nome, tipo, raridade, elemento e atributos inalterados.
- **SC-005**: Com qualquer uma das fontes indisponível, a execução termina, o relatório identifica a fonte e o que ficou de fora, e nenhuma carta publicada é perdida ou tirada de circulação por causa da falha.
- **SC-006**: 100% das cartas têm origem registrada, e 0 campos de texto reproduzem trechos das fontes.
- **SC-007**: Nenhuma execução ultrapassa os limites de uso publicados de qualquer fonte.
- **SC-008**: Uma versão do catálogo pode ser regenerada a partir da coleta armazenada, sem consultar nenhuma fonte externa, com resultado idêntico ao original.
- **SC-009**: Num conjunto de teste com casos conhecidos de cada categoria de revisão (FR-030), 100% dos casos aparecem na fila de revisão com o motivo correto.
- **SC-010**: O relatório de cada execução traz, sem cálculo manual adicional, as métricas previstas para a monografia: distribuição por tipo e raridade, tempo de geração e taxa de itens sem correspondência.
- **SC-011**: Em qualquer versão publicada, 100% das cartas têm descrição aprovada, 100% das cartas SR ou superior têm passiva aprovada, e 0 passivas são idênticas entre si.
- **SC-012**: 100% dos textos de uma versão publicada têm histórico completo, o que permite contar quantos foram aprovados sem edição, editados, escritos pelo autor, aprovados individualmente ou aprovados por amostragem.
- **SC-013**: 100% das cartas têm exatamente um dos seis elementos; 100% das cartas com escolha manual saem com o elemento escolhido; e 100% dos status HTTP sem escolha manual têm o elemento da sua classe.
- **SC-014**: 0 passivas publicadas mudam depois que a feature `partida` converge, e 100% das correções de texto em cartas publicadas estão registradas com versão e motivo.

## Assumptions

- **Escala de raridade**: R < SR < SSR < UR < LR, conforme a ideia original do autor (`PROMPT_TCC.md` §3), confirmada na clarificação de 2026-09-27 (o §4.4 a listava como premissa não confirmada).
- **Volume**: não há mínimo total de cartas (decisão do autor na clarificação). O pipeline gera tudo o que passa nos filtros; a publicação exige pelo menos o topo de cada tipo (SC-001). O volume real só se conhece implementando o pipeline (`decision.md`).
- **Quando a estabilidade passa a valer**: a garantia de que cartas não mudam (FR-020) vale a partir da primeira versão marcada como publicada. Antes disso, durante o desenvolvimento, as versões são rascunho, e cortes, faixas, mínimos e o formato de nome das funções podem ser ajustados livremente.
- **Rebalanceamento**: alterar deliberadamente atributos de cartas já publicadas, ou uma passiva depois que a `partida` convergir, está fora do escopo desta feature. Se for necessário no futuro, será uma decisão explícita registrada em ADR.
- **Tipos no MVP**: só os quatro do FR-001. As expansões candidatas (Wikidata, MDN, exceções e erros de runtime, estruturas de dados, padrões de projeto, ferramentas como Git e Docker) ficam fora; o modelo permite acrescentar tipos sem alterar cartas existentes.
- **Palavras-chave** das linguagens entram no tipo Função.
- **LR** pode ser atribuída a uma carta de qualquer tipo; o §4.1 não restringe o tipo.
- **Limite por tipo**: não há teto de cartas por tipo no MVP; o volume é controlado pelo mínimo de popularidade de cada tipo.
- **Viés temporal da métrica**: a contagem de perguntas do Stack Overflow favorece tecnologias antigas (`research.md`). O MVP não aplica correção; o viés é registrado como limitação, e o relatório mede as divergências (FR-030 c). A mitigação, se houver, é questão aberta herdada da avaliação.
- **Fora de circulação**: o catálogo apenas marca a situação da carta. Se cartas fora de circulação continuam saindo em boosters é decisão da feature `boosters`.
- **Regras de jogo**: a tabela de efetividade entre os seis elementos e o efeito executável das passivas pertencem à feature `partida`; este catálogo carrega os dados das cartas.
- **Passivas antes das regras**: as passivas são escritas antes de a `partida` definir as regras que vão executá-las. O risco de uma passiva não ser implementável foi aceito pelo autor e é mitigado pelo FR-020a: a passiva pode ser corrigida até a `partida` convergir.
- **Volume de revisão**: com a política do FR-039, a revisão individual se concentra nas cartas SR ou superior (todas as linguagens, os 40% de status e métodos HTTP no topo e os 10% de funções no topo, com os cortes iniciais). As descrições de cartas R, que são a maioria, são revisadas por amostragem.
- **IA generativa nos textos**: a escolha da ferramenta ou do modelo de IA é decisão de `/speckit-plan`. Não se envia nenhum dado pessoal a ela, só dados das cartas.
- **Idioma**: português no MVP; inglês é uma expansão futura prevista (FR-040). Os nomes das cartas seguem o original (Python, 404 Not Found, printf).
- **Arte**: a renderização das cartas pertence à feature `arte-cartas`; este catálogo só fornece os dados (ex.: cor da linguagem).
- **Dependências**: esta feature não depende de nenhuma outra. As features `contas-e-colecao`, `boosters`, `arte-cartas` e `partida` consomem a versão publicada do catálogo. A trava de correção de passivas (FR-020a) depende de a `partida` sinalizar quando convergiu.
- **Quem opera**: a geração é uma operação interna, executada pelo mantenedor, não por jogadores. Não envolve dados pessoais.
- **Fontes previstas** (`PROMPT_TCC.md` §4.1): GitHub Linguist (linguagens e cores), a lista padronizada de status e métodos HTTP, DevDocs (funções e palavras-chave; sem API oficial, então reaproveitar dados já obtidos é obrigatório) e Stack Exchange (popularidade, com limite diário de consultas). Os termos de cada uma ainda precisam ser verificados na fonte (FR-025).
