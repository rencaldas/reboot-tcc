# Checklist de acessibilidade: Protótipo jogável do Reboot

**Objetivo**: avaliar se os requisitos de teclado, toque, leitor de tela, WCAG AA, independência de cor e alternativa ao arrastar estão completos, claros, consistentes e mensuráveis.

**Criado em**: 2026-11-19

**Feature**: [spec.md](../spec.md)

**Nota**: checklist customizado gerado pelo `/speckit-checklist` a partir da spec, do plano e do modelo de dados.

**Responsabilidade da revisão**: este checklist pertence aos revisores (os autores). Marque `[x]` só quando a revisão concluir que o critério de qualidade do requisito foi atendido.

**Significado da marcação**: `[x]` quer dizer que o critério foi revisado e atendido na escrita dos requisitos. Não quer dizer que a implementação está pronta.

## Completude

- [ ] CHK001 Os requisitos de teclado cobrem todos os elementos interativos, e não só o arrastar: canais, previsões, dicas, pular, silenciar, apagar progresso, falas do guia e quiz? [Coverage, Spec §FR-018]
- [ ] CHK002 Estão definidos os anúncios ao leitor de tela para cada mudança de estado relevante: retorno "maior"/"menor", resultado por combinação, erros restantes, derrota e conclusão de fase? [Gap, Spec §FR-018]
- [ ] CHK003 A alternativa por seleção do FR-007 especifica também como cancelar uma seleção e como devolver uma peça à bandeja? [Completeness, Spec §FR-007]
- [ ] CHK004 Há requisito para quem prefere movimento reduzido, já que o FR-019 pede animações e os Edge Cases admitem "movimento" como forma de indicar o sinal? [Gap, Spec §FR-019, Edge Cases]
- [ ] CHK005 Está definido o tamanho mínimo dos alvos de toque, como peças, células da grade e botões de 1 em 1? [Gap, Spec §FR-018]
- [ ] CHK006 Os efeitos sonoros que carregam informação (acerto, erro, derrota) têm equivalente visual ou textual exigido? [Gap, Spec §FR-020]
- [ ] CHK007 Está definido como a música começa (sozinha ou só depois de uma ação) e se o controle de silenciar fica acessível em todas as telas? [Gap, Spec §FR-020]
- [ ] CHK008 Estão definidos requisitos de foco ao trocar de tela ou de fase e ao abrir e fechar diálogos, como a confirmação de apagar progresso? [Gap, Spec §FR-018, §FR-021]

## Clareza

- [ ] CHK009 O FR-018 especifica qual versão da WCAG vale para o "nível AA" (2.1 ou 2.2)? [Clarity, Spec §FR-018]
- [ ] CHK010 Está explícito que nenhuma fala ou tela avança sozinha por tempo, ou o tempo de leitura das falas do guia está definido? [Clarity, Gap, Spec §FR-002]

## Consistência

- [ ] CHK011 A restrição "projetado para retrato" é consistente com o nível AA da WCAG, que não permite travar a orientação sem necessidade essencial? [Conflict, Spec Edge Cases, §FR-018, Clarificações 2026-11-17]
- [ ] CHK012 O Edge Case "quem não distingue bem as cores consegue resolver usando os valores numéricos" é consistente com o FR-005, que esconde os valores da cor-alvo? Está definido que o caminho sem percepção de cor é o retorno "maior"/"menor"? [Consistency, Spec Edge Cases, §FR-005]
- [ ] CHK013 O mesmo caminho do CHK012 está definido para quem usa leitor de tela, que não percebe a comparação lado a lado? [Coverage, Spec §FR-005, §FR-018]

## Casos de borda

- [ ] CHK014 Estão definidos requisitos de contraste para textos e rótulos exibidos sobre as amostras de cor, cujo fundo pode ter qualquer valor RGB? [Edge Case, Gap, Spec §FR-018]
- [ ] CHK015 Estão definidos requisitos de ampliação de texto e de largura mínima de tela (zoom e reflow)? [Gap, Spec §FR-018]

## Mensurabilidade

- [ ] CHK016 Há um critério de sucesso que torne a acessibilidade verificável, dizendo com quais tecnologias assistivas e em quais fluxos? Hoje nenhum SC trata de acessibilidade. [Measurability, Gap, Spec §Success Criteria]

## Notas

- Marque `[x]` só depois que a revisão confirmar que o critério de qualidade do requisito foi atendido.
- Deixe desmarcados os itens que ainda pedem clarificação, correção ou avaliação.
- O `/speckit-implement` lê o estado das marcações como gate e não as altera.
- `checklists/requirements.md` tem ciclo próprio, mantido pelo `/speckit-specify` e pelo `/speckit-clarify`.
- Itens não atendidos voltam para o `/speckit-clarify` ou para a edição da spec.
