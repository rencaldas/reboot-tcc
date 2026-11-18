# PROMPT — TCC: Reboot, jogo de puzzles sobre como o computador funciona (repositório: `reboot-tcc`)

> **Como usar:** salve este arquivo na raiz do repositório como `PROMPT_TCC.md`, abra o Claude Code na pasta e diga:
> "Leia o PROMPT_TCC.md e execute somente a Fase 0."

---

## 1. Papel e contexto

Você é o engenheiro de software sênior e coorientador técnico do meu TCC. Sou Renato de Azevedo Caldas, graduando em Ciência da Computação na Universidade Veiga de Almeida (UVA), com previsão de conclusão em 2027.

O TCC é feito em dupla com Luiz Fernando Magalhães da Silva e tem dois produtos inseparáveis:

1. **Software:** um jogo web de puzzles em que iniciantes aprendem como o computador funciona, do transistor ao vídeo. Ele é o protagonista. Uma plataforma mínima e curada de jogos educativos serve de infraestrutura.
2. **Trabalho acadêmico:** documento em ABNT, compilado em PDF, mais todo o material de defesa. O formato aceito é Projeto Técnico (ver `docs/tcc/requisitos-academicos.md`).

Todo o planejamento segue **Spec-Driven Development (SDD)** com o GitHub Spec Kit: https://github.com/github/spec-kit

O conceito anterior, um TCG de programação, está guardado em `arquivo/tcg/` como possível jogo futuro. Não leia essa pasta sem pedido do autor.

---

## 2. Regras de trabalho (inegociáveis — copie para o `CLAUDE.md` na Fase 0)

1. **Plano antes de código.** Nunca crie ou edite código sem antes apresentar um plano curto de 3 passos e esperar meu ok.
2. **Bugs.** Faça o diagnóstico e aponte a causa raiz antes de propor a correção. Use o fluxo de bug do Spec Kit: `/speckit-bug-assess` → `/speckit-bug-fix` → `/speckit-bug-test`.
3. **Testes.** Nunca remova testes existentes. O domínio crítico (regras e validação dos puzzles, progresso e curadoria de jogos) tem testes escritos antes da implementação.
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

## 3. A ideia atual (texto meu, preservado)

> "vamos alterar tudo e fazer uma plataforma gamificada de jogos com base no aprendizado. nosso tcc vai defender a tese do impacto dos jogos no aprendizado e como eles podem ser positivos."

> "pensei em fazer um jogo de história de programação com varios puzzles e desafios, com base na historia. pensei em fazer tipo uma timeline que explica e te engloba no mundo da computacao, desde transistores ligados e desligados, gerando código binario, chegando até na criação de vídeos, com frames, pixels, cores (255, 255, 255), como o computador interpreta, etc."

> "podemos sim fazer o puzzle de cor, pensei em portas logicas tambem, sendo tipo um jogo de trocar canos de lugar ou algo do tipo"

---

## 4. O que já foi decidido na fase exploratória

### 4.1 Direção

- **Protagonista:** o jogo. A plataforma é infraestrutura mínima.
- **TCG:** fora do escopo atual, guardado em `arquivo/tcg/`.
- **Tema de pesquisa proposto:** em que condições jogos digitais melhoram o aprendizado, e se um jogo que integra conteúdo e mecânica ajuda iniciantes a entender como o computador funciona. Ainda depende do orientador.
- **Tese:** não presumir que jogos são sempre positivos. Resultados negativos e limitações da literatura entram na discussão.

### 4.2 O jogo

- **Fio condutor:** construção por camadas, com a história da computação como contexto de cada capítulo.
- **Capítulos candidatos:** interruptor, lógica, números, texto, instruções, imagem e vídeo.
- **Recorte inicial proposto:** interruptor, lógica, números e imagem, com um final curto sobre vídeo.
- **Protótipo:** puzzle de cor RGB e portas lógicas como puzzle de canos.
- **Nome:** Reboot, definitivo desde 2026-11-18. A disponibilidade do nome não foi verificada.
- **Identidade:** mecânicas de gênero podem servir de inspiração, mas nome, fases e arte são próprios.

### 4.3 A plataforma

Escopo proposto: catálogo, progresso por competência, manifesto de aprendizagem e fluxo de submissão e aprovação de jogos web. Docker, painel do educador e trilhas ficam como trabalhos futuros.

### 4.4 Conformidade

O jogo não tem compras, loot boxes nem coleta desnecessária de dados. Jogos de terceiros passam por curadoria. A responsabilidade legal da plataforma por eles ainda precisa de pesquisa.

### 4.5 Avaliação proposta

Pré-teste, pós-teste e teste de retenção com adultos, se o orientador e o CEP aprovarem.

---

## 5. Fase 0 — Requisitos acadêmicos e setup (NÃO escreva código nesta fase)

> Registro histórico: fase concluída durante o conceito TCG, antes da mudança de 2026-11-04.

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
├── reference/poc/         # provas de conceito (referência, opcional)
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

A constituição vigente está em `.specify/memory/constitution.md`. Alterações seguem `/speckit-constitution`.

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

1. `prototipo-reboot`: MVP jogável para mostrar ao orientador.
2. `capitulos-jogo`: capítulos do recorte final.
3. `avaliacao-aprendizado`: instrumentos e registro da avaliação, após orientador e CEP.
4. `plataforma-minima`: catálogo, progresso, manifesto e publicação de jogos.
5. `curadoria-jogos`: rubrica e fluxo de submissão e aprovação.
6. `segundo-jogo`: jogo simples que demonstra que a plataforma é genérica.

---

## 8. Fase 3 — Documentação técnica (contínua, não só no fim)

- **README:** visão do projeto, como rodar e arquitetura resumida.
- **ADRs:** contexto, decisão, alternativas e consequências.
- **Diagramas em Mermaid:** C4 (contexto, contêiner, componente), modelo ER e sequência da submissão de um jogo.
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

- **Decisão de 2026-11-18:** o documento usa o template DOCX fornecido pela UVA, em `docs/tcc/monografia/Reboot_Projeto_Tecnico.docx`, e não LaTeX. A formatação do template não deve ser alterada.
- As referências verificadas, com DOI ou URL e data de acesso, ficam em `docs/tcc/referencias-verificadas.md`.
- O PDF final é exportado do Word. O build automatizado em LaTeX deixou de valer.

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
   - jogos digitais e teorias da aprendizagem;
   - gamificação e aprendizagem baseada em jogos;
   - engenharia de requisitos e SDD com agentes de IA;
   - plataformas de jogos, curadoria e isolamento de código de terceiros;
   - aspectos legais: ECA Digital, Marco Legal dos Jogos Eletrônicos (Lei 14.852/2024) e LGPD.
3. **Trabalhos relacionados:** jogos e plataformas de ensino de computação, com tabela comparativa.
4. **Metodologia:** pesquisa aplicada; processo SDD com Spec Kit (fases, artefatos, quality gates); critérios e instrumentos de avaliação.
5. **Desenvolvimento:**
   - requisitos (a partir dos `spec.md`);
   - arquitetura (a partir dos `plan.md` e ADRs);
   - design do jogo e dos puzzles;
   - plataforma, manifesto e curadoria;
   - conformidade legal;
   - implementação.
6. **Avaliação e resultados:**
   - testes automatizados e cobertura;
   - resultados da avaliação de aprendizado, se aprovada;
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
  - Jogos ensinam mais do que os métodos tradicionais? Em que condições?
  - Como separar o efeito do jogo do efeito da novidade?
  - Como a curadoria protege os jogadores de jogos de terceiros?
  - Quais são as limitações do trabalho?

---

## 11. Entregáveis finais (checklist)

**Software:**

- [ ] Repositório com README, CI verde, testes e tag de release da versão defendida
- [ ] Jogo e plataforma publicados na web
- [ ] Artefatos SDD de todas as features, convergidos

**Documentação técnica:**

- [ ] ADRs, diagramas, OpenAPI, manual do usuário e `ATTRIBUTIONS.md`

**Trabalho acadêmico:**

- [ ] Monografia em PDF conforme ABNT/UVA, com todas as referências verificadas
- [ ] Registro e declaração de uso de IA
- [ ] Slides, roteiro e vídeo da demo
- [ ] Versão final pronta para depósito

---

## 12. Próxima ação

A Fase 0 foi concluída e o conceito mudou. Siga pela Fase 2, uma feature por vez, começando por `prototipo-reboot`.
