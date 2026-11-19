# Contrato: progresso salvo no navegador

**Feature**: `002-prototipo-reboot` | **Requisitos**: FR-021, FR-015, Edge Cases (dado ausente ou corrompido)

## Armazenamento

- **Onde**: `window.localStorage`, só no navegador do jogador. Nada é enviado a servidor.
- **Chave**: `reboot:progresso`
- **Valor**: JSON no formato abaixo.

## Formato (versão de esquema 1)

```json
{
  "versaoEsquema": 1,
  "fases": {
    "cor-1": "concluida",
    "cor-2": "concluida",
    "canos-1": "pulada"
  },
  "emAndamento": { "faseId": "canos-2", "errosConfirmacao": 1 },
  "somMudo": false
}
```

| Campo | Regra |
|---|---|
| `versaoEsquema` | Exatamente `1`. |
| `fases` | Chaves são `id` de fases existentes no conteúdo; valores são `"concluida"` ou `"pulada"`. Uma fase com limite de erros nunca aparece como `"pulada"`. |
| `emAndamento` | `null` ou objeto com `faseId` existente e `errosConfirmacao` inteiro entre 0 e `limiteErros - 1` (0 em fases sem limite). |
| `somMudo` | Booleano. |

Nenhum outro campo é aceito. É proibido salvar nome, e-mail, identificador de aparelho, respostas do quiz ou
qualquer dado que identifique o jogador (FR-013, FR-021).

## Regras de leitura

1. Se a chave não existir, o jogo usa o progresso inicial: nenhuma fase concluída, nada em andamento, som
   ligado.
2. Se o valor não for JSON válido, ou violar qualquer regra da tabela acima, o jogo usa o progresso inicial
   sem mostrar erro ao jogador e sobrescreve o valor inválido no próximo salvamento.
3. Se `versaoEsquema` for diferente de `1`, aplica-se a regra 2. Uma mudança de esquema exige nova versão e
   regra de migração neste contrato.
4. Se o `localStorage` estiver indisponível (por exemplo, bloqueado pelo navegador), o jogo funciona
   normalmente, sem salvar, e não mostra erro.

## Regras de escrita

- O progresso é salvo ao concluir, pular ou perder uma fase, a cada confirmação errada e ao mudar o som.
- "Apagar progresso" remove a chave e volta ao início do jogo, depois de confirmação do jogador em um
  diálogo do próprio jogo.

## Testes obrigatórios (escritos antes da implementação, Princípio II)

- Leitura com chave ausente, JSON inválido, versão desconhecida, fase inexistente, valor de status inválido,
  contador fora da faixa, campo extra e fase com limite marcada como pulada: todos resultam no progresso
  inicial.
- Escrita seguida de leitura devolve o mesmo progresso.
- `localStorage` que lança exceção não interrompe o jogo.
