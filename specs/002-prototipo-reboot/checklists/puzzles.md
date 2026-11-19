# Checklist de regras dos puzzles: Protótipo jogável do Reboot

**Objetivo**: avaliar se os requisitos das regras de cor, canos, portas lógicas, limite de erros, dicas e validação estão completos, claros, consistentes e mensuráveis.

**Criado em**: 2026-11-19

**Feature**: [spec.md](../spec.md)

**Nota**: checklist customizado gerado pelo `/speckit-checklist` a partir da spec, do plano e do modelo de dados.

**Responsabilidade da revisão**: este checklist pertence aos revisores (os autores). Marque `[x]` só quando a revisão concluir que o critério de qualidade do requisito foi atendido.

**Significado da marcação**: `[x]` quer dizer que o critério foi revisado e atendido na escrita dos requisitos. Não quer dizer que a implementação está pronta.

## Completude

- [ ] CHK001 A spec define o valor inicial dos três canais em cada fase de cor, inclusive ao recomeçar uma fase perdida? [Gap, Spec §FR-003, §FR-004]
- [ ] CHK002 Está definido o que o jogador vê nos canais que já estão certos depois de "Confirmar cor": só a ausência de retorno, ou uma indicação explícita de acerto? [Ambiguity, Spec §FR-005]
- [ ] CHK003 Está definido se o retorno "maior" ou "menor" de um canal continua visível ou some quando o jogador altera aquele canal? [Gap, Spec §FR-005]
- [ ] CHK004 Estão definidas as regras de acesso às 3 dicas: se ficam todas disponíveis desde o início, se abrem em sequência ou se dependem de erros? [Gap, Spec §FR-011]
- [ ] CHK005 Está definido se as duas fases de canos, juntas, precisam usar as três portas (AND, OR e NOT), já que a US2 revela as três ao final? [Gap, Spec §FR-008, US2 cenário 4]
- [ ] CHK006 Está definido se uma fase de canos pode combinar mais de uma junção, como um NOT ligado a um AND? [Gap, Spec §FR-008]
- [ ] CHK007 A spec define como o jogador "escolhe a junção" na fase de escolher: pegando uma de várias peças da bandeja ou trocando o tipo de uma junção já posicionada? [Ambiguity, Spec §FR-010, Clarificações 2026-11-18]
- [ ] CHK008 A spec diz se as peças de cano podem ser giradas pelo jogador, ou se já vêm na orientação definida pela fase? [Gap, Spec §FR-007]

## Clareza

- [ ] CHK009 O momento da derrota no FR-003 está escrito sem ambiguidade? "Permitir no máximo 3 confirmações erradas" pode ser lido como 3 erros permitidos e derrota no 4º, enquanto "na terceira, o jogador perde" indica derrota no 3º. [Clarity, Spec §FR-003]
- [ ] CHK010 Está definido o limite do que a dica mais direta (3º nível) pode mostrar, em relação a "sem revelar a resposta na primeira tentativa"? [Ambiguity, Spec §FR-011]
- [ ] CHK011 Está claro se a imagem de pixels do FR-006 é uma fase com interação própria ou uma tela de revelação depois da última fase de cor? [Clarity, Spec §FR-006, US1 cenário 4]

## Consistência

- [ ] CHK012 O retorno por combinação da US2 ("quais passaram e quais falharam") é consistente com o FR-011 na fase de prever? Como a saída é 0 ou 1, apontar uma previsão errada revela a resposta certa dela. [Conflict, Spec §FR-011, US2 cenário 2, Clarificações 2026-11-19]
- [ ] CHK013 O FR-009 ("combinações de entrada definidas na fase") é consistente com o contrato de conteúdo, que exige todas as combinações possíveis? [Consistency, Spec §FR-009, contracts/conteudo-fase.md]

## Cobertura de cenários e casos de borda

- [ ] CHK014 Está definido se confirmar com o caminho aberto (sinal não chega à saída) conta como confirmação errada numa fase com limite? [Gap, Spec §FR-003, §FR-009]
- [ ] CHK015 Está definido se o jogador pode confirmar uma fase de prever sem ter marcado todas as previsões? [Gap, Spec §FR-010]
- [ ] CHK016 Está definido o comportamento ao digitar um valor inválido num canal: fora de 0–255, decimal, vazio ou texto? [Edge Case, Gap, Spec §FR-004]
- [ ] CHK017 Está definido se confirmar de novo exatamente a mesma cor errada conta como mais uma confirmação errada? [Edge Case, Gap, Spec §FR-003, §FR-005]
- [ ] CHK018 Está definido o que acontece ao soltar uma peça numa célula já ocupada por outra peça móvel: troca, substituição ou retorno à bandeja? [Edge Case, Gap, Spec §FR-007, Edge Cases]
- [ ] CHK019 Está definido se, ao recarregar a página no meio de uma fase, a montagem dos canos e os valores dos canais são mantidos ou só o contador de erros? [Gap, Spec §FR-021, Key Entities §Progresso salvo]

## Mensurabilidade

- [ ] CHK020 O SC-006 define critérios para o conjunto de "soluções incorretas de teste" (quais tipos de erro precisam estar cobertos), para que o critério possa ser verificado de forma objetiva? [Measurability, Spec §SC-006]

## Notas

- Marque `[x]` só depois que a revisão confirmar que o critério de qualidade do requisito foi atendido.
- Deixe desmarcados os itens que ainda pedem clarificação, correção ou avaliação.
- O `/speckit-implement` lê o estado das marcações como gate e não as altera.
- `checklists/requirements.md` tem ciclo próprio, mantido pelo `/speckit-specify` e pelo `/speckit-clarify`.
- Itens não atendidos voltam para o `/speckit-clarify` ou para a edição da spec.
