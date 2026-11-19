---

description: "Lista de tarefas da feature 002-prototipo-reboot"
---

# Tasks: Protótipo jogável do Reboot

**Input**: documentos de design em `specs/002-prototipo-reboot/`

**Prerequisites**: [plan.md](plan.md), [spec.md](spec.md), [research.md](research.md), [data-model.md](data-model.md), [contracts/](contracts/), [quickstart.md](quickstart.md)

**Tests**: obrigatórios. A constituição (Princípio II) exige testes escritos **antes** da implementação no domínio crítico: regras e validação dos puzzles, jornada e progresso. Os testes de cada fase são escritos primeiro e precisam **falhar** antes da implementação correspondente.

**Organization**: as tarefas estão agrupadas por user story, para que cada uma possa ser implementada e testada de forma independente.

## Formato: `[ID] [P?] [Story] Descrição`

- **[P]**: pode rodar em paralelo (arquivos diferentes, sem dependência de tarefa incompleta).
- **[Story]**: user story a que a tarefa pertence (US1, US2, US3).
- **[Autores]**: tarefa feita pelos autores, não pela IA (aprovação de conteúdo, validação manual, configuração no GitHub).
- Todos os caminhos de código são relativos a `apps/reboot/`, exceto quando indicado.

---

## Phase 1: Setup (infraestrutura compartilhada)

**Purpose**: criar o projeto, o ambiente reprodutível e o CI ([research.md](research.md) R1, R3, R10 a R12).

- [ ] T001 Criar `apps/reboot/` com Vite no template React + TypeScript, removendo todos os arquivos e trechos de exemplo do template (Princípio I: sem código morto). Ajustar `apps/reboot/package.json` com `name: "reboot"`, `version: "0.1.0"`, `private: true`, `engines.node: ">=26"` e os scripts `dev`, `build`, `preview`, `test`, `lint` e `typecheck`.
- [ ] T002 [P] Criar `apps/reboot/.nvmrc` com a versão `26` do Node (R11).
- [ ] T003 [P] Configurar o TypeScript em modo estrito em `apps/reboot/tsconfig.json`, com `strict`, `noUnusedLocals` e `noUnusedParameters` ativados.
- [ ] T004 Configurar `apps/reboot/vite.config.ts` com `base: '/reboot-tcc/'` e injeção da versão do `package.json` como constante global de build (FR-017, R9), declarando o tipo em `apps/reboot/src/vite-env.d.ts`.
- [ ] T005 [P] Configurar o Vitest em `apps/reboot/vitest.config.ts`: ambiente `node` por padrão e `jsdom` para `tests/interface/**`. Criar `apps/reboot/tests/setup.ts` com a limpeza da Testing Library e os matchers de DOM.
- [ ] T006 [P] Criar o helper de acessibilidade `apps/reboot/tests/interface/axe.ts`, que roda o `axe-core` num container renderizado e falha listando cada violação encontrada.
- [ ] T007 [P] Configurar ESLint (regras de TypeScript e de hooks do React) e Prettier em `apps/reboot/eslint.config.js` e `apps/reboot/.prettierrc`. Incluir uma regra que proíbe, em `src/dominio/**`, importar React, `src/interface/**` ou usar APIs do navegador (R2).
- [ ] T008 [P] Criar `.github/workflows/ci.yml` (na raiz do repositório): em toda pull request, usar o Node do `apps/reboot/.nvmrc`, rodar `npm ci`, `lint`, `typecheck`, `test` e `build` dentro de `apps/reboot/` (R10, R11).
- [ ] T009 [P] Criar `.github/workflows/pages.yml` (na raiz do repositório): a cada push na `main`, gerar o build de `apps/reboot/` e publicá-lo no GitHub Pages pelas actions oficiais de Pages (R10).
- [ ] T010 Ajustar `apps/reboot/index.html` com `lang="pt-BR"`, título "Reboot" e nenhum recurso externo (fontes, scripts ou estilos de outro domínio, R8). Fazer `apps/reboot/src/main.tsx` montar `App` e importar `src/interface/estilo/base.css`.

**Checkpoint**: `npm run lint`, `npm run typecheck`, `npm test` (sem testes ainda) e `npm run build` rodam sem erro.

---

## Phase 2: Foundational (pré-requisitos bloqueantes)

**Purpose**: tipos, jornada, progresso e as peças de interface usadas pelas duas etapas.

**⚠️ CRITICAL**: nenhuma user story começa antes desta fase terminar.

### Tipos e regras da jornada

- [ ] T011 [P] Criar os tipos básicos do domínio em `apps/reboot/src/dominio/tipos.ts`, conforme o [data-model.md](data-model.md):
  - `Bit` (`0` ou `1`);
  - `Canal` ("Inteiro de 0 a 255");
  - `Cor` (`{ r, g, b }`);
  - `Direcao` (`'norte' | 'leste' | 'sul' | 'oeste'`);
  - `Posicao` (`{ linha, coluna }`, "a partir de 0");
  - `TipoPorta` (`'AND' | 'OR' | 'NOT'`).
- [ ] T012 [P] Criar os tipos de conteúdo em `apps/reboot/src/conteudo/tipos.ts`, conforme o [data-model.md](data-model.md):
  - `Etapa`;
  - `RegraFase`, com `id` ("Único e estável entre versões"), `etapa`, `limiteErros` ("`null` na 1ª fase da etapa; `3` na 2ª") e `podePular` ("`true` só quando `limiteErros` é `null`");
  - `FaseCor`, com `alvo`, `descricaoAlvo` ("Descrição da cor-alvo em palavras, sem números") e `imagemPixels` opcional;
  - `FaseCanos`, com `tipo` (`'prever' | 'escolher'`), `dimensoes`, `fixas`, `bandeja`, `entradas` e `combinacoes`;
  - campos comuns a toda fase: `dicas` ("Exatamente 3, do mais vago ao mais direto"), `devolveAoMundo` e `falaGuia` (`{ inicio, conclusao }`);
  - as variantes de `Peca`: `fonte`, `saida`, `fio` (`portas: [Direcao, Direcao]`) e `juncao` ("AND e OR têm 2 entradas; NOT tem 1");
  - `FatoHistorico`, com `fonte: { titulo, autoria, url, acessadoEm }`.
- [ ] T013 [P] Escrever primeiro o teste `apps/reboot/tests/conteudo/jornada.test.ts`, conforme as invariantes de estrutura do [contrato de conteúdo](contracts/conteudo-fase.md):
  - "Existem 2 etapas, `cor` antes de `canos`, cada uma com exatamente 2 fases";
  - os `id` de fase são únicos;
  - a 1ª fase de cada etapa tem `limiteErros: null` e `podePular: true`, e a 2ª tem `limiteErros: 3` e `podePular: false`.
- [ ] T014 Criar `apps/reboot/src/conteudo/jornada.ts` com a ordem das etapas e as regras de cada fase (`cor-1`, `cor-2`, `canos-1`, `canos-2`), fazendo o T013 passar.

### Testes do domínio crítico (escrever primeiro e ver falhar)

- [ ] T015 [P] Escrever `apps/reboot/tests/dominio/jornada/tentativas.test.ts`, cobrindo o FR-003, o FR-009, o FR-011 e o FR-018:
  - **Três desfechos de confirmação:** incompleta, errada e certa.
  - **Incompleta não muda nada:** não altera o contador nem libera dica.
  - **Limite 3:** da 1ª à 3ª confirmação errada a fase continua, e a 4ª causa a derrota, que zera o contador.
  - **Fase sem limite:** nunca há derrota.
  - **Opção de acessibilidade:** com `semLimiteCor` ligado, as fases de cor ficam sem limite e as de canos mantêm o limite.
  - **Dicas:** `dicasLiberadas = min(errosConfirmacao, 3)`, nenhuma antes do 1º erro, e a derrota zera as dicas.
  - **Pular:** a opção de acessibilidade não muda o fato de a fase poder ser pulada.
- [ ] T016 [P] Escrever `apps/reboot/tests/dominio/jornada/ordem.test.ts`, cobrindo as transições de fase e de jornada do [data-model.md](data-model.md):
  - a fase atual é a primeira que não está concluída nem pulada;
  - uma fase fica bloqueada até a anterior ser concluída ou pulada;
  - pular só é permitido quando `podePular` é verdadeiro;
  - perder uma fase não altera as fases já concluídas;
  - a jornada termina quando todas as fases estão concluídas ou puladas.
- [ ] T017 [P] Escrever `apps/reboot/tests/dominio/progresso/progresso.test.ts` com todos os "Testes obrigatórios" do [contrato do progresso](contracts/progresso-salvo.md):
  - **Casos que resultam no progresso inicial:** chave ausente, JSON inválido, versão desconhecida, fase inexistente, status inválido, contador fora da faixa, campo extra, e fase com limite marcada como pulada.
  - **Faixa do contador com acessibilidade:** com `semLimiteCor`, o contador de uma fase de cor pode passar do limite.
  - **Ida e volta:** gravar e ler devolve o mesmo progresso.
  - **Armazenamento indisponível:** um armazenamento que lança exceção não interrompe o jogo.
  - **Apagar:** apagar remove a chave `reboot:progresso`.

### Implementação do domínio da jornada e do progresso

- [ ] T018 [P] Implementar `apps/reboot/src/dominio/jornada/tentativas.ts`: o registro de cada confirmação, o limite efetivo (incluindo `semLimiteCor`) e as dicas liberadas, fazendo o T015 passar.
- [ ] T019 [P] Implementar `apps/reboot/src/dominio/jornada/ordem.ts`: a fase atual, os bloqueios, pular e o fim da jornada, recebendo as regras das fases como parâmetro e fazendo o T016 passar.
- [ ] T020 Implementar `apps/reboot/src/dominio/progresso/validar.ts`, que valida o formato da versão de esquema `1` do [contrato do progresso](contracts/progresso-salvo.md) contra as regras das fases recebidas como parâmetro.
- [ ] T021 Implementar `apps/reboot/src/dominio/progresso/armazenamento.ts`, com ler, salvar e apagar sobre uma interface injetada de armazenamento chave-valor (`getItem`, `setItem`, `removeItem`) e a chave `reboot:progresso`. Toda exceção do armazenamento é absorvida. Depende do T020 e faz o T017 passar.

### Base da interface (compartilhada pelas etapas)

- [ ] T022 [P] Criar `apps/reboot/src/interface/estilo/base.css`:
  - pilha de fontes do sistema (R8);
  - foco sempre visível;
  - contraste de texto no nível AA da WCAG 2.2;
  - alvos de toque de no mínimo 24 × 24 px CSS (WCAG 2.2);
  - layout pensado para retrato, sem bloquear a paisagem (FR-018).
- [ ] T023 [P] Criar `apps/reboot/src/interface/acessibilidade/Anunciador.tsx`, com uma região `aria-live` única e um hook `useAnunciar` para mensagens ao leitor de tela (R4).
- [ ] T024 [P] Criar `apps/reboot/src/interface/componentes/Versao.tsx`, que exibe a versão do protótipo injetada no build (FR-017).
- [ ] T025 Escrever primeiro `apps/reboot/tests/interface/fundacao.test.tsx`, sem violações do axe em nenhum dos componentes:
  - **`ContadorTentativas`:** ao abrir uma fase com limite, informa em texto que é possível errar 3 vezes e mostra em texto quantas tentativas restam (FR-003).
  - **`PainelDicas`:** mostra só as dicas liberadas, nenhuma antes do 1º erro (FR-011).
  - **`Configuracoes`, opção de acessibilidade:** liga e desliga a opção, e ela fica salva (FR-018).
  - **`Configuracoes`, apagar progresso:** pede confirmação num diálogo do próprio jogo antes de apagar, e devolve o foco ao fechar (FR-021).
- [ ] T026 [P] Implementar `apps/reboot/src/interface/componentes/ContadorTentativas.tsx` (parte do T025).
- [ ] T027 [P] Implementar `apps/reboot/src/interface/componentes/PainelDicas.tsx` (parte do T025).
- [ ] T028 Implementar `apps/reboot/src/interface/estado/useProgresso.ts`, que liga o domínio do progresso ao `window.localStorage` e expõe ler, atualizar e apagar.
- [ ] T029 Implementar `apps/reboot/src/interface/componentes/Configuracoes.tsx`, com a opção de acessibilidade `semLimiteCor` e o apagar progresso com diálogo próprio, fazendo o T025 passar. Depende do T028.
- [ ] T030 Implementar `apps/reboot/src/interface/App.tsx`, que escolhe a tela pela fase atual da jornada e mostra `Versao`, `Configuracoes` e `Anunciador` em todas as telas.

**Checkpoint**: domínio da jornada e do progresso com testes verdes. A interface base renderiza, sem violações do axe.

---

## Phase 3: User Story 1 - Religar as cores (Priority: P1) 🎯 MVP

**Goal**: as duas fases de cor jogáveis, com conferência por canal, dicas, limite, imagem de pixels e explicação da etapa.

**Independent Test**: jogar só a etapa de cor, do início ao fim, e responder corretamente o que significa (255, 0, 0). Automaticamente, os testes de `tests/dominio/cor`, `tests/conteudo/cor.test.ts` e `tests/interface/fase-cor.test.tsx` passam.

### Testes da User Story 1 (escrever primeiro e ver falhar) ⚠️

- [ ] T031 [P] [US1] Escrever `apps/reboot/tests/dominio/cor/conferencia.test.ts`, cobrindo o FR-005:
  - cada canal recebe `'certo'`, `'maior'` ("o valor certo está acima do inserido") ou `'menor'` ("o valor certo está abaixo do inserido");
  - a fase só é concluída quando os três canais são exatamente iguais aos da cor-alvo;
  - o resultado nunca expõe o valor da cor-alvo;
  - confirmar duas vezes a mesma cor errada é avaliado como duas confirmações.
- [ ] T032 [P] [US1] Escrever `apps/reboot/tests/dominio/cor/canal.test.ts`, cobrindo o FR-004:
  - toda fase começa em `{ r: 0, g: 0, b: 0 }`;
  - o ajuste de 1 em 1 fica limitado entre 0 e 255;
  - para o valor digitado, o comportamento é proposto e precisa da confirmação dos autores (puzzles CHK016) antes de implementar: aceitar só inteiros de 0 a 255, e qualquer outro valor (vazio, decimal, texto ou fora da faixa) não altera o canal.
- [ ] T033 [P] [US1] Escrever `apps/reboot/tests/conteudo/cor.test.ts`, conforme o [contrato de conteúdo](contracts/conteudo-fase.md):
  - todo canal do `alvo` é inteiro entre 0 e 255;
  - toda fase de cor tem `descricaoAlvo` não vazia e sem algarismos;
  - toda fase tem exatamente 3 dicas não vazias;
  - `cor-2` tem `imagemPixels`;
  - a etapa tem explicação;
  - a cor-alvo, usada como resposta, é aceita pela conferência (SC-006).
- [ ] T034 [P] [US1] Escrever `apps/reboot/tests/interface/fase-cor.test.tsx`:
  - **Ajuste em tempo real:** controle deslizante, botões de 1 em 1 e digitação atualizam a amostra e os três valores na hora (US1 cenário 1).
  - **Confirmação errada:** marca só "maior" ou "menor" nos canais errados, anuncia isso ao leitor de tela e libera a dica seguinte (US1 cenário 3).
  - **Confirmação certa:** conclui a fase (US1 cenário 2).
  - **Limite em `cor-2`:** mostra o aviso e as tentativas restantes, e na 4ª confirmação errada a fase recomeça em (0, 0, 0), sem dicas.
  - **Opção de acessibilidade:** com ela ligada, `cor-2` não tem limite.
  - **Pular:** só `cor-1` pode ser pulada.
  - **Descrição da cor-alvo:** está disponível como texto.
  - **Acessibilidade:** os canais têm rótulos acessíveis e não há violações do axe.
- [ ] T035 [P] [US1] Escrever `apps/reboot/tests/interface/imagem-pixels.test.tsx`:
  - cada pixel da imagem pode ser alcançado por teclado e por toque, e mostra seus três valores (FR-006, US1 cenário 4);
  - não há violações do axe.

### Implementação da User Story 1

- [ ] T036 [P] [US1] Implementar `apps/reboot/src/dominio/cor/canal.ts` (valor inicial, ajuste de 1 em 1 e leitura do valor digitado), fazendo o T032 passar.
- [ ] T037 [P] [US1] Implementar `apps/reboot/src/dominio/cor/conferencia.ts`, fazendo o T031 passar.
- [ ] T038 [US1] Rascunhar o conteúdo da etapa de cor em `apps/reboot/src/conteudo/cor.ts`, seguindo a clarificação de 2026-11-19 (a IA rascunha):
  - `cor-1` e `cor-2`, cada uma com `alvo`, `descricaoAlvo` sem números, 3 dicas do mais vago ao mais direto, `devolveAoMundo` e `falaGuia`;
  - a `imagemPixels` de `cor-2`;
  - a explicação da etapa em poucas frases (FR-012).
- [ ] T039 [US1] **[Autores]** Revisar e aprovar `apps/reboot/src/conteudo/cor.ts`. Nada desse conteúdo entra na `main` sem aprovação. O T033 precisa passar.
- [ ] T040 [P] [US1] Implementar `apps/reboot/src/interface/componentes/SeletorCanal.tsx`, com controle deslizante, botões −1 e +1 e campo numérico, todos rotulados (FR-004).
- [ ] T041 [P] [US1] Implementar `apps/reboot/src/interface/componentes/AmostrasCor.tsx`, com as amostras lado a lado, a descrição em texto da cor-alvo e os valores atuais do jogador (FR-005, FR-018).
- [ ] T042 [US1] Implementar `apps/reboot/src/interface/telas/FaseCor.tsx`, integrando `SeletorCanal`, `AmostrasCor`, "Confirmar cor", `ContadorTentativas`, `PainelDicas`, o botão pular (só quando `podePular`) e os anúncios, fazendo o T034 passar.
- [ ] T043 [P] [US1] Implementar `apps/reboot/src/interface/telas/ImagemPixels.tsx`, fazendo o T035 passar.
- [ ] T044 [P] [US1] Implementar a tela genérica `apps/reboot/src/interface/telas/ExplicacaoEtapa.tsx`, que mostra a explicação de qualquer etapa depois da prática (FR-012).
- [ ] T045 [US1] Ligar a etapa de cor em `apps/reboot/src/interface/App.tsx`, na ordem `cor-1` → `cor-2` → imagem de pixels → explicação, salvando o progresso a cada conclusão, pulo, erro ou derrota.

**Checkpoint**: a etapa de cor pode ser jogada e demonstrada sozinha. Este é o MVP.

---

## Phase 4: User Story 2 - Fazer o sinal passar pelos canos (Priority: P2)

**Goal**: as fases de prever e de escolher, com grade, bandeja, arrastar e alternativa por seleção, simulação do sinal, validação e revelação das portas.

**Independent Test**: jogar só a etapa de canos e prever corretamente a saída de uma combinação que não apareceu nas fases. Automaticamente, os testes de `tests/dominio/canos`, `tests/conteudo/canos.test.ts` e `tests/interface/fase-canos.test.tsx` passam, renderizando a etapa diretamente.

### Testes da User Story 2 (escrever primeiro e ver falhar) ⚠️

- [ ] T046 [P] [US2] Escrever `apps/reboot/tests/dominio/canos/portas.test.ts`, com as tabelas-verdade completas de AND e OR de 2 entradas e de NOT de 1 entrada (FR-008).
- [ ] T047 [P] [US2] Escrever `apps/reboot/tests/dominio/canos/grade.test.ts`:
  - colocar uma peça da bandeja na grade, mover a peça dentro da grade e devolvê-la à bandeja;
  - soltar fora da grade, numa célula de peça fixa ou numa célula já ocupada devolve a peça à bandeja, sem alterar o restante da montagem (Edge Case "posição inválida").
- [ ] T048 [P] [US2] Escrever `apps/reboot/tests/dominio/canos/simulacao.test.ts`, cobrindo a R5:
  - o sinal sai da fonte, passa por fios retos e curvos e por junções e chega à saída, calculado de trás para frente a partir da saída;
  - um caminho aberto resulta em `'desconectado'`;
  - um ciclo resulta em `'desconectado'`, sem erro;
  - para a mesma montagem e a mesma combinação, o resultado é sempre o mesmo.
- [ ] T049 [P] [US2] Escrever `apps/reboot/tests/dominio/canos/validacao.test.ts`, cobrindo o FR-009, o FR-010 e o SC-006:
  - **Confirmação incompleta:** acontece com alguma combinação `'desconectado'` ou com previsão em branco.
  - **Fase de escolher:** o resultado traz a lista por combinação (`entradas`, `esperada`, `obtida`, `passou`), e um caminho ligado com a junção errada é uma confirmação errada.
  - **Fase de prever:** o resultado traz só a quantidade de previsões erradas, sem dizer quais, e é certo só com o circuito correto e todas as previsões iguais à saída obtida.
  - **Solução alternativa:** uma montagem diferente da de referência, mas correta, é aceita.
- [ ] T050 [P] [US2] Escrever `apps/reboot/tests/conteudo/canos.test.ts` e o arquivo de soluções `apps/reboot/tests/conteudo/solucoes-canos.ts`, conforme o [contrato de conteúdo](contracts/conteudo-fase.md):
  - **Tipos e peças:** `canos-1` é de prever e `canos-2` é de escolher; as peças fixas ficam dentro das dimensões e não se sobrepõem; a fase de prever tem exatamente uma junção na bandeja, e a de escolher tem junções de mais de um tipo.
  - **Combinações:** `combinacoes` cobre todas as combinações possíveis das entradas, sem repetição.
  - **Conteúdo:** cada fase tem 3 dicas.
  - **Soluções de referência e alternativa:** são aceitas.
  - **Soluções incorretas:** são rejeitadas, entre elas a junção errada, o caminho aberto (como confirmação incompleta) e as previsões erradas sobre um circuito correto.
- [ ] T051 [P] [US2] Escrever `apps/reboot/tests/interface/fase-canos.test.tsx`:
  - **Seleção por teclado:** pela alternativa por seleção, só com teclado, o jogador seleciona uma peça da bandeja, a coloca numa célula, a move, a devolve à bandeja e cancela a seleção com Esc. Cada ação é anunciada ao leitor de tela (FR-007).
  - **Peça solta em posição inválida:** volta à bandeja.
  - **Estado do sinal:** é indicado por forma ou texto, além da cor (FR-018).
  - **Fase de prever:** com previsões erradas, mostra só a quantidade e o sinal não corre (US2 cenário 5); com tudo certo, o sinal corre.
  - **Fase de escolher:** mostra quais combinações passaram e quais falharam (US2 cenário 2).
  - **Confirmação incompleta:** avisa o que falta, sem gastar tentativa.
  - **Limite em `canos-2`:** aparece o aviso e as tentativas restantes.
  - **Acessibilidade:** não há violações do axe.

### Implementação da User Story 2

- [ ] T052 [P] [US2] Implementar `apps/reboot/src/dominio/canos/portas.ts`, fazendo o T046 passar.
- [ ] T053 [P] [US2] Implementar `apps/reboot/src/dominio/canos/grade.ts` (colocar, mover e devolver peças), fazendo o T047 passar.
- [ ] T054 [US2] Implementar `apps/reboot/src/dominio/canos/simulacao.ts`, fazendo o T048 passar. Depende do T052 e do T053.
- [ ] T055 [US2] Implementar `apps/reboot/src/dominio/canos/validacao.ts`, fazendo o T049 passar. Depende do T054.
- [ ] T056 [US2] Rascunhar o conteúdo da etapa de canos em `apps/reboot/src/conteudo/canos.ts` (a IA rascunha):
  - `canos-1` (prever) e `canos-2` (escolher), cada uma com grade, peças fixas, bandeja, entradas, combinações, 3 dicas, `devolveAoMundo` e `falaGuia`;
  - a explicação da etapa e o texto de revelação das portas AND, OR e NOT (US2 cenário 4).
- [ ] T057 [US2] **[Autores]** Revisar e aprovar `apps/reboot/src/conteudo/canos.ts` e as soluções de `apps/reboot/tests/conteudo/solucoes-canos.ts`. O T050 precisa passar.
- [ ] T058 [P] [US2] Implementar `apps/reboot/src/interface/componentes/PecaVisual.tsx`, em SVG feito para o projeto, com estados "com energia", "sem energia" e "desconectado" indicados por forma e por texto acessível, além da cor (FR-007, FR-018).
- [ ] T059 [P] [US2] Implementar `apps/reboot/src/interface/acessibilidade/navegacaoGrade.ts`: setas movem o foco entre as células, Enter ou Espaço seleciona e solta, e Esc cancela (R4).
- [ ] T060 [P] [US2] Implementar `apps/reboot/src/interface/componentes/useArrastar.ts`, com o arrastar por Pointer Events para mouse, toque e caneta, que dispara os mesmos comandos da seleção (R4).
- [ ] T061 [US2] Implementar `apps/reboot/src/interface/componentes/Grade.tsx` e `apps/reboot/src/interface/componentes/Bandeja.tsx`, com a alternativa por seleção, o arrastar e os anúncios de cada ação. Depende do T058, T059 e T060.
- [ ] T062 [P] [US2] Implementar `apps/reboot/src/interface/componentes/PainelPrevisoes.tsx`, para marcar 0 ou 1 em cada combinação, com rótulos acessíveis.
- [ ] T063 [P] [US2] Implementar `apps/reboot/src/interface/componentes/ResultadoCombinacoes.tsx`, com o resultado por combinação (fase de escolher) ou só a quantidade de erros (fase de prever), em texto.
- [ ] T064 [US2] Implementar `apps/reboot/src/interface/telas/FaseCanos.tsx`, integrando `Grade`, `Bandeja`, `PainelPrevisoes`, `ResultadoCombinacoes`, `ContadorTentativas`, `PainelDicas` e o botão pular (só quando `podePular`), fazendo o T051 passar.
- [ ] T065 [P] [US2] Implementar `apps/reboot/src/interface/telas/RevelacaoPortas.tsx`, que associa cada junção usada à sua porta lógica e à sua regra (US2 cenário 4).
- [ ] T066 [US2] Ligar a etapa de canos em `apps/reboot/src/interface/App.tsx`, na ordem `canos-1` → `canos-2` → revelação das portas → explicação da etapa (`ExplicacaoEtapa`), salvando o progresso.

**Checkpoint**: as etapas de cor e de canos funcionam e são testáveis de forma independente.

---

## Phase 5: User Story 3 - Abrir e testar em poucos minutos (Priority: P3)

**Goal**: a experiência completa, com abertura, premissa, guia, mundo que se religa, fatos com fonte, som, encerramento com quiz e retomada do progresso.

**Independent Test**: enviar o link a alguém que nunca viu o jogo e observar se a pessoa conclui a experiência sem ajuda. Automaticamente, `tests/interface/jornada-completa.test.tsx` percorre o jogo inteiro.

### Testes da User Story 3 (escrever primeiro e ver falhar) ⚠️

- [ ] T067 [P] [US3] Escrever `apps/reboot/tests/conteudo/narrativa.test.ts`:
  - a premissa, o resumo do encerramento e as 2 perguntas do quiz existem;
  - toda fase tem `falaGuia` com início e conclusão;
  - todo fato histórico tem `fonte` com título, autoria, URL e data de acesso preenchidos (FR-014).
- [ ] T068 [P] [US3] Escrever `apps/reboot/tests/interface/abertura.test.tsx`:
  - a abertura tem o botão "Começar" e a premissa pode ser pulada (US3 cenário 2);
  - nenhuma fala avança sozinha por tempo;
  - não há campo que peça dado pessoal (FR-015);
  - não há violações do axe.
- [ ] T069 [P] [US3] Escrever `apps/reboot/tests/interface/encerramento.test.tsx`:
  - o encerramento mostra o resumo dos conceitos, o aviso de protótipo e a versão (US3 cenário 3, FR-017);
  - o quiz mostra se cada resposta está certa;
  - nenhuma resposta do quiz é gravada no armazenamento (FR-013);
  - não há violações do axe.
- [ ] T070 [P] [US3] Escrever `apps/reboot/tests/interface/som.test.tsx`, com a API de áudio simulada:
  - nenhum som toca antes de "Começar";
  - o controle de silenciar funciona em todas as telas e fica salvo em `somMudo` (FR-020).
- [ ] T071 [P] [US3] Escrever `apps/reboot/tests/interface/jornada-completa.test.tsx`, com armazenamento em memória:
  - o teste percorre abertura → cor → canos → encerramento;
  - cada fase concluída devolve algo visível ao mundo (FR-002);
  - ao "recarregar" (remontar o App), o jogo retoma na primeira fase não concluída;
  - "Apagar progresso" volta ao início.

### Implementação da User Story 3

- [ ] T072 [US3] Rascunhar o conteúdo narrativo em `apps/reboot/src/conteudo/narrativa.ts` e os fatos históricos em `apps/reboot/src/conteudo/fatos.ts` (a IA rascunha). O arquivo de fatos pode ficar vazio, porque o protótipo admite nenhum fato.
  - **Narrativa:** a premissa, o nome e as falas do personagem guia original (FR-016), o resumo do encerramento e o quiz com o significado de (255, 0, 0) e a saída de uma porta lógica numa combinação nova (FR-013).
  - **Fatos históricos:** cada fato traz uma fonte para verificação.
- [ ] T073 [US3] **[Autores]** Revisar e aprovar `narrativa.ts` e `fatos.ts`, verificando na fonte cada fato histórico (FR-014, Princípio VII). O T067 precisa passar.
- [ ] T074 [P] [US3] Implementar `apps/reboot/src/interface/componentes/Guia.tsx`, com as falas curtas, que podem ser puladas e só avançam por ação do jogador (FR-002).
- [ ] T075 [P] [US3] Implementar `apps/reboot/src/interface/componentes/Mundo.tsx`, a representação provisória (CSS/SVG) do mundo que se religa a cada fase concluída (FR-002, FR-022).
- [ ] T076 [P] [US3] Implementar `apps/reboot/src/interface/componentes/FatoHistorico.tsx`, que exibe o fato com a fonte consultável (US3 cenário 4).
- [ ] T077 [P] [US3] Implementar `apps/reboot/src/interface/telas/Abertura.tsx`, fazendo o T068 passar.
- [ ] T078 [P] [US3] Implementar `apps/reboot/src/interface/componentes/Quiz.tsx`, que mostra se cada resposta está certa e mantém as respostas só em memória.
- [ ] T079 [US3] Implementar `apps/reboot/src/interface/telas/Encerramento.tsx`, com o resumo, o aviso de protótipo, `Versao` e `Quiz`, fazendo o T069 passar.
- [ ] T080 [P] [US3] Implementar `apps/reboot/src/audio/sintetizador.ts`, com os efeitos (acerto, erro, derrota, peça colocada) e a música provisória sintetizados pela Web Audio API, sem arquivos de áudio (R7, FR-022).
- [ ] T081 [US3] Implementar `apps/reboot/src/interface/estado/useSom.ts` e acrescentar o controle de silenciar em `apps/reboot/src/interface/componentes/Configuracoes.tsx`, com `somMudo` salvo e o áudio liberado só depois de "Começar", fazendo o T070 passar.
- [ ] T082 [US3] Ligar a experiência completa em `apps/reboot/src/interface/App.tsx`, fazendo o T071 passar:
  - a ordem abertura → etapas, com `Guia` e `Mundo`, → encerramento;
  - os sons dos eventos;
  - a retomada na primeira fase não concluída.

**Checkpoint**: a experiência completa funciona do link ao quiz.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: documentação, registros acadêmicos, validação final e entrega.

- [ ] T083 [P] Atualizar `ATTRIBUTIONS.md` (raiz) registrando que o protótipo não usa assets de terceiros: a arte em CSS/SVG e o áudio sintetizado foram feitos para o projeto, e as fontes são as do sistema (Princípio VII, FR-022).
- [ ] T084 [P] Registrar em `docs/tcc/uso-de-ia.md` o código e o conteúdo rascunhados pela IA nesta feature e o que os autores decidiram ou aprovaram (FR-022; conformidade CHK011).
- [ ] T085 [P] Atualizar `README.md` (raiz) com como rodar o protótipo e o link `https://rencaldas.github.io/reboot-tcc/`.
- [ ] T086 Rodar a validação automática do [quickstart.md](quickstart.md) (`npm ci`, `npm test`, `npm run lint`, `npm run typecheck`, `npm run build`, `npm run preview`) e corrigir o que falhar.
- [ ] T087 **[Autores]** Abrir a PR da feature para a `main` com o template do projeto. Depois da primeira execução do `ci.yml` nela, configurar no GitHub:
  - em *Settings → Pages*, a fonte "GitHub Actions";
  - em *Settings → Branches*, a proteção da `main`, exigindo PR, o check do CI e sem bypass para administradores (Princípio II e X).
- [ ] T088 **[Autores]** Executar os roteiros manuais 1 a 8 do [quickstart.md](quickstart.md) no link publicado: teclado, leitor de tela, celular em retrato e em paisagem, e a aba Rede sem requisições a outros domínios.
- [ ] T089 Registrar a entrada da feature em `docs/tcc/diario-de-bordo.md`, com decisões, problemas, tempo gasto e métricas (Princípio IX).

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: sem dependências.
- **Foundational (Phase 2)**: depende do Setup e **bloqueia** todas as user stories.
- **US1 (Phase 3)**: depende só da Foundational. É o MVP.
- **US2 (Phase 4)**: depende só da Foundational. Reaproveita `ExplicacaoEtapa` (T044) da US1. Se a US2 for feita antes, esse componente sai junto com ela.
- **US3 (Phase 5)**: depende da Foundational e de ao menos uma etapa pronta. O T071 e o T082 exigem a US1 e a US2.
- **Polish (Phase 6)**: depende das user stories desejadas.

### Within Each User Story

- Testes primeiro, falhando antes da implementação (Princípio II).
- Domínio → conteúdo (rascunho da IA → aprovação dos autores) → componentes → tela → ligação no `App`.
- As tarefas **[Autores]** de aprovação de conteúdo (T039, T057, T073) bloqueiam o merge, mas não o desenvolvimento das telas.

### Parallel Opportunities

- **Setup:** T002, T003, T005, T006, T007, T008 e T009 em paralelo.
- **Foundational:** os tipos (T011, T012), os testes (T013, T015, T016, T017) e a base visual (T022, T023, T024) em paralelo.
- **Testes de cada story:** os marcados [P] em paralelo; depois, as implementações [P] de domínio e de componentes.

---

## Parallel Example: User Story 2

```bash
# Testes da US2 juntos (devem falhar):
Task: "Escrever tests/dominio/canos/portas.test.ts"
Task: "Escrever tests/dominio/canos/grade.test.ts"
Task: "Escrever tests/dominio/canos/simulacao.test.ts"
Task: "Escrever tests/dominio/canos/validacao.test.ts"
Task: "Escrever tests/conteudo/canos.test.ts"
Task: "Escrever tests/interface/fase-canos.test.tsx"

# Componentes independentes juntos:
Task: "Implementar PecaVisual.tsx"
Task: "Implementar navegacaoGrade.ts"
Task: "Implementar useArrastar.ts"
Task: "Implementar PainelPrevisoes.tsx"
Task: "Implementar ResultadoCombinacoes.tsx"
```

---

## Implementation Strategy

### MVP First (User Story 1)

1. Phase 1 (Setup) e Phase 2 (Foundational).
2. Phase 3 (US1): a etapa de cor.
3. **Parar e validar**: jogar a etapa de cor e rodar os testes.
4. Pode ser mostrada ao orientador como primeira demonstração.

### Incremental Delivery

1. Setup + Foundational → base pronta.
2. US1 → etapa de cor (MVP).
3. US2 → etapa de canos.
4. US3 → experiência completa, publicada.
5. Polish → registros, validação manual e PR.

### Etapas do `/speckit-implement` (regra do projeto)

O `/speckit-implement` roda em etapas, com validação ao fim de cada uma (`PROMPT_TCC.md` §7):

1. Setup + Foundational;
2. US1;
3. US2;
4. US3;
5. Polish.

Cada etapa começa com um plano curto de 3 passos aprovado pelos autores (Princípio III).

---

## Notes

- [P] = arquivos diferentes, sem dependência de tarefa incompleta.
- Commits pequenos e atômicos em Conventional Commits, na branch `feat/002-prototipo-reboot` (Princípio X).
- O `/speckit-implement` lê o estado dos checklists em `checklists/` como gate. Os itens são marcados só pelos autores.
- **Decisão pendente dos autores:** o comportamento do valor digitado inválido num canal (T032, puzzles CHK016).
