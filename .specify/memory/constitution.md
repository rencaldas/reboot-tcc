# tcg-dev Constitution

Projeto de TCC: jogo web de puzzles sobre como o computador funciona, protagonista do trabalho,
publicado em uma plataforma curada mínima de jogos educativos. Ver `PROMPT_TCC.md` na raiz do repositório para o
roteiro completo (fases, backlog de features, requisitos acadêmicos); esta constituição registra
apenas os princípios não-negociáveis de engenharia derivados dele.

## Core Principles

### I. Código Limpo e Modular
Funções pequenas, com responsabilidade única. Não deve haver código morto ou comentado no
repositório.

**Rationale:** este é um projeto solo com IA como coautora de código; código enxuto e sem ruído é
o que permite revisão humana rápida e mantém a base auditável para a banca.

### II. Testes Primeiro no Domínio Crítico (NÃO NEGOCIÁVEL)
O domínio crítico — regras e validação dos puzzles, progresso do jogador e curadoria de jogos —
exige testes escritos antes da implementação. Testes existentes NUNCA são removidos, apenas
substituídos por versões que cobrem o mesmo comportamento ou mais. O CI bloqueia merge com qualquer
teste falhando.

**Rationale:** um puzzle que aceita a resposta errada ensina o conceito errado, e falhas de progresso
ou de curadoria comprometem a avaliação e a segurança dos jogadores.

### III. Plano Antes de Código
Toda criação ou edição de código começa com um plano curto de 3 passos, apresentado ao autor e
aprovado antes de qualquer implementação. Correções de bugs exigem diagnóstico e causa raiz
identificados antes da correção, seguindo o fluxo `/speckit-bug-assess` → `/speckit-bug-fix` →
`/speckit-bug-test`.

**Rationale:** mantém o autor — responsável final pelo trabalho perante a banca — no controle de
cada decisão de implementação, mesmo com IA gerando a maior parte do código.

### IV. Sem Suposições
Qualquer ambiguidade sobre requisito, implementação, regra de norma acadêmica ou referência
bibliográfica é marcada como `[NEEDS CLARIFICATION]` ou levada como pergunta direta ao autor.
Nunca se assume ou "chuta" uma resposta.

**Rationale:** este projeto tem consequências acadêmicas (avaliação por banca) e legais
(conformidade regulatória); uma suposição errada pode invalidar tanto uma seção do trabalho
acadêmico quanto uma decisão de arquitetura.

### V. Autoridade dos Dados Definida por Feature
Cada feature declara no plano onde fica a fonte da verdade de seus dados. Nenhum backend é presumido:
uma feature sem contas nem dados compartilhados pode rodar só no cliente. Qualquer dado usado em
avaliação, publicação ou aprovação de jogos DEVE ter validação no servidor.

**Rationale:** o protótipo precisa ser simples, enquanto dados que sustentam resultados ou decisões de
curadoria não podem depender de um cliente manipulável.

### VI. Conformidade por Design
O jogo e a plataforma não têm compras, loot boxes nem recompensas aleatórias pagas. O tratamento de
dados pessoais segue minimização (LGPD). Jogos de terceiros só são publicados após curadoria que
verifique esses requisitos. Qualquer interação entre jogadores exige salvaguardas antes de ser
habilitada.

**Rationale:** o público pode incluir crianças e adolescentes (Lei 15.211/2025, ECA Digital); a
conformidade não pode depender de uma interpretação favorável da lei.

### VII. Fontes e Licenças
Toda fonte externa e todo asset de terceiros têm termos de uso e licença registrados em
`ATTRIBUTIONS.md`, verificados na própria fonte. Todo fato histórico apresentado no jogo DEVE ter
fonte registrada. Nenhum texto protegido é copiado sem permissão. Nenhuma marca, logo, nome, fase ou
arte de outro jogo é usado.

**Rationale:** o jogo ensina, então um fato errado é um defeito de conteúdo; e usar material alheio
sem licença expõe o trabalho a problemas legais e acadêmicos.

### VIII. Reprodutibilidade
Fases e conteúdo são determinísticos e versionados: a mesma versão produz o mesmo puzzle e a mesma
validação. O build do software é reprodutível, com ambiente documentado. O PDF do trabalho acadêmico
é gerado por build automatizado.

**Rationale:** para comparar participantes em uma avaliação, é preciso saber exatamente qual versão do
jogo cada um usou.

### IX. Rastreabilidade Acadêmica
Toda decisão de arquitetura relevante gera um ADR. Cada feature registra decisões, problemas
encontrados, tempo gasto e métricas no diário de bordo (`docs/tcc/diario-de-bordo.md`). Nenhuma
referência bibliográfica entra no trabalho acadêmico sem verificação direta na fonte (DOI, URL ou
leitura do trecho citado).

**Rationale:** esse rastro é o que transforma o processo de desenvolvimento em evidência
utilizável no capítulo de Metodologia, e protege a integridade acadêmica do trabalho.

### X. Git e Controle de Versão
Nenhum commit ou push direto na `main`. Todo trabalho usa branches `feat/`, `fix/`, `chore/` ou
`docs/` (ou o padrão de branch numerada do Spec Kit para features SDD). Commits são pequenos,
atômicos, em Conventional Commits. Toda PR usa o template fixo definido no projeto e tem o título
prefixado pelo tipo do commit principal.

**Rationale:** garante histórico revisável e reconstituível — necessário tanto para qualidade de
engenharia quanto para a rastreabilidade acadêmica do Princípio IX.

## Governança

Esta constituição tem precedência sobre qualquer prática ou convenção não escrita do projeto. Ela
é o gate de conformidade verificado na seção "Constitution Check" do `/speckit-plan` para cada
feature, antes da fase de tasks.

**Emendas:** qualquer alteração de princípio segue o mesmo fluxo de qualquer outra mudança neste
projeto — nunca direto na `main` (Princípio X). Uma emenda é proposta via `/speckit-constitution`,
documentada nesta própria seção como um novo Sync Impact Report no momento da mudança, e só é
mesclada após revisão do autor.

**Versionamento:** semântico (MAJOR.MINOR.PATCH) — MAJOR para remoção ou redefinição incompatível
de princípio, MINOR para princípio novo ou expansão material, PATCH para redação/clarificação sem
mudança de substância.

**Revisão de conformidade:** toda spec, plano e PR é avaliada quanto à conformidade com estes
princípios antes do merge. Desvios identificados no `/speckit-analyze` ou no `/speckit-checklist`
voltam para a fase de origem (specify/clarify, plan ou tasks) para correção — nunca são ignorados
silenciosamente.

**Version**: 2.0.0 | **Ratified**: 2026-09-27 | **Last Amended**: 2026-11-04
