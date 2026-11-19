# Quickstart: validar o protótipo do Reboot

**Feature**: `002-prototipo-reboot` | **Plano**: [plan.md](plan.md)

Guia para rodar o protótipo e provar, de ponta a ponta, que ele atende a spec. Os comandos valem depois que
o setup da implementação existir em `apps/reboot/`.

## Pré-requisitos

- Node 26 (versão em `apps/reboot/.nvmrc`) e npm.
- Um navegador atual de computador e um celular.
- Para o teste de acessibilidade: um leitor de tela (NVDA no Windows ou TalkBack no Android).

## Rodar localmente

```bash
cd apps/reboot
npm ci
npm run dev        # servidor local com recarga
npm test           # todos os testes (domínio, conteúdo e componentes)
npm run lint       # ESLint e checagem de tipos
npm run build      # build estático em dist/
npm run preview    # serve o build com o mesmo caminho base do GitHub Pages
```

**Esperado**: testes, lint e build sem erro.

## Validação automática

| O que prova | Onde | Requisitos |
|---|---|---|
| Conferência de cor por canal, só com "maior" ou "menor" | testes de domínio de cor | FR-004, FR-005 |
| Portas AND, OR e NOT e simulação do sinal | testes de domínio de canos | FR-008, FR-009 |
| Ligar sem a lógica certa é rejeitado; solução alternativa é aceita | testes de soluções | FR-010, SC-006 |
| Até 3 erros permitidos, derrota na 4ª e recomeço; pular só na 1ª fase | testes de jornada | FR-003 |
| Progresso ausente ou corrompido recomeça sem erro | testes de progresso | FR-021, Edge Cases |
| Invariantes do conteúdo | testes de conteúdo | [contrato de conteúdo](contracts/conteudo-fase.md) |
| Sem violações automáticas de acessibilidade | testes de componente com axe | FR-018 |

## Validação manual (antes de mostrar ao orientador)

Faça cada roteiro numa janela anônima, para começar sem progresso salvo.

1. **Abertura (US3)**: abra o link publicado. Clique em "Começar" em até 30 segundos (SC-001). Confirme que
   a premissa pode ser pulada e que não há cadastro, anúncio nem pedido de dado.
2. **Cor (US1)**: na 1ª fase, erre um canal de propósito e confirme. O canal recebe só "maior" ou "menor".
   Acerte com os botões de 1 em 1 e digitando o valor. Na 2ª fase, erre 3 vezes: a fase continua. Erre
   a 4ª: a fase recomeça do zero e a 1ª continua concluída. Ao final, veja a imagem de pixels com os valores.
3. **Canos (US2)**: na fase de prever, arraste peças, solte uma fora da grade (ela volta à bandeja), marque
   as previsões e confirme. Na fase de escolher, ligue o caminho com a junção errada: o teste mostra quais
   combinações falharam e a fase não é concluída.
4. **Encerramento (US3)**: confirme o resumo, o aviso de protótipo, a versão visível e o quiz, que mostra
   se cada resposta está certa. Recarregue a página: as respostas do quiz não ficaram salvas.
5. **Progresso (FR-021)**: no meio da etapa de canos, recarregue: o jogo volta à mesma fase. Use "Apagar
   progresso": o jogo volta ao início.
6. **Acessibilidade (FR-007, FR-018)**: jogue tudo só com teclado (Tab, setas, Enter, Esc), sem mouse.
   Repita uma fase de canos com leitor de tela e confirme que cada seleção e cada movimento são anunciados.
   Confirme que nenhuma informação depende só de cor. Ligue a opção de acessibilidade e conclua a 2ª fase de
   cor com leitor de tela, usando só a descrição da cor-alvo e o retorno "maior"/"menor": a fase não tem
   limite de erros. Gire o celular para paisagem: o jogo continua utilizável.
7. **Celular (SC-007)**: jogue do início ao fim em retrato, só com toque.
8. **Privacidade (FR-015)**: com as ferramentas do desenvolvedor abertas na aba Rede, jogue uma fase e
   confirme que nenhum pedido sai para outro domínio além do próprio site.

## Publicação

O merge na `main` publica o build em `https://rencaldas.github.io/reboot-tcc/`. Confira o roteiro 1 nesse
endereço depois de cada publicação.
