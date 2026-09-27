# PROMPT — TCC: TCG de Programação (nome provisório: `tcg-dev`)

> **Como usar:** salve este arquivo na raiz do repositório como `PROMPT_TCC.md`, abra o Claude Code na pasta e diga:
> "Leia o PROMPT_TCC.md e execute somente a Fase 0."

---

## 1. Papel e contexto

Você é o engenheiro de software sênior e coorientador técnico do meu TCC. Sou Renato de Azevedo Caldas, graduando em Ciência da Computação na Universidade Veiga de Almeida (UVA), com previsão de conclusão em 2027.

O TCC tem dois produtos inseparáveis:

1. **Software:** um jogo de cartas colecionáveis (TCG) web + app com tema de programação. O catálogo, com milhares de cartas, é gerado automaticamente a partir de dados públicos.
2. **Trabalho acadêmico:** monografia (ou artigo, conforme a UVA) em ABNT, compilada em PDF, mais todo o material de defesa.

Todo o planejamento segue **Spec-Driven Development (SDD)** com o GitHub Spec Kit: https://github.com/github/spec-kit

---

## 2. Regras de trabalho (inegociáveis — copie para o `CLAUDE.md` na Fase 0)

1. **Plano antes de código.** Nunca crie ou edite código sem antes apresentar um plano curto de 3 passos e esperar meu ok.
2. **Bugs.** Faça o diagnóstico e aponte a causa raiz antes de propor a correção. Use o fluxo de bug do Spec Kit: `/speckit-bug-assess` → `/speckit-bug-fix` → `/speckit-bug-test`.
3. **Testes.** Nunca remova testes existentes. O domínio crítico (pipeline, raridade, RNG de booster e economia) tem testes escritos antes da implementação.
4. **Código.** Funções pequenas e modulares, com responsabilidade única. Nada de código comentado ou morto.
5. **Ambiguidade.** Diga "não sei" ou pergunte. Nunca chute uma implementação, um requisito, uma regra de norma ou uma referência bibliográfica.
6. **Ritmo.** Uma fase por vez. Ao fim de cada fase, pare, resuma o que foi feito e aguarde minha revisão.
7. **Git.**
   - Nunca faça commit ou push direto na `main`.
   - Use branches `feat/`, `fix/`, `chore/` ou `docs/`. Se o Spec Kit criar branches numeradas para as features SDD, mantenha o padrão dele.
   - Commits pequenos e atômicos em Conventional Commits: `feat`, `fix`, `refactor`, `style`, `docs`, `test`, `chore`, `perf`, `build`, `ci`.
   - O título da PR segue o prefixo do commit principal.
   - Se houver dúvida entre `feat`, `refactor` e `fix`, pergunte.
   - Toda PR usa exatamente este template:

```markdown
## Pull Request: [Tipo]: [Descrição do que foi feito]

### 💡 Contexto e Motivação
[Por que esta PR existe: problema resolvido, história de usuário, feature SDD.]

### 📋 Mudanças e Impacto
[Mudanças de alto nível.]

### 🧪 Instruções de Teste (Como o revisor pode testar)
[Passos exatos para testar ou verificar.]

### ✅ Checklist de Qualidade
* [ ] Testes automatizados (Unitários/Integração) passaram.
* [ ] O código foi revisado pelo autor (Self-Review).
* [ ] Não há código comentado ou arquivos desnecessários.
* [ ] As instruções de teste (se aplicável) foram fornecidas na descrição.
```

---

## 3. A ideia original (texto meu, preservado)

> "É possível fazer um game web e app de um jogo de TCG, voltado para programação? O usuario recebe cartinhas através de Boosters, e terá radidades de cartas de R até UR, LR será uma raridade mais rara ainda. Ajustar raridade por importancia de carta, exemplo: Python, C, Java (UR) algo assim. R (funções de cada linguagem, tipo printf(), scan, etc) Terá cartas de tudo que envolve programação, tipo Erro 404, Erro 403, Post, Get. etc. Entendeu? Como podemos fazer? Existe uma api com todos esses dados, para criar automaticamente cada carta? seria muito demorado criar uma carta um por um. Me mostre um metodo que consuma todos os dados pra criar milhares de cartas pro TCG."

---

## 4. O que já foi decidido na fase exploratória

### 4.1 Geração do catálogo (existe uma prova de conceito)

Existe uma PoC em Python, o `build_cards.py`. Coloque-a em `reference/poc/build_cards.py`. Ela **não é código de produção**: use como referência de comportamento e reescreva com testes na feature `catalogo-cartas`.

**Fontes.** Não existe uma API única com todos os dados. As fontes são:

- **GitHub Linguist (`languages.yml`):** linguagens do tipo `programming`, com cor e extensões.
- **stdlib `http` do Python (`HTTPStatus` e `HTTPMethod`):** status codes e métodos HTTP. Requer Python 3.11+.
- **DevDocs (`https://documents.devdocs.io/{slug}/index.json`):** funções e keywords por linguagem. O DevDocs **não tem API oficial**, então o cache local é obrigatório e as requisições não podem martelar o servidor.
- **Stack Exchange API (`/2.3/tags/{tags}/info`):** lotes de 100 tags. O limite é de ~300 requisições por dia sem key e 10 mil por dia com key gratuita.

**Regras de raridade e atributos:**

- A popularidade é a contagem de perguntas da tag no Stack Overflow. Ela é a régua única que permite comparar tipos diferentes.
- A raridade é o percentil da carta dentro do seu tipo, limitado a uma faixa fixa por tipo:

  | Tipo de carta | Raridades permitidas |
  |---|---|
  | Linguagem | SR, SSR, UR |
  | Status HTTP | R, SR, SSR |
  | Método HTTP | R, SR, SSR |
  | Função | R, SR |

- LR é curadoria manual, feita por uma lista de overrides, e nunca sai da estatística.
- Os atributos (ataque e defesa) são determinísticos: saem do hash do id da carta, dentro da banda da raridade. Um rebuild nunca altera cartas existentes.

**Filtros:**

- Mínimo de perguntas por tipo, para cortar ruído.
- Aliases de nome para tag, para nomes que não batem com a tag do Stack Overflow.
- No DevDocs, só entram identificadores simples.

**Risco conhecido.** Tags genéricas como `map` e `print` medem o conceito, não a função. Por isso o topo de cada tipo precisa de revisão manual.

**Expansões candidatas:**

- Wikidata: ano de criação, paradigma e "influenciado por", útil para sinergias entre cartas.
- MDN browser-compat-data: Web APIs.
- Categorias desejadas que ainda não têm fonte definida: exceções e erros de runtime, estruturas de dados, padrões de projeto, ferramentas como Git e Docker. Levante as fontes na clarificação, sem presumir que existam.

### 4.2 Conteúdo e licenças

- Não copie texto de documentação para as descrições das cartas. MDN e cppreference são CC-BY-SA.
- As descrições são próprias: geradas em lote por LLM, com revisão humana e registro do processo.
- Sem logos oficiais, que são marcas registradas. A arte é procedural, em SVG, usando a cor do Linguist.
- Mantenha um `ATTRIBUTIONS.md` com fonte, termos de uso e licença de cada dataset. Verifique cada termo na própria fonte, sem presumir.

### 4.3 Boosters, economia e conformidade legal (Brasil)

**Base legal:**

- A Lei 15.211/2025 (ECA Digital) está em vigor desde 17/03/2026.
- O art. 20 veda caixas de recompensa em jogos direcionados a crianças e adolescentes ou de acesso provável por eles.
- A definição de caixa de recompensa (art. 2º, IV) exige aquisição mediante pagamento.

**Requisitos que decorrem disso:**

- Os boosters são 100% gratuitos e obtidos só por gameplay.
- Não existe nenhum caminho pago até o booster. A moeda de booster não pode ser comprada nem convertida a partir de moeda premium. Isso é garantido no ledger do backend, não só na UI.
- A abertura de booster acontece no servidor, com CSPRNG. As drop rates ficam numa tabela versionada e são exibidas ao jogador.
- Se houver troca de cartas ou chat entre jogadores, o art. 21 exige salvaguardas.
- LGPD: minimização de dados.

**Fora do MVP (zonas cinzentas):** booster liberado por anúncio e troca ou venda de cartas por dinheiro real.

**Monetização futura, se houver:** só itens de conteúdo conhecido, como cosméticos e passe com recompensas visíveis.

**Status jurídico.** O tema ainda está em disputa (há uma ADI no STF). Isto não é parecer jurídico: registre como risco no projeto e valide com o orientador.

### 4.4 Premissas NÃO confirmadas (levar ao `/speckit-clarify`)

- A escala de raridade é R → SR → SSR → UR → LR?
- Qual é a stack técnica? (ver 7.3)
- Como é uma partida? Existe batalha? Deckbuilding? PvP ou PvE?
- O jogo **ensina** programação ou só usa o tema?

---

## 5. Fase 0 — Requisitos acadêmicos e setup (NÃO escreva código nesta fase)

### 5.1 Perguntas para mim

Faça todas de uma vez, numa lista numerada, e registre as respostas em `docs/tcc/requisitos-academicos.md`:

1. Qual formato de TCC a UVA exige: monografia ou artigo científico (NBR 6022)? Existe pré-projeto (NBR 15287)?
2. A UVA tem manual de normalização ou template próprio? Peça o PDF. Ele prevalece sobre a ABNT quando houver conflito.
3. Quem são o orientador e o coorientador? Quais as datas de entrega (pré-projeto, qualificação, versão final, defesa)? Quem compõe a banca?
4. Há limite de páginas? Qual o idioma? A UVA exige artigo ou pôster? O trabalho vai para depósito no repositório institucional?
5. Qual a política da UVA e do orientador sobre uso de IA generativa no TCC?
6. **Qual é a pergunta de pesquisa?** Um jogo sozinho é produto, não pesquisa. Proponha 2 ou 3 enquadramentos para eu decidir com o orientador, por exemplo:
   - (a) geração automática de catálogo de TCG a partir de dados abertos e métricas de popularidade: contribuição técnica, com avaliação quantitativa do catálogo;
   - (b) gamificação no ensino de programação: exige avaliação de aprendizagem com pessoas;
   - (c) SDD com agentes de IA como processo de engenharia: estudo de caso do próprio desenvolvimento.
7. Tenho acesso às normas ABNT vigentes? A biblioteca pode ter a coleção. Peça os PDFs em `docs/tcc/normas/` e deixe essa pasta no `.gitignore`, porque as normas são protegidas por direito autoral.

### 5.2 Setup

Eu rodo os comandos no terminal. Você me passa os comandos e confere o resultado:

```bash
uv tool install specify-cli
specify init tcg-dev --integration claude   # skills instaladas em .claude/skills
cd tcg-dev
specify extension add bug      # fluxo de bug: assess → fix → test
specify extension add assess   # opcional: avaliação da ideia (insumo pra Justificativa)
```

Observações:

- Se o diretório já existir, consulte o guia de projetos existentes do Spec Kit antes de rodar `init`.
- Confira `specify version` e a documentação atual: o Spec Kit muda rápido.
- Invoque os comandos na forma que o Claude Code expuser (`/speckit-<comando>`).

### 5.3 `CLAUDE.md` e estrutura do repositório

Crie o `CLAUDE.md` com as regras da seção 2 e proponha a estrutura abaixo. A estrutura de código final (`apps/*`) será definida no `/speckit-plan`.

```
tcg-dev/
├── .specify/              # Spec Kit (constituição, templates)
├── specs/                 # artefatos SDD por feature (gerados pelo Spec Kit)
├── pipeline/              # geração do catálogo
├── apps/                  # web, api, mobile (definido no plan)
├── reference/poc/         # build_cards.py (só referência)
├── docs/
│   ├── adr/               # decisões de arquitetura
│   ├── arquitetura/       # C4, ER, sequência (Mermaid)
│   ├── api/               # OpenAPI
│   ├── manual-usuario/
│   └── tcc/
│       ├── monografia/    # LaTeX (abnTeX2)
│       ├── referencias.bib
│       ├── defesa/
│       ├── normas/        # gitignored
│       ├── diario-de-bordo.md
│       ├── rastreabilidade.md
│       ├── regras-formatacao.md
│       └── uso-de-ia.md
├── ATTRIBUTIONS.md
├── CLAUDE.md
└── README.md
```

### 5.4 Opcional: avaliação da ideia

Rode `/speckit-assess-intake` → `research` → `define` → `shape` → `decide` com `slug=tcg-programacao`. Os artefatos servem de insumo para a Justificativa da monografia.

**Pare aqui e aguarde minha revisão.**

---

## 6. Fase 1 — Constituição

Rode com este conteúdo, ajustado ao que vier da Fase 0:

```
/speckit-constitution Projeto de TCC: TCG web e mobile com tema de programação e catálogo de cartas gerado automaticamente a partir de dados públicos. Princípios:
I. Código limpo e modular: funções pequenas, responsabilidade única, sem código morto ou comentado.
II. Testes primeiro no domínio crítico (pipeline, raridade, RNG de booster, economia). Testes existentes nunca são removidos. CI bloqueia merge com teste falhando.
III. Plano antes de código: toda criação ou edição de código começa com um plano de 3 passos aprovado pelo autor. Bugs exigem diagnóstico e causa raiz antes da correção.
IV. Sem suposições: ambiguidade vira [NEEDS CLARIFICATION] ou pergunta ao autor, nunca chute.
V. Servidor autoritativo: abertura de booster, RNG (CSPRNG), inventário e economia só no backend. Drop rates versionadas e visíveis ao jogador.
VI. Conformidade por design: boosters exclusivamente gratuitos e sem nenhum caminho pago (ECA Digital, Lei 15.211/2025). Minimização de dados (LGPD). Salvaguardas para qualquer interação entre usuários.
VII. Dados e licenças: toda fonte externa usa cache, respeita rate limit e tem termos registrados em ATTRIBUTIONS.md. Nenhum texto de documentação copiado. Nenhuma marca ou logo de terceiros.
VIII. Reprodutibilidade: catálogo determinístico e versionado (mesmas entradas geram as mesmas cartas; cartas existentes nunca mudam de identidade). Ambiente via Docker. PDF da monografia gerado por build automatizado.
IX. Rastreabilidade acadêmica: decisões relevantes viram ADR. Cada feature registra decisões, problemas e métricas no diário de bordo. Nenhuma referência bibliográfica sem verificação.
X. Git: branches, Conventional Commits e PR com template. Nunca commit direto na main.
```

---

## 7. Fase 2 — Features via SDD

### Ciclo por feature (ordem oficial do Spec Kit)

`/speckit-specify` → `/speckit-clarify` (quantas rodadas forem necessárias) → `/speckit-plan` → `/speckit-checklist` → `/speckit-tasks` → `/speckit-analyze` → `/speckit-implement` (em etapas) → `/speckit-converge`.

Repita implement → converge até o resultado ser **Converged**.

### Regras do ciclo

- `/speckit-specify` descreve só o quê e o porquê. A stack vai no `/speckit-plan`.
- **Checklists customizados são meus.** Você avalia quando eu pedir, mas nunca marca `[x]` sozinho.
- `/speckit-analyze` é somente leitura. Cada problema encontrado é corrigido na fase de origem (specify/clarify, plan ou tasks) e reanalisado até sair limpo.
- `/speckit-implement` roda em etapas: primeiro Setup + Foundational, depois uma user story por vez, validando cada etapa antes de seguir.
- Ao fim de cada feature:
  - PR para a `main`.
  - Entrada no `diario-de-bordo.md` com decisões, problemas, tempo gasto e métricas. Esse material alimenta o capítulo de Metodologia.

### Backlog de features (ordem sugerida — confirme comigo)

1. `catalogo-cartas`: pipeline de geração do catálogo (seção 4.1).
2. `contas-e-colecao`: cadastro, login, inventário e visualização da coleção.
3. `boosters`: obtenção gratuita por gameplay, abertura server-side, drop rates visíveis, tratamento de duplicatas.
4. `arte-cartas`: renderização SVG procedural.
5. `partida`: regras do TCG, a definir no clarify. Pode ficar reduzida no MVP.
6. `app-mobile`: empacotamento e instalação (PWA e/ou loja).
7. `conteudo-educativo`: só se a pergunta de pesquisa exigir.

### 7.1 Argumento pronto para a primeira feature

```
/speckit-specify Pipeline de geração do catálogo de cartas de um TCG de programação. Como mantenedor do jogo, quero gerar automaticamente milhares de cartas a partir de fontes públicas, porque criar carta por carta é inviável. Cada carta representa um elemento do universo da programação: linguagens, status HTTP (ex.: 404, 403), métodos HTTP (ex.: GET, POST) e funções de linguagens (ex.: printf, scanf, print). A raridade reflete a importância do elemento, medida por um indicador de popularidade comparável entre tipos, e cada tipo tem uma faixa de raridades permitida: linguagens muito importantes (Python, C, Java) chegam a UR; funções ficam nas raridades baixas; LR é escolhida manualmente e é mais rara que UR. Cada carta tem atributos de jogo que dependem da raridade e não mudam entre execuções. Rodar o pipeline de novo com os mesmos dados produz exatamente o mesmo catálogo, e cartas já existentes nunca mudam de identidade nem são apagadas se já estiverem na coleção de algum jogador. O pipeline gera um relatório com a distribuição de cartas por tipo e raridade e lista as cartas que precisam de revisão manual (ex.: nomes genéricos com popularidade inflada). O catálogo não contém texto copiado das fontes nem logos de terceiros e registra a origem de cada carta. Uma fonte que falhar não derruba a execução inteira: o relatório indica o que ficou de fora.
```

### 7.2 O que o `/speckit-clarify` precisa cobrir nessa feature

Se ele não perguntar, force com um argumento de foco:

- Qual é a escala de raridade?
- O que acontece com uma carta que some da fonte? (Versionamento de catálogo.)
- Quais fontes usar para as categorias sem API?
- Há limite de cartas por tipo?
- Qual é a política de revisão manual?
- Qual é o volume-alvo de cartas no MVP?

### 7.3 Proposta de stack para o `/speckit-plan`

Isto é uma proposta, não uma decisão. Apresente alternativas, me pergunte e registre **um ADR por escolha**.

- **Pipeline:** Python 3.12, reaproveitando a PoC.
- **Backend:** Django + DRF + PostgreSQL, que eu já uso, **ou** Supabase (Postgres + Auth + Edge Functions). Critérios de escolha:
  - segurança do RNG server-side;
  - custo zero ou baixo para manter no ar até a banca;
  - facilidade de testar.
- **Frontend:** React + TypeScript como PWA. O app mobile sai via Capacitor ou Expo (decidir no ADR).
- **Infra:** Docker Compose local. CI no GitHub Actions com lint, testes e build do PDF da monografia.

---

## 8. Fase 3 — Documentação técnica (contínua, não só no fim)

- **README:** visão do projeto, como rodar e arquitetura resumida.
- **ADRs:** contexto, decisão, alternativas e consequências.
- **Diagramas em Mermaid:** C4 (contexto, contêiner, componente), modelo ER e sequência da abertura de booster.
- **API:** OpenAPI gerado a partir do backend.
- **Operação:** manual do usuário com capturas de tela e guia de deploy.
- **Arquivos de projeto:**
  - `ATTRIBUTIONS.md`;
  - `CHANGELOG.md`;
  - `LICENSE`: me pergunte qual licença usar, considerando as licenças das fontes.

---

## 9. Fase 4 — Monografia ABNT em PDF

### 9.1 Normas

Estas são as versões vigentes que conheço em set/2026. Confirme cada uma antes de usar. O manual da UVA prevalece.

- **Formato e estrutura:**
  - NBR 14724:2024 (trabalhos acadêmicos, apresentação; substituiu a de 2011);
  - NBR 6024 (numeração progressiva);
  - NBR 6027 (sumário);
  - NBR 6028 (resumo);
  - NBR 12225 (lombada, se impresso);
  - NBR 6034 (índice, se houver).
- **Referências e citações:**
  - NBR 6023:2025 (referências);
  - NBR 10520:2023 (citações).
- **Conforme o formato exigido:**
  - NBR 15287 (projeto de pesquisa);
  - NBR 6022 (se for artigo).
- **Tabelas:** Normas de Apresentação Tabular do IBGE.

**Não formate de memória.** Derive margens, fontes, espaçamentos e a ordem dos elementos do texto da norma ou do manual que eu fornecer. Registre tudo em `docs/tcc/regras-formatacao.md`, com a seção de origem de cada regra.

### 9.2 Ferramenta e build

- LaTeX com abnTeX2 + `abntex2cite` ou `biblatex-abnt`. Se a UVA exigir template Word, me avise e adaptamos.
- A classe e os estilos foram criados para versões anteriores das normas. Audite os pontos divergentes em relação à NBR 14724:2024 e à NBR 6023:2025, ajuste e documente cada ajuste.
- **Build reprodutível:**
  - `latexmk` rodando via Docker (imagem TeX Live), disparado por `make pdf`;
  - o CI gera o PDF como artefato em toda PR que tocar `docs/tcc/`.

### 9.3 Estrutura

Ajuste ao manual da UVA e ao formato escolhido.

**Pré-textuais:**

- capa e folha de rosto;
- ficha catalográfica (normalmente gerada pela biblioteca; deixe um placeholder);
- errata (se houver) e folha de aprovação;
- dedicatória, agradecimentos e epígrafe: opcionais, e escritos por mim;
- resumo + palavras-chave e abstract + keywords;
- listas de ilustrações, tabelas, abreviaturas e siglas;
- sumário.

**Textuais (sugestão):**

1. **Introdução:** contexto, problema, objetivos geral e específicos, justificativa e estrutura do trabalho.
2. **Fundamentação teórica:**
   - TCGs e sistemas de raridade;
   - gamificação e aprendizagem baseada em jogos;
   - engenharia de requisitos e SDD com agentes de IA;
   - integração de dados abertos (ETL, APIs, rate limit, licenças);
   - aspectos legais: ECA Digital, Marco Legal dos Jogos Eletrônicos (Lei 14.852/2024) e LGPD.
3. **Trabalhos relacionados:** jogos e plataformas de ensino de programação e TCGs educativos, com tabela comparativa.
4. **Metodologia:** pesquisa aplicada; processo SDD com Spec Kit (fases, artefatos, quality gates); critérios e instrumentos de avaliação.
5. **Desenvolvimento:**
   - requisitos (a partir dos `spec.md`);
   - arquitetura (a partir dos `plan.md` e ADRs);
   - pipeline do catálogo e modelagem de dados;
   - economia de boosters e conformidade legal;
   - implementação.
6. **Avaliação e resultados:**
   - testes automatizados e cobertura;
   - métricas do catálogo: cartas por tipo e raridade, tempo de geração, taxa de tags sem correspondência;
   - teste de usabilidade (ex.: SUS), se aprovado;
   - análise do processo SDD, com dados do diário de bordo.
7. **Conclusão:** contribuições, limitações e trabalhos futuros.

**Pós-textuais:** referências, glossário, apêndices (specs, questionários, TCLE) e anexos.

### 9.4 Rastreabilidade

Mantenha `docs/tcc/rastreabilidade.md` ligando cada seção da monografia aos artefatos de origem: spec, plan, ADR, relatório de testes e commit.

### 9.5 Integridade acadêmica (obrigatório)

**Referências e citações:**

- **Nunca invente** referência, autor, DOI, página ou citação.
- Toda entrada do `.bib` precisa de DOI ou URL conferido por você na sessão, com data de acesso.
- O que você não conseguir verificar entra marcado com `% VERIFICAR`, e eu confiro na fonte.
- Citações diretas só de trechos que eu forneci ou que você leu na fonte, sempre com página.

**Autoria:**

- **A autoria do texto é minha.**
- Você produz: estrutura, esqueletos de seção com tópicos, diagramas, tabelas, dados de resultados e revisão de formatação ABNT e de coerência.
- Rascunhos só quando eu pedir, marcados com `%% RASCUNHO-IA` para eu reescrever.

**Registro de uso de IA:** mantenha `docs/tcc/uso-de-ia.md` com o que a IA fez, em qual seção e como eu revisei. Esse registro atende à política da UVA e vira declaração na Metodologia.

### 9.6 Pesquisa com pessoas

Se houver teste de usabilidade ou questionário:

- só participantes adultos, anônimos e com TCLE;
- antes de coletar qualquer dado, pergunte ao orientador se é preciso submeter ao CEP. A Resolução CNS 510/2016 trata das hipóteses de dispensa.

---

## 10. Fase 5 — Defesa

- **Slides:** Beamer com a identidade da monografia ou PPTX, conforme a UVA, para 15 a 20 minutos.
- **Roteiro de fala.**
- **Demonstração:** demo ao vivo, com um vídeo gravado de backup.
- **Perguntas prováveis da banca**, com respostas apoiadas nos artefatos:
  - Por que o Stack Overflow como métrica? Que vieses isso traz?
  - Como garantir que o RNG não é manipulável?
  - O booster é loot box perante o ECA Digital?
  - Quais são as limitações do trabalho?

---

## 11. Entregáveis finais (checklist)

**Software:**

- [ ] Repositório com README, CI verde, testes e tag de release da versão defendida
- [ ] Catálogo gerado + relatório de distribuição
- [ ] Web publicada + app instalável
- [ ] Artefatos SDD de todas as features, convergidos

**Documentação técnica:**

- [ ] ADRs, diagramas, OpenAPI, manual do usuário e `ATTRIBUTIONS.md`

**Trabalho acadêmico:**

- [ ] Monografia em PDF conforme ABNT/UVA, com todas as referências verificadas
- [ ] Registro e declaração de uso de IA
- [ ] Slides, roteiro e vídeo da demo
- [ ] Versão final pronta para depósito

---

## 12. Primeira ação

Leia este arquivo inteiro e confirme em até 5 linhas o que entendeu. Depois execute **somente a Fase 0**: faça as perguntas da seção 5.1 e me passe os comandos de setup. Não crie código ainda.
