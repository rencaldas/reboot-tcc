# Research: Protótipo jogável do Reboot

**Feature**: `002-prototipo-reboot` | **Data**: 2026-11-19 | **Plano**: [plan.md](plan.md)

Cada decisão segue o formato Decisão / Motivo / Alternativas. As decisões de stack foram tomadas pelo
autor em 2026-11-19 e estão registradas no [ADR 0001](../../docs/adr/0001-stack-do-prototipo.md).

## R1. Linguagem e interface

- **Decisão**: TypeScript em modo estrito, com React para a interface e Vite como ferramenta de build.
- **Motivo**: a interface é feita de elementos do DOM (botões, campos numéricos, grade), então teclado,
  foco, leitor de tela e zoom vêm do navegador, como exige o FR-018. Os autores já conhecem TS e React,
  o que pesa na defesa. Os puzzles são de lógica e não precisam de física nem de canvas.
- **Alternativas**: Phaser (canvas), rejeitado porque toda a acessibilidade teria de ser construída à
  mão; TypeScript sem framework, rejeitado por exigir mais código manual de atualização de tela.

## R2. Separação entre domínio e interface

- **Decisão**: as regras dos puzzles, a jornada e o progresso ficam em `src/dominio/`, em TypeScript
  puro, sem importar React nem APIs do navegador. A interface só chama funções do domínio.
- **Motivo**: o Princípio II exige testes antes da implementação no domínio crítico. Funções puras são
  testáveis de forma rápida e determinística (Princípio VIII) e podem ser explicadas isoladamente na
  monografia.
- **Alternativas**: regras dentro dos componentes React, rejeitado porque misturaria regra e tela e
  obrigaria a testar regra pela interface.

## R3. Testes

- **Decisão**: Vitest para todos os testes. Domínio em ambiente Node. Componentes com Testing Library
  em ambiente jsdom. Verificação automática de acessibilidade com `axe-core` nos testes de componente.
- **Motivo**: o Vitest usa a mesma configuração do Vite, sem uma segunda cadeia de build. O `axe-core`
  detecta parte das falhas de WCAG (rótulos, papéis, contraste declarado) de forma automática.
- **Alternativas**: Jest, rejeitado por exigir configuração própria de TypeScript e ESM; testes
  ponta a ponta com Playwright, adiados porque o SC-007 (computador e celular) é verificado
  manualmente no protótipo, conforme o [quickstart](quickstart.md).
- **Limite conhecido**: o `axe-core` não substitui o teste manual com leitor de tela, que fica no
  quickstart.

## R4. Arrastar peças e alternativa por seleção (FR-007)

- **Decisão**: o arrastar usa Pointer Events nativos (mouse, toque e caneta num único modelo). A
  alternativa por seleção é o caminho principal de acessibilidade: o jogador seleciona uma peça e
  depois a célula de destino, por clique, toque ou teclado (Tab, setas, Enter/Espaço, Esc). Cada ação
  é anunciada numa região `aria-live`.
- **Motivo**: a seleção já é obrigatória pela spec e resolve teclado e leitor de tela. Com ela pronta,
  o arrastar vira só uma forma mais rápida de chegar ao mesmo comando, e Pointer Events bastam para
  isso sem uma dependência a mais.
- **Alternativas**: biblioteca de drag-and-drop (como dnd-kit), rejeitada para o protótipo porque
  duplicaria o modelo de seleção e acrescentaria uma dependência; HTML5 Drag and Drop, rejeitado porque
  não funciona com toque.

## R5. Simulação do sinal nos canos

- **Decisão**: a grade é um grafo de células com portas nos quatro lados (norte, leste, sul, oeste). O
  valor da saída é calculado de trás para frente, a partir da célula de saída, seguindo as conexões até
  as fontes. Junções calculam AND, OR ou NOT sobre as entradas. Ciclo ou caminho aberto resulta em
  "desconectado", nunca em erro.
- **Motivo**: o cálculo é determinístico (Princípio VIII) e permite testar todas as combinações de
  entrada (FR-009). Como a validação compara o comportamento, qualquer arranjo que produza as saídas
  certas é aceito.
- **Alternativas**: simulação passo a passo no tempo, rejeitada por ser mais complexa e porque a
  animação do sinal é só visual e pode ser derivada do resultado.

## R6. Persistência do progresso (FR-021)

- **Decisão**: `localStorage`, numa única chave, com um objeto JSON que tem versão de esquema. A
  leitura valida o formato. Se estiver ausente, corrompido ou com versão desconhecida, o jogo recomeça
  do início sem erro. Formato no [contrato do progresso](contracts/progresso-salvo.md).
- **Motivo**: o Princípio V permite rodar só no cliente quando não há contas nem dados compartilhados.
  Nada que identifique o jogador é salvo.
- **Alternativas**: IndexedDB, rejeitado por ser complexo demais para poucos bytes; cookies, rejeitados
  porque são enviados ao servidor sem necessidade.

## R7. Áudio provisório (FR-020, FR-022)

- **Decisão**: efeitos e música provisórios sintetizados com a Web Audio API (osciladores), sem
  arquivos de áudio. O controle de silenciar fica salvo junto com o progresso.
- **Motivo**: o som é feito para o projeto, então não há licença de terceiros a registrar. O navegador
  só libera o áudio depois de uma interação do jogador, e o jogo começa com um clique em "Começar".
- **Alternativas**: arquivos de áudio livres, adiados para a feature de arte e áudio finais, quando a
  licença de cada um for verificada e registrada em `ATTRIBUTIONS.md`.

## R8. Arte provisória e fontes

- **Decisão**: formas geométricas simples em CSS e SVG feitas para o projeto, e pilha de fontes do
  sistema operacional. Nenhuma fonte ou recurso carregado de CDN.
- **Motivo**: evita licenças de terceiros no protótipo (Princípio VII) e evita que o navegador do
  jogador contate servidores externos, o que exporia o IP sem necessidade (Princípio VI).
- **Alternativas**: fontes web hospedadas por terceiros, rejeitadas pelo motivo acima.

## R9. Versão visível (FR-017)

- **Decisão**: a versão do protótipo é a versão do `package.json` (começando em `0.1.0`), injetada no
  build e exibida no rodapé e no encerramento. Todo conteúdo das fases fica em arquivos versionados no
  repositório, nunca é gerado em tempo de execução.
- **Motivo**: a mesma versão produz as mesmas fases e a mesma validação (Princípio VIII).
- **Alternativas**: hash do commit, rejeitado como versão principal porque não é legível para o
  testador.

## R10. Hospedagem e CI (FR-001, Princípio II)

- **Decisão**: build estático publicado no GitHub Pages pelo GitHub Actions, no endereço
  `https://rencaldas.github.io/reboot-tcc/` (o `base` do Vite é `/reboot-tcc/`). Um workflow de CI
  roda lint, checagem de tipos, testes e build em toda PR. Outro workflow publica a cada merge na
  `main`.
- **Motivo**: decisão do autor na clarificação de 2026-11-17. Não há backend nem custo. Para o CI
  bloquear merge com teste falhando, como pede o Princípio II, a proteção da `main` precisa exigir o
  check do CI, configuração feita pelo autor no GitHub.
- **Alternativas**: Netlify ou Vercel, não avaliados porque o autor já decidiu pelo GitHub Pages e
  disse que decide uma mudança se surgir algum problema.

## R11. Ambiente e reprodutibilidade do build

- **Decisão**: Node 26 (a versão instalada na máquina do autor), fixado em `.nvmrc` e no campo
  `engines`. Dependências fixadas pelo `package-lock.json` e instaladas com `npm ci` no CI. Versões das
  bibliotecas: as estáveis no momento do setup, registradas pelo lockfile.
- **Motivo**: o Princípio VIII pede build reprodutível com ambiente documentado.
- **Alternativas**: pnpm ou yarn, rejeitados porque o npm já vem com o Node e basta para um único app.

## R12. Qualidade de código

- **Decisão**: ESLint (regras de TypeScript e de hooks do React) e Prettier, rodando no CI.
- **Motivo**: apoia o Princípio I, com código limpo e sem código morto.
- **Alternativas**: sem linter, rejeitado.

## Pendências de conteúdo (não bloqueiam o plano)

Conforme a clarificação de 2026-11-19, a IA rascunha o conteúdo num arquivo separado do código e os
autores aprovam antes de ele entrar no jogo. Itens a rascunhar e aprovar:

1. Nome, aparência provisória e falas do personagem guia.
2. Cores-alvo das 2 fases de cor e a imagem de pixels do FR-006.
3. Grade, bandeja, entradas e saídas esperadas das 2 fases de canos (prever com uma porta fixa; escolher
   entre AND, OR e NOT).
4. Três dicas por fase, explicações de cada etapa, textos da premissa e do encerramento.
5. As 2 perguntas do quiz (FR-013), com a combinação nova de porta lógica.
6. Fatos históricos, se houver, cada um com fonte verificada pelos autores (FR-014).
