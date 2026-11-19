# Checklist de conformidade e LGPD: Protótipo jogável do Reboot

**Objetivo**: avaliar se os requisitos sobre ausência de compras, anúncios e coleta, progresso local, quiz não salvo, licenças e fontes estão completos, claros e consistentes com a constituição.

**Criado em**: 2026-11-19

**Feature**: [spec.md](../spec.md)

**Nota**: checklist customizado gerado pelo `/speckit-checklist` a partir da spec, do plano e da constituição. Os itens perguntam se a spec trata cada tema. Não afirmam qual é a exigência legal, que deve ser confirmada na fonte (Princípio IV).

**Responsabilidade da revisão**: este checklist pertence aos revisores (os autores). Marque `[x]` só quando a revisão concluir que o critério de qualidade do requisito foi atendido.

**Significado da marcação**: `[x]` quer dizer que o critério foi revisado e atendido na escrita dos requisitos. Não quer dizer que a implementação está pronta.

## Completude

- [ ] CHK001 A spec exige que o jogo não faça requisições a serviços de terceiros (fontes, análise de uso, CDN)? Hoje essa restrição está só no plano. [Gap, Spec §FR-015]
- [ ] CHK002 A spec define se o jogador deve ser informado de que o progresso fica salvo no navegador, e onde essa informação aparece? [Gap, Spec §FR-021]
- [ ] CHK003 A spec define se o protótipo precisa de alguma informação sobre privacidade, mesmo sem coletar dados? [Gap, Spec §FR-015]
- [ ] CHK004 A spec define a faixa etária esperada do público do protótipo, já que a constituição considera crianças e adolescentes (Princípio VI)? [Gap, Assumption, Constituição §VI]
- [ ] CHK005 A spec, ou outro documento referenciado, define a licença sob a qual o código e o conteúdo do protótipo são distribuídos, agora que o repositório será público? [Gap]

## Clareza

- [ ] CHK006 Está claro se "coleta de dados pessoais" no FR-015 também abrange os registros de acesso que a hospedagem pode guardar, ou se isso fica fora do escopo da spec por depender do provedor? [Ambiguity, Spec §FR-015, Assumption]
- [ ] CHK007 O FR-014 define o que conta como "fato histórico" e como a fonte é apresentada ao jogador (link, referência, tela própria)? [Clarity, Spec §FR-014]
- [ ] CHK008 Existe um critério verificável para a fronteira entre "inspirado no estilo" e "reproduzir identidade" de outro jogo? [Measurability, Spec §FR-016, §FR-019]

## Consistência

- [ ] CHK009 O SC-004 e o SC-005 medem acertos no quiz, mas o FR-013 proíbe salvar ou enviar as respostas. Está definido como esses critérios são medidos, por exemplo pela observação do autor durante o teste? [Conflict, Spec §FR-013, §SC-004, §SC-005]
- [ ] CHK010 Está definido quais dados do teste informal com os 3 amigos são anotados pelo autor, onde ficam e se identificam os testadores, de forma consistente com a premissa de que o teste não é pesquisa? [Gap, Spec §SC-003, Assumptions]
- [ ] CHK011 A clarificação de 2026-11-19 (conteúdo rascunhado por IA) é consistente com o FR-022, que hoje exige declarar só os assets de arte e áudio gerados por IA, e não os textos? [Consistency, Spec §FR-022, Clarificações 2026-11-19]
- [ ] CHK012 O FR-015 (sem publicidade, compras ou loot boxes) é consistente com a constituição, que proíbe também recompensas aleatórias pagas? [Consistency, Spec §FR-015, Constituição §VI]

## Notas

- Marque `[x]` só depois que a revisão confirmar que o critério de qualidade do requisito foi atendido.
- Deixe desmarcados os itens que ainda pedem clarificação, correção ou avaliação.
- O `/speckit-implement` lê o estado das marcações como gate e não as altera.
- `checklists/requirements.md` tem ciclo próprio, mantido pelo `/speckit-specify` e pelo `/speckit-clarify`.
- Itens não atendidos voltam para o `/speckit-clarify` ou para a edição da spec.
