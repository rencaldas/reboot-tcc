<!--
Sync Impact Report
- Version change: (nenhuma anterior) → 1.0.0
- Ratificação inicial. Princípios I–X adotados literalmente do conteúdo fornecido em
  PROMPT_TCC.md §6, com rationale adicionado a cada um onde não era óbvio pelo enunciado.
- Added sections: Governança (preenchida a partir do padrão do template).
- Removed sections: os slots opcionais do template padrão ([SECTION_2_NAME]/[SECTION_2_CONTENT]
  e [SECTION_3_NAME]/[SECTION_3_CONTENT], ex.: "Additional Constraints", "Development Workflow")
  foram omitidos. O conteúdo que ocupariam já está integralmente coberto pelos Princípios III
  (plano antes de código), VI (conformidade), IX (rastreabilidade) e X (git/PR) — preenchê-los
  de qualquer outra forma exigiria inventar conteúdo não fornecido, o que o Princípio IV proíbe.
- Templates a revisar: `.specify/templates/plan-template.md` já tem um gate "Constitution Check"
  (linha ~39) — nenhuma edição necessária agora, mas confirmar na primeira execução do
  `/speckit-plan` que o gate referencia estes princípios especificamente.
- TODOs pendentes: nenhum placeholder crítico deixado sem preenchimento.
-->

# tcg-dev Constitution

Projeto de TCC: TCG web e mobile com tema de programação e catálogo de cartas gerado
automaticamente a partir de dados públicos. Ver `PROMPT_TCC.md` na raiz do repositório para o
roteiro completo (fases, backlog de features, requisitos acadêmicos); esta constituição registra
apenas os princípios não-negociáveis de engenharia derivados dele.

## Core Principles

### I. Código Limpo e Modular
Funções pequenas, com responsabilidade única. Não deve haver código morto ou comentado no
repositório.

**Rationale:** este é um projeto solo com IA como coautora de código; código enxuto e sem ruído é
o que permite revisão humana rápida e mantém a base auditável para a banca.

### II. Testes Primeiro no Domínio Crítico (NÃO NEGOCIÁVEL)
O domínio crítico — pipeline de geração do catálogo, cálculo de raridade, RNG de abertura de
booster e economia — exige testes escritos antes da implementação. Testes existentes NUNCA são
removidos, apenas substituídos por versões que cobrem o mesmo comportamento ou mais. O CI bloqueia
merge com qualquer teste falhando.

**Rationale:** esses quatro subsistemas concentram tanto o risco legal (RNG e economia, ver
Princípio VI) quanto o risco de credibilidade acadêmica (reprodutibilidade do catálogo, ver
Princípio VIII); regressões neles são as mais caras de descobrir tarde.

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

### V. Servidor Autoritativo
Abertura de booster, geração de números aleatórios (CSPRNG), inventário do jogador e qualquer
operação de economia só podem ocorrer no backend. As drop rates de cada booster são versionadas e
exibidas ao jogador.

**Rationale:** RNG ou inventário controlado pelo cliente é trivialmente manipulável, o que quebra
tanto a integridade do jogo quanto a exigência de transparência de drop rates.

### VI. Conformidade por Design
Boosters são exclusivamente gratuitos, obtidos só por gameplay, sem nenhum caminho pago — direto
ou indireto via conversão de moeda premium — até o booster. Isso é garantido no ledger do backend,
não apenas na interface. O tratamento de dados pessoais segue minimização (LGPD). Qualquer
interação entre jogadores (troca, chat) exige salvaguardas antes de ser habilitada.

**Rationale:** a Lei 15.211/2025 (ECA Digital), art. 20, veda caixas de recompensa pagas em jogos
de acesso provável por crianças e adolescentes; este princípio existe para que o jogo nunca
dependa de uma reinterpretação favorável dessa lei para continuar em conformidade.

### VII. Dados e Licenças
Toda fonte de dados externa usada no pipeline do catálogo passa por cache local, respeita o rate
limit documentado da fonte, e tem seus termos de uso e licença registrados em `ATTRIBUTIONS.md`,
verificados na própria fonte. Nenhum texto de documentação de terceiros é copiado para descrições
de carta. Nenhuma marca ou logo de terceiros é usada.

**Rationale:** o catálogo depende de fontes como DevDocs (sem API oficial) e Stack Exchange
(rate-limited); descumprir isso pode tanto quebrar o pipeline por bloqueio da fonte quanto
infringir direitos autorais ou de marca.

### VIII. Reprodutibilidade
O catálogo é determinístico e versionado: rodar o pipeline de novo com os mesmos dados de entrada
produz exatamente o mesmo catálogo. Cartas já existentes nunca mudam de identidade nem são
removidas, mesmo que a fonte de origem mude ou desapareça. O ambiente de desenvolvimento roda em
Docker. O PDF da monografia/documentação é gerado por build automatizado.

**Rationale:** jogadores acumulam coleções ao longo do tempo; uma carta que muda de identidade
entre execuções do pipeline quebra a coleção de qualquer jogador que já a tenha. Reprodutibilidade
também é um requisito de rigor acadêmico para a avaliação quantitativa do catálogo.

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

**Version**: 1.0.0 | **Ratified**: 2026-09-27 | **Last Amended**: 2026-09-27
