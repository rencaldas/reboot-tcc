# Checklist de experiência e narrativa: Protótipo jogável do Reboot

**Objetivo**: avaliar se os requisitos de premissa, personagem guia, retorno de erro, explicações, encerramento e duração da experiência estão completos, claros, consistentes e mensuráveis.

**Criado em**: 2026-11-19

**Feature**: [spec.md](../spec.md)

**Nota**: checklist customizado gerado pelo `/speckit-checklist` a partir da spec, do plano e do modelo de dados.

**Responsabilidade da revisão**: este checklist pertence aos revisores (os autores). Marque `[x]` só quando a revisão concluir que o critério de qualidade do requisito foi atendido.

**Significado da marcação**: `[x]` quer dizer que o critério foi revisado e atendido na escrita dos requisitos. Não quer dizer que a implementação está pronta.

## Completude

- [ ] CHK001 Está definido o que acontece na narrativa quando uma fase é pulada: se ela devolve algo ao mundo e se a explicação da etapa ainda aparece? [Gap, Spec §FR-002, §FR-003, §FR-012]
- [ ] CHK002 Está definido o retorno ao jogador quando ele perde uma fase com limite: mensagem, comentário do guia e o que foi zerado? [Gap, Spec §FR-003]
- [ ] CHK003 Está definido se o jogador é avisado do limite de 3 confirmações erradas antes de começar uma fase com limite, e se vê quantas tentativas restam? [Gap, Spec §FR-003]
- [ ] CHK004 O "retorno específico" do FR-011 está definido para cada tipo de erro nos canos: caminho aberto, lógica errada e previsão errada? [Completeness, Spec §FR-011]
- [ ] CHK005 Está definido quando o personagem guia fala: só no início e na conclusão, ou também em erros, dicas e derrota? [Gap, Spec §FR-002]
- [ ] CHK006 Está definido o formato do quiz (múltipla escolha ou resposta livre) e se o jogador vê a resposta certa depois de responder? [Gap, Spec §FR-013]
- [ ] CHK007 Está definido o que o jogador vê ao reabrir o jogo: a premissa de novo ou direto a fase em andamento? [Gap, Spec §FR-021, Edge Cases]
- [ ] CHK008 Está definido o que acontece ao abrir o jogo depois de concluí-lo inteiro: encerramento, opção de jogar de novo ou início automático? [Gap, Spec §FR-021]
- [ ] CHK009 Está definido quais ações têm efeito sonoro e em quais telas há música? [Gap, Spec §FR-020]

## Clareza

- [ ] CHK010 "Falas curtas", "poucas frases" e "poucos segundos" estão quantificados, por exemplo em número máximo de frases ou de segundos de leitura? [Clarity, Spec §FR-002, §FR-012, US3 cenário 2]
- [ ] CHK011 O SC-001 e o SC-002 definem de onde a contagem de tempo começa e onde termina, como da abertura do link até "Começar" e até o fim do quiz? [Clarity, Spec §SC-001, §SC-002]
- [ ] CHK012 "Navegadores atuais de computador e celular" está especificado de forma verificável, com quais navegadores e versões mínimas valem para o SC-007? [Clarity, Spec §FR-001, §SC-007]

## Consistência

- [ ] CHK013 O FR-019 (visual final desenhado à mão) é verificável nesta feature, ou deve ser marcado explicitamente como fora do escopo do protótipo, de forma consistente com o FR-022 e as premissas que pedem arte provisória? [Consistency, Spec §FR-019, §FR-022, Assumptions]
- [ ] CHK014 A premissa de religar o mundo "camada por camada" é consistente com a ordem cor antes de canos, que as premissas justificam só pela compreensão imediata? [Consistency, Spec §FR-002, Assumptions]

## Mensurabilidade

- [ ] CHK015 "Cerca de 5 minutos para a maioria dos testadores" está mensurável com 3 testadores, dizendo quantos contam como maioria e qual tolerância vale como "cerca de"? [Measurability, Spec §SC-002, §SC-003]

## Notas

- Marque `[x]` só depois que a revisão confirmar que o critério de qualidade do requisito foi atendido.
- Deixe desmarcados os itens que ainda pedem clarificação, correção ou avaliação.
- O `/speckit-implement` lê o estado das marcações como gate e não as altera.
- `checklists/requirements.md` tem ciclo próprio, mantido pelo `/speckit-specify` e pelo `/speckit-clarify`.
- Itens não atendidos voltam para o `/speckit-clarify` ou para a edição da spec.
