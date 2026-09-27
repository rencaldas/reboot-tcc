# Idea Research: TCG web/app de tema de programação com catálogo gerado automaticamente

- **Slug**: tcg-programacao
- **Created**: 2026-09-27
- **Evidence confidence (overall)**: medium — boa cobertura de trabalhos relacionados e confirmação legal; nenhuma evidência direta de demanda de usuário (nenhuma pesquisa/entrevista feita ainda) e a métrica de popularidade (Stack Overflow) tem limitações documentadas.

**Nota metodológica:** toda evidência abaixo foi obtida via busca na web (WebSearch) em 2026-09-27, não via leitura completa das páginas originais — cada achado carrega a citação da página encontrada. Isso é suficiente para decidir se a ideia vale a pena seguir (esta etapa de assessment), mas **não substitui a verificação direta na fonte exigida pelo Princípio IX / seção 9.5 do `PROMPT_TCC.md`** antes de qualquer citação entrar no `.bib` da monografia.

## Users & Demand

- Existem pelo menos quatro projetos independentes de "jogo de cartas para ensinar/tematizar programação" já construídos no Brasil (ver Prior Art) — isso é sinal de que a ideia geral tem apelo percebido por múltiplos criadores independentes, mas não é evidência direta de demanda de jogador para *este* projeto especificamente. [ASSUMPTION, inferido da existência de projetos similares] (confidence: low)
- Revisões sistemáticas e meta-análises recentes mostram efeito positivo mensurável de gamificação em educação de programação sobre motivação e, em parte dos estudos, sobre desempenho — uma meta-análise citada reporta taxa média de sucesso de 84,14% em plataformas gamificadas vs. não-gamificadas. [source: ScienceDirect meta-analysis; PMC systematic review] (confidence: medium — mas ver "Evidence Against" sobre a falta de unanimidade)

## Prior Art

- **TechDex TCG** — jogo de cartas onde o jogador coleciona tecnologias reais (Docker, React, Kubernetes, Rust, PostgreSQL, AWS…), cada carta com stats, raridade, classe e habilidades. É o precedente conceitualmente mais próximo encontrado: cartas = tecnologias reais, com raridade e atributos de jogo. Encontrado via post no TabNews (projeto pessoal/indie, não é produto comercial nem trabalho acadêmico publicado). — [source: tabnews.com.br/gustavow3b] (confidence: medium — descrição pública apenas, sem acesso ao código/dados do projeto para comparar a fundo)
- **AlgoCards** — jogo de cartas "desplugado" (sem computador) para ensinar conceitos de programação/pensamento computacional; cartas correspondem a comandos executados por bonecos ou pelos próprios jogadores. Material disponível gratuitamente em PDF. — [source: computacional.com.br, via tabnews.com.br/steniovm]
- **Cartas Programáveis** — jogo de cartas criado por Camila Achutti (CEO da Mastertech), adotado pela rede estadual de ensino de São Paulo para ensinar programação. — [source: educacao.sp.gov.br — fonte governamental oficial] (confidence: medium-high)
- **Logirunner** — jogo de cartas da UFSM para ensino de lógica e algoritmos. — [source: ufsm.br]
- Nenhum dos precedentes encontrados combina as três características centrais deste projeto: (1) geração automática do catálogo em escala a partir de fontes de dados públicas, (2) uma economia formal de raridade/booster com conformidade legal explícita, e (3) avaliação acadêmica do próprio pipeline de geração. Isso é o que diferenciaria a contribuição se o enquadramento de pesquisa (a) — geração automática de catálogo — for o escolhido.

## Market & Context

- TCGs comerciais consolidados (Magic: The Gathering, Pokémon TCG, Pokémon TCG Pocket) validam o formato "coleção via booster + níveis de raridade" como comprovadamente engajante em escala global — e é exatamente esse formato (mecânica de "caixa de recompensa") que motivou a regulação do ECA Digital que este projeto precisa respeitar. — [source: Wikipedia — Collectible card game, Pokémon TCG Pocket]
- Múltiplos esforços educacionais independentes com cartas de programação já rodando dentro da rede pública paulista sugerem apetite institucional brasileiro por esse formato especificamente em contexto escolar — relevante se o enquadramento (b) for escolhido.

## Data & Constraints

- **Confirmado:** a Lei 15.211/2025 (ECA Digital) está em vigor desde 17/03/2026 e proíbe caixas de recompensa pagas em jogos de acesso provável por crianças e adolescentes — corrobora a afirmação do `PROMPT_TCC.md` §4.3. — [source: conjur.com.br, radio Senado, planalto.gov.br (texto da lei)]
- **Confirmado (não estava verificado antes deste research):** existe de fato uma ADI em curso no STF contra o ECA Digital — **ADI 7999**, ajuizada pelo partido Missão, relatada pelo Ministro Luiz Fux, contestando tanto a proibição total de loot boxes (o partido defende que pais deveriam poder autorizar o uso) quanto as regras de verificação de idade por dados biométricos. Sem julgamento do mérito ou de medida cautelar até a data da busca. — [source: conjur.com.br (2026-08-05), gazetadopovo.com.br] (confidence: medium — fontes jornalísticas jurídicas, não o inteiro teor do processo; tentativa de acessar `portal.stf.jus.br` diretamente para confirmar na fonte primária retornou HTTP 403)
- **Risco de metodologia identificado:** a contagem de perguntas por tag no Stack Overflow é uma métrica de popularidade real e usada por terceiros (a própria ferramenta "Stack Overflow Trends", rankings da RedMonk), mas tem viés documentado: tecnologias mais novas acumulam menos perguntas por terem menos tempo de existência, e o volume de perguntas no Stack Overflow como um todo atingiu o pico por volta de 2016 e vem em declínio desde então — o que pode distorcer comparações entre tecnologias muito novas e muito antigas. — [source: stackoverflow.blog, redmonk.com, szabgab.com] (confidence: medium — este é um risco real que a seção de Limitações da monografia deveria endereçar explicitamente, e que a feature `catalogo-cartas` deveria considerar ao definir a janela de dados usada)

## Evidence Against the Idea

- A literatura de gamificação em educação de programação **não é unânime**: parte dos estudos encontrados aponta que gamificação melhora motivação/engajamento mas não necessariamente retenção/aprendizagem profunda, e que elementos de jogo podem até distrair do aprendizado se mal desenhados. Isso significa que o enquadramento (b) não pode assumir efeito positivo de aprendizagem por si só — exigiria instrumento de avaliação rigoroso (pré/pós-teste), não só satisfação. — [source: PMC systematic review — "mixed perspectives"]
- O nicho "jogo de cartas + programação" já tem pelo menos quatro tentativas independentes no Brasil (TechDex TCG, AlgoCards, Cartas Programáveis, Logirunner) — a existência do jogo em si não é mais o diferencial acadêmico; o diferencial precisa estar na geração automática do catálogo e sua avaliação (enquadramento a), não na ideia do jogo isoladamente.
- Não há nenhuma pesquisa de demanda direta (entrevista, survey, piloto) feita para este projeto especificamente — toda a evidência de "demanda" acima é inferida da existência de projetos parecidos, o que é evidência fraca de que jogadores vão de fato adotar e manter engajamento com este catálogo específico.
- A métrica de popularidade via Stack Overflow tem viés temporal documentado (ver Data & Constraints) — um risco real de que o catálogo super-representa tecnologias antigas e sub-representa as recentes, na ausência de correção.

## Gaps & Open Questions

- [NEEDS CLARIFICATION: decisão final entre os enquadramentos de pesquisa (a) e (b) — ver `intake.md`. As implicações de evidência são bem diferentes: (a) precisa de avaliação quantitativa do catálogo; (b) precisa de instrumento de avaliação de aprendizagem com pessoas, incluindo possível submissão ao CEP.]
- [NEEDS CLARIFICATION: não foi possível comparar em profundidade com o TechDex TCG por falta de acesso ao código/dados desse projeto — só a descrição pública encontrada via busca.]
- [NEEDS CLARIFICATION: confirmar o andamento da ADI 7999 no STF periodicamente até a defesa, já que pode mudar a base legal do Princípio VI da constituição.]

## Sources

- [Comparing tag trends with our Most Loved programming languages](https://stackoverflow.blog/2023/01/26/comparing-tag-trends-with-our-most-loved-programming-languages/) (host: stackoverflow.blog, policy: websearch)
- [Stack Overflow and the Programming Language Rankings](https://redmonk.com/rstephens/2025/06/18/stackoverflow/) (host: redmonk.com, policy: websearch)
- [Programming language popularity - Stack Overflow](https://szabgab.com/programming-language-popularity-stackoverflow) (host: szabgab.com, policy: websearch)
- [Adultização: Lei nº 15.211 proíbe caixa de recompensas em games](https://www.conjur.com.br/2025-out-15/adultizacao-lei-no-15-211-proibe-caixa-de-recompensas-em-games/) (host: conjur.com.br, policy: websearch)
- [L15211 - Planalto (texto da lei)](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2025/lei/l15211.htm) (host: planalto.gov.br, policy: websearch)
- [ECA digital proíbe rolagem infinita e caixa de recompensa em games infantojuvenis — Rádio Senado](https://www12.senado.leg.br/radio/1/noticia/2026/03/27/eca-digital-proibe-rolagem-infinita-e-caixa-de-recompensa-em-games-infantojuvenis) (host: senado.leg.br, policy: websearch)
- [Partido questiona regras sobre verificação de idade na internet (ADI 7999)](https://conjur.com.br/2026-ago-05/partido-questiona-regras-do-eca-digital-sobre-verificacao-de-idade-na-internet/) (host: conjur.com.br, policy: websearch)
- [Missão pede que STF autorize recompensas aleatórias a crianças e adolescentes em jogos](https://www.gazetadopovo.com.br/republica/missao-pede-que-stf-autorize-recompensas-aleatorias-a-criancas-e-adolescentes-em-jogos/) (host: gazetadopovo.com.br, policy: websearch)
- [Supremo Tribunal Federal — detalhe do processo](https://portal.stf.jus.br/processos/detalhe.asp?incidente=6822592) (host: portal.stf.jus.br, policy: fetch tentado e bloqueado — HTTP 403, não confirmado na fonte primária)
- [Pitch: Fiz um TCG que roda dentro do VS Code — TechDex TCG](https://www.tabnews.com.br/gustavow3b/fiz-um-tcg-jogo-de-cartas-que-roda-dentro-do-vs-code-colecionando-tecnologias-de-verdade-vim-contar-os-perrengues-e-chamar-voces-pra-testar) (host: tabnews.com.br, policy: websearch)
- [Jogo de Cartas para ensinar computação DESPLUGADA ganha versão PLUGADA (AlgoCards)](https://www.tabnews.com.br/steniovm/jogo-de-cartas-para-ensinar-computacao-desplugada-ganha-versao-plugada) (host: tabnews.com.br, policy: websearch)
- [Jogo de cartas ensina programação para estudantes da rede estadual de São Paulo](https://www.educacao.sp.gov.br/jogo-de-cartas-ensina-programacao-para-estudantes-da-rede-estadual-de-sao-paulo/) (host: educacao.sp.gov.br, policy: websearch)
- [Conheça o Logirunner](https://www.ufsm.br/unidades-universitarias/ct/2017/07/28/conheca-o-logirunner-jogo-de-cartas-que-ajuda-na-aprendizagem-de-logica-e-algoritmos) (host: ufsm.br, policy: websearch)
- [Level up your coding: a systematic review of personalized, cognitive, and gamified learning in programming education](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11623090/) (host: ncbi.nlm.nih.gov, policy: websearch)
- [The effectiveness of gamification in programming education: Evidence from a meta-analysis](https://www.sciencedirect.com/science/article/pii/S2666920X22000510) (host: sciencedirect.com, policy: websearch)
- [Collectible card game — Wikipedia](https://en.wikipedia.org/wiki/Collectible_card_game) (host: en.wikipedia.org, policy: websearch)
