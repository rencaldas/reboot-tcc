# CLAUDE.md — tcg-dev (jogo de puzzles sobre computação)

Este projeto é o TCC (Projeto Técnico) de Renato de Azevedo Caldas, Ciência da Computação, UVA. A especificação completa e o histórico de decisões estão em [`PROMPT_TCC.md`](PROMPT_TCC.md) — leia-o antes de qualquer trabalho relevante. Este arquivo contém apenas as regras de trabalho inegociáveis, copiadas da seção 2 do prompt.

## Regras de trabalho (inegociáveis)

1. **Plano antes de código.** Nunca crie ou edite código sem antes apresentar um plano curto de 3 passos e esperar o ok do autor.
2. **Bugs.** Faça o diagnóstico e aponte a causa raiz antes de propor a correção. Use o fluxo de bug do Spec Kit: `/speckit-bug-assess` → `/speckit-bug-fix` → `/speckit-bug-test`.
3. **Testes.** Nunca remova testes existentes. O domínio crítico (regras e validação dos puzzles, progresso e curadoria de jogos) tem testes escritos antes da implementação.
4. **Código.** Funções pequenas e modulares, com responsabilidade única. Nada de código comentado ou morto.
5. **Ambiguidade.** Diga "não sei" ou pergunte. Nunca chute uma implementação, um requisito, uma regra de norma ou uma referência bibliográfica.
6. **Ritmo.** Uma fase por vez. Ao fim de cada fase, pare, resuma o que foi feito e aguarde a revisão do autor.
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

## Conformidade legal (contexto permanente)

O jogo não tem compras, loot boxes nem coleta desnecessária de dados, e jogos de terceiros passam por curadoria antes da publicação. Ver seção 4.4 do `PROMPT_TCC.md`.

## Material arquivado

`arquivo/` guarda conceitos fora do projeto ativo, como o TCG. Não leia nem use essa pasta sem pedido do autor.
