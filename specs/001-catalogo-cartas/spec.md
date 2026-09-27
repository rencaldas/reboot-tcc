# Feature Specification: Catálogo de cartas gerado automaticamente

**Feature Branch**: `feat/001-catalogo-cartas`

**Created**: 2026-09-27

**Status**: Draft

**Input**: User description: "Pipeline de geração do catálogo de cartas de um TCG de programação. Como mantenedor do jogo, quero gerar automaticamente milhares de cartas a partir de fontes públicas, porque criar carta por carta é inviável. Cada carta representa um elemento do universo da programação: linguagens, status HTTP (ex.: 404, 403), métodos HTTP (ex.: GET, POST) e funções de linguagens (ex.: printf, scanf, print). A raridade reflete a importância do elemento, medida por um indicador de popularidade comparável entre tipos, e cada tipo tem uma faixa de raridades permitida: linguagens muito importantes (Python, C, Java) chegam a UR; funções ficam nas raridades baixas; LR é escolhida manualmente e é mais rara que UR. Cada carta tem atributos de jogo que dependem da raridade e não mudam entre execuções. Rodar o pipeline de novo com os mesmos dados produz exatamente o mesmo catálogo, e cartas já existentes nunca mudam de identidade nem são apagadas se já estiverem na coleção de algum jogador. O pipeline gera um relatório com a distribuição de cartas por tipo e raridade e lista as cartas que precisam de revisão manual (ex.: nomes genéricos com popularidade inflada). O catálogo não contém texto copiado das fontes nem logos de terceiros e registra a origem de cada carta. Uma fonte que falhar não derruba a execução inteira: o relatório indica o que ficou de fora." — Complemento do autor (2026-09-27): "Cartas terão ataque, defesa e HP. Futuramente podemos colocar dano crítico e chance de crítico etc. Elementos todas as cartas terão; a partir de SR, cada carta terá uma passiva única."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Gerar o catálogo a partir das fontes públicas (Priority: P1)

O mantenedor do jogo executa a geração do catálogo e obtém, sem criar nenhuma carta à mão, milhares de cartas de quatro tipos: linguagens de programação, status HTTP, métodos HTTP e funções de linguagens. Cada carta sai com nome, tipo, raridade, elemento, atributos de jogo (ataque, defesa e HP) e o registro de onde veio.

**Why this priority**: sem catálogo não existe jogo, e o catálogo gerado automaticamente é o núcleo da contribuição do projeto (Option B da avaliação da ideia, `.specify/assessments/tcg-programacao/concept.md`). Todas as outras features (coleção, boosters, arte, partida) consomem este resultado.

**Independent Test**: executar a geração sobre um conjunto fixo de dados de entrada e verificar que o catálogo resultante tem cartas dos quatro tipos, com todos os campos obrigatórios preenchidos e raridades dentro da faixa de cada tipo.

**Acceptance Scenarios**:

1. **Given** dados de entrada disponíveis para os quatro tipos, **When** o mantenedor executa a geração, **Then** o catálogo contém cartas dos quatro tipos, e cada carta tem identificador, nome, tipo, raridade, elemento, ataque, defesa, HP e origem.
2. **Given** uma linguagem entre as mais populares do seu tipo (ex.: Python), **When** a raridade é calculada, **Then** ela recebe UR, a raridade mais alta permitida a linguagens; **and given** uma função entre as menos populares do seu tipo, **Then** ela recebe R.
3. **Given** qualquer carta gerada sem override manual, **When** sua raridade é conferida, **Then** ela está dentro da faixa permitida ao seu tipo.
4. **Given** um item de fonte cuja popularidade fica abaixo do mínimo definido para o seu tipo, **When** a geração é executada, **Then** o item não vira carta e é contabilizado no relatório como filtrado.
5. **Given** qualquer carta gerada, **When** seus atributos são conferidos, **Then** ataque, defesa e HP estão dentro da faixa definida para a raridade da carta.

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
4. **Given** as cartas mais populares de cada tipo, **When** a geração é executada, **Then** elas aparecem na fila de revisão como topo do tipo.

---

### User Story 4 - Textos próprios das cartas, com revisão humana (Priority: P2)

O mantenedor gera em lote, com IA generativa, rascunhos de descrição para todas as cartas e de uma passiva única para cada carta SR ou superior. Depois revisa cada rascunho, aprovando, editando ou rejeitando. Só cartas com os textos aprovados entram numa versão publicada. Todo o processo fica registrado: o que foi gerado automaticamente, o que o autor editou e quando cada texto foi revisado.

**Why this priority**: sem textos aprovados nenhuma carta pode ser publicada (FR-037), e a descrição é o que dá identidade à carta perante o jogador. Mesmo assim, a geração e a estabilidade do catálogo (P1) já podem ser testadas em versões de rascunho, sem textos. O registro do processo também alimenta a declaração de uso de IA do TCC (`docs/tcc/uso-de-ia.md`).

**Independent Test**: gerar rascunhos para um conjunto pequeno de cartas (com pelo menos uma R e uma SR+), aprovar alguns, editar um e rejeitar um; publicar uma versão e verificar que só as cartas com textos aprovados entram nela e que cada texto registra seu histórico.

**Acceptance Scenarios**:

1. **Given** cartas sem texto, **When** o mantenedor pede a geração de rascunhos, **Then** cada carta recebe um rascunho de descrição, cada carta SR ou superior recebe também um rascunho de passiva, e cartas R não recebem passiva.
2. **Given** um rascunho, **When** o mantenedor o aprova, edita ou rejeita, **Then** a decisão fica registrada com data, e um texto editado fica marcado como editado pelo autor.
3. **Given** uma carta com algum texto obrigatório ainda não aprovado, **When** uma versão é publicada, **Then** essa carta não entra na versão publicada e o relatório a lista como pendente de texto.
4. **Given** textos já gerados, **When** a geração do catálogo é executada de novo, **Then** nenhum texto é gerado de novo e o catálogo sai idêntico.
5. **Given** duas passivas com o mesmo texto, **When** o mantenedor tenta aprovar a segunda, **Then** a duplicata é sinalizada e a aprovação só é aceita depois que a passiva for diferenciada.

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

- **Funções com o mesmo nome em linguagens diferentes** (ex.: `print`): viram cartas distintas, uma por linguagem; se a tag de popularidade for genérica, todas entram na fila de revisão.
- **Empate de popularidade no limite entre duas raridades**: resolvido por uma regra de desempate determinística, que não depende da ordem em que os dados foram lidos.
- **Tipo com poucos itens** (ex.: métodos HTTP): a distribuição por percentil continua produzindo só raridades permitidas ao tipo, e o relatório mostra a distribuição real.
- **Fonte com falha vs. item que sumiu**: uma falha de fonte nunca é interpretada como desaparecimento de itens; nenhuma carta é marcada como fora de circulação por causa de uma falha.
- **Item sem tag correspondente** (popularidade indeterminável): não vira carta automaticamente, é contado como "sem correspondência" no relatório e pode entrar por alias ou inclusão manual.
- **Identificadores compostos no tipo Função**: só identificadores simples entram; os demais são contabilizados como filtrados.
- **Limite de uso de uma fonte atingido no meio da execução**: a execução para de consultar essa fonte, trata os itens não obtidos como falha de fonte (não como desaparecimento) e os lista no relatório.
- **Curadoria que alteraria uma carta já publicada**: uma exclusão marca a carta como fora de circulação (nunca a remove); uma promoção a LR ou uma troca de elemento não é aplicada e aparece no relatório como curadoria não aplicada.
- **Tipo com regra temática que deixa algum elemento sem nenhuma carta** (ex.: as classes de status HTTP são menos numerosas que os elementos): aceito; o relatório mostra a distribuição por elemento de cada tipo.
- **Rascunho de texto que reproduz trecho de documentação** conhecido: a revisão humana rejeita, e a carta recebe um novo rascunho.
- **Carta rejeitada várias vezes**: continua em rascunho, fora das versões publicadas, até que o autor aprove um texto (gerado ou escrito por ele).
- **Dados novos que dariam outra raridade a uma carta publicada**: a carta mantém a raridade publicada e o relatório registra a divergência (insumo para analisar o viés da métrica de popularidade).

## Requirements *(mandatory)*

### Functional Requirements

**Geração e tipos de carta**

- **FR-001**: O sistema DEVE gerar cartas automaticamente, sem criação manual carta a carta, para quatro tipos: Linguagem de programação, Status HTTP, Método HTTP e Função (funções e palavras-chave de linguagens).
- **FR-002**: Cada tipo DEVE ter uma fonte primária que define quais itens existem; as demais fontes apenas complementam dados (ex.: popularidade).
- **FR-003**: A popularidade de cada item DEVE ser medida pela contagem de perguntas da tag correspondente no Stack Overflow, única régua usada para comparar itens de tipos diferentes.
- **FR-004**: Itens com popularidade abaixo do mínimo definido para o seu tipo NÃO DEVEM virar carta.
- **FR-005**: Itens sem tag correspondente NÃO DEVEM virar carta automaticamente e DEVEM ser contabilizados como "sem correspondência".
- **FR-006**: No tipo Função, só identificadores simples DEVEM virar carta.

**Raridade**

- **FR-007**: A escala de raridade DEVE ser, em ordem crescente: R, SR, SSR, UR, LR.
- **FR-008**: A raridade de cada carta DEVE ser a posição (percentil) da sua popularidade dentro do seu tipo, limitada à faixa do tipo: Linguagem de SR a UR; Status HTTP de R a SSR; Método HTTP de R a SSR; Função de R a SR. Os cortes de percentil DEVEM ser parâmetros registrados em cada versão do catálogo.
- **FR-009**: A raridade LR DEVE existir apenas por curadoria manual e nunca DEVE resultar do cálculo estatístico.
- **FR-010**: Empates de popularidade DEVEM ser resolvidos por uma regra determinística, independente da ordem de leitura dos dados.

**Atributos, elemento e passiva**

- **FR-011**: Toda carta DEVE ter ataque, defesa e HP, com valores dentro da faixa definida para a sua raridade e derivados de forma determinística da identidade da carta (a mesma carta sempre recebe os mesmos valores).
- **FR-012**: As faixas de atributos por raridade DEVEM ser parâmetros registrados em cada versão do catálogo.
- **FR-013**: O modelo de carta DEVE permitir acrescentar atributos no futuro (ex.: dano crítico, chance de crítico) sem alterar os valores dos atributos já existentes.
- **FR-014**: Toda carta DEVE ter exatamente um elemento, de um conjunto de pelo menos 6 elementos com tema de programação, atribuído em três camadas, da de maior precedência para a de menor: (a) a curadoria pode definir o elemento de uma carta ainda não publicada; (b) sem curadoria, nos tipos em que os dados do item sustentam uma regra temática (ex.: status HTTP pela classe 1xx–5xx), vale essa regra; (c) nos demais casos, o elemento é distribuído de forma determinística e equilibrada entre os elementos, a partir da identidade da carta.
- **FR-015**: Toda carta de raridade SR ou superior DEVE ter uma passiva única, que nenhuma outra carta compartilha, temática em relação ao que a carta representa; cartas R NÃO DEVEM ter passiva. Esta feature produz as passivas e as descrições das cartas (ver "Textos da carta").
- **FR-015a**: A lista de elementos, os tipos que têm regra temática e as regras de cada um DEVEM ser parâmetros registrados em cada versão do catálogo.

**Identidade, reprodutibilidade e versões**

- **FR-016**: Cada carta DEVE ter um identificador estável, derivado do seu tipo e da chave do item na fonte primária. Um identificador NUNCA DEVE ser reatribuído a outra carta.
- **FR-017**: Execuções com as mesmas entradas, a mesma curadoria e os mesmos parâmetros DEVEM produzir catálogos idênticos.
- **FR-018**: Os dados obtidos das fontes em cada execução DEVEM ser armazenados e identificados, para que qualquer versão do catálogo possa ser reproduzida sem consultar as fontes de novo.
- **FR-019**: Cada execução que altera o conteúdo do catálogo DEVE gerar uma nova versão numerada; versões anteriores DEVEM ser preservadas sem alteração.
- **FR-020**: Depois que uma versão é marcada como publicada, nenhuma versão posterior PODE alterar identificador, nome, tipo, raridade, elemento ou atributos de uma carta publicada, nem removê-la.
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

- **FR-029**: Cada execução DEVE produzir um relatório com: total de cartas; distribuição por tipo × raridade; distribuição por elemento; tempo de geração; taxa de itens sem correspondência, por tipo; itens filtrados, por motivo; fontes que falharam e o impacto de cada falha; diferenças em relação à versão anterior (cartas novas e cartas que saíram de circulação); situação dos textos (quantas descrições e passivas estão pendentes, em rascunho, aprovadas ou rejeitadas); e a fila de revisão.
- **FR-030**: A fila de revisão DEVE sinalizar, com motivo: (a) as cartas no topo de popularidade de cada tipo; (b) cartas de nome genérico cuja popularidade provavelmente mede o conceito e não o item; (c) cartas publicadas cuja raridade recalculada seria diferente da publicada; (d) curadoria não aplicada (FR-022).

**Curadoria**

- **FR-031**: O mantenedor DEVE poder registrar, sempre com motivo: overrides para LR, escolha manual de elemento, aliases de nome para tag, inclusões e exclusões manuais, e decisões sobre itens da fila de revisão. Toda execução DEVE aplicar essas decisões de forma determinística.
- **FR-032**: Um item da fila de revisão marcado como resolvido NÃO DEVE reaparecer na fila em execuções seguintes.

**Textos da carta**

- **FR-033**: O sistema DEVE gerar em lote, com IA generativa, rascunhos próprios de descrição para toda carta e de passiva para toda carta SR ou superior, sem reproduzir texto das fontes (FR-026).
- **FR-034**: O mantenedor DEVE poder aprovar, editar ou rejeitar cada rascunho. Uma carta com rascunho rejeitado pode receber um novo rascunho, gerado só para ela.
- **FR-035**: Cada texto DEVE registrar sua situação (pendente, rascunho, aprovado ou rejeitado) e seu histórico: se foi gerado automaticamente ou escrito/editado pelo autor, qual processo de geração foi usado, e a data e a decisão da revisão.
- **FR-036**: Um texto gerado DEVE ser armazenado e reaproveitado nas execuções seguintes, e nunca gerado de novo automaticamente. Uma nova geração só ocorre para cartas sem texto ou por pedido explícito do mantenedor para uma carta específica.
- **FR-037**: Só cartas com todos os textos obrigatórios aprovados (descrição; e passiva, se SR ou superior) PODEM entrar numa versão publicada. As demais ficam em rascunho, e o relatório as lista como pendentes de texto.
- **FR-038**: Duas cartas NÃO PODEM ter passivas aprovadas com o mesmo texto. Uma duplicata é sinalizada e só pode ser aprovada depois de diferenciada.

### Key Entities *(include if feature involves data)*

- **Carta**: unidade colecionável do jogo. Tem identificador estável, nome, tipo, raridade, elemento, atributos (ataque, defesa, HP, extensível), passiva (apenas SR ou superior), origem, situação (em circulação ou fora de circulação) e a versão do catálogo em que foi publicada pela primeira vez.
- **Tipo de carta**: categoria de origem do item (Linguagem, Status HTTP, Método HTTP, Função). Define a fonte primária, a faixa de raridades permitida e o mínimo de popularidade. Não se confunde com Elemento.
- **Elemento**: afinidade de jogo (pelo menos 6, com tema de programação) usada pela mecânica de efetividade entre elementos, cujas regras pertencem à feature `partida`. Toda carta tem exatamente um.
- **Raridade**: R, SR, SSR, UR e LR, em ordem crescente, cada uma com sua faixa de atributos.
- **Passiva**: efeito especial único de uma carta SR ou superior, temático em relação ao que a carta representa (ex.: a carta do status 401 Unauthorized impede o oponente de jogar por 1 turno). Nesta feature, a passiva é um texto aprovado; a regra executável correspondente pertence à feature `partida`.
- **Texto da carta**: descrição (toda carta) ou passiva (SR ou superior). Tem situação (pendente, rascunho, aprovado ou rejeitado) e histórico (origem automática ou autoral, processo de geração usado, data e decisão da revisão).
- **Fonte de dados**: provedor externo de itens ou de popularidade, com termos de uso, licença e limites de uso.
- **Coleta**: cópia dos dados obtidos das fontes numa execução, identificada, que permite reproduzir a versão gerada a partir dela.
- **Curadoria**: conjunto de decisões manuais (overrides LR, escolhas de elemento, aliases, inclusões, exclusões e resoluções de revisão), cada uma com motivo.
- **Versão do catálogo**: conjunto imutável de cartas, junto com os parâmetros (cortes de percentil, faixas de atributos, mínimos por tipo), a curadoria e a coleta que a produziram. Pode ser rascunho ou publicada.
- **Relatório de geração**: resumo de uma execução (FR-029).
- **Item de revisão**: carta sinalizada, com motivo, situação (pendente ou resolvido) e a decisão tomada.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A primeira versão publicada do catálogo contém pelo menos 1.000 cartas, cobrindo os quatro tipos.
- **SC-002**: Duas execuções com as mesmas entradas, curadoria e parâmetros produzem catálogos idênticos em 100% das cartas e dos campos.
- **SC-003**: 100% das cartas têm raridade dentro da faixa do seu tipo, ou LR com override registrado; 0 cartas LR sem override.
- **SC-004**: 100% das cartas de qualquer versão publicada existem em todas as versões posteriores, com identificador, nome, tipo, raridade, elemento e atributos inalterados.
- **SC-005**: Com qualquer uma das fontes indisponível, a execução termina, o relatório identifica a fonte e o que ficou de fora, e nenhuma carta publicada é perdida ou tirada de circulação por causa da falha.
- **SC-006**: 100% das cartas têm origem registrada, e 0 campos de texto reproduzem trechos das fontes.
- **SC-007**: Nenhuma execução ultrapassa os limites de uso publicados de qualquer fonte.
- **SC-008**: Uma versão do catálogo pode ser regenerada a partir da coleta armazenada, sem consultar nenhuma fonte externa, com resultado idêntico ao original.
- **SC-009**: Num conjunto de teste com casos conhecidos de cada categoria de revisão (FR-030), 100% dos casos aparecem na fila de revisão com o motivo correto.
- **SC-010**: O relatório de cada execução traz, sem cálculo manual adicional, as métricas previstas para a monografia: distribuição por tipo e raridade, tempo de geração e taxa de itens sem correspondência.
- **SC-011**: Em qualquer versão publicada, 100% das cartas têm descrição aprovada, 100% das cartas SR ou superior têm passiva aprovada, e 0 passivas se repetem.
- **SC-012**: 100% dos textos de uma versão publicada têm histórico completo (origem, processo de geração, data e decisão da revisão), o que permite contar quantos foram aprovados sem edição, editados ou escritos pelo autor.
- **SC-013**: 100% das cartas têm exatamente um elemento, e 100% das cartas com escolha manual de elemento saem com o elemento escolhido.

## Assumptions

- **Escala de raridade**: R < SR < SSR < UR < LR, conforme a ideia original do autor (`PROMPT_TCC.md` §3). O §4.4 ainda a lista como premissa não confirmada, então o `/speckit-clarify` deve confirmá-la.
- **Volume-alvo**: pelo menos 1.000 cartas (leitura mínima de "milhares"). O volume real só se confirma implementando o pipeline (`decision.md`); o número a confirmar no `/speckit-clarify`.
- **Quando a estabilidade passa a valer**: a garantia de que cartas não mudam (FR-020) vale a partir da primeira versão marcada como publicada. Antes disso, durante o desenvolvimento, as versões são rascunho e cortes, faixas e mínimos podem ser ajustados livremente.
- **Rebalanceamento**: alterar deliberadamente atributos de cartas já publicadas está fora do escopo desta feature. Se for necessário no futuro, será uma decisão explícita registrada em ADR.
- **Tipos no MVP**: só os quatro do FR-001. As expansões candidatas (Wikidata, MDN, exceções e erros de runtime, estruturas de dados, padrões de projeto, ferramentas como Git e Docker) ficam fora; o modelo permite acrescentar tipos sem alterar cartas existentes.
- **Palavras-chave** das linguagens entram no tipo Função.
- **LR** pode ser atribuída a uma carta de qualquer tipo; o §4.1 não restringe o tipo.
- **Limite por tipo**: não há teto de cartas por tipo no MVP; o volume é controlado pelo mínimo de popularidade de cada tipo. A confirmar no `/speckit-clarify`.
- **Topo do tipo para revisão**: a quantidade de cartas do topo de cada tipo que entra na fila é um parâmetro, com valor a definir no `/speckit-clarify`.
- **Viés temporal da métrica**: a contagem de perguntas do Stack Overflow favorece tecnologias antigas (`research.md`). O MVP não aplica correção; o viés é registrado como limitação, e o relatório mede as divergências (FR-030 c). A mitigação, se houver, é questão aberta herdada da avaliação.
- **Fora de circulação**: o catálogo apenas marca a situação da carta. Se cartas fora de circulação continuam saindo em boosters é decisão da feature `boosters`.
- **Regras de jogo**: o efeito de elementos e de passivas em partida pertence à feature `partida`; este catálogo carrega os dados das cartas.
- **Elementos (decisão do autor, Q1 = atribuição mista)**: a lista concreta de elementos (mínimo 6), a tabela de efetividade entre eles, quais tipos têm regra temática e quais são essas regras ainda não foram definidas. A confirmar no `/speckit-clarify`.
- **Textos (decisão do autor, Q2 = gerados nesta feature)**: as passivas são escritas antes de a feature `partida` definir as regras que vão executá-las. É um risco aceito: a `partida` pode encontrar passivas que não consegue implementar. Duas questões ficam para o `/speckit-clarify`: se descrição e passiva de uma carta publicada podem ser corrigidas depois, e com que registro (hoje fora da lista de campos congelados do FR-020); e o que fazer com uma passiva impossível de implementar.
- **Volume de revisão**: com revisão humana de todos os textos (`PROMPT_TCC.md` §4.2) e pelo menos 1.000 cartas, a primeira versão publicada exige pelo menos 1.000 descrições aprovadas, mais as passivas das cartas SR ou superior. Como a publicação é por carta (FR-037), o catálogo pode ser publicado em partes, à medida que a revisão avança. Se a revisão é de todos os textos ou por amostragem é uma política a confirmar no `/speckit-clarify`.
- **IA generativa nos textos**: a escolha da ferramenta ou do modelo de IA é decisão de `/speckit-plan`. Não se envia nenhum dado pessoal a ela, só dados das cartas.
- **Arte**: a renderização das cartas pertence à feature `arte-cartas`; este catálogo só fornece os dados (ex.: cor da linguagem).
- **Dependências**: esta feature não depende de nenhuma outra. As features `contas-e-colecao`, `boosters`, `arte-cartas` e `partida` consomem a versão publicada do catálogo.
- **Quem opera**: a geração é uma operação interna, executada pelo mantenedor, não por jogadores. Não envolve dados pessoais.
- **Fontes previstas** (`PROMPT_TCC.md` §4.1): GitHub Linguist (linguagens e cores), a lista padronizada de status e métodos HTTP, DevDocs (funções e palavras-chave; sem API oficial, então reaproveitar dados já obtidos é obrigatório) e Stack Exchange (popularidade, com limite diário de consultas). Os termos de cada uma ainda precisam ser verificados na fonte (FR-025).
