# Prompt 05 — Configuracao de Hooks para Automacao Repetitiva

**Categoria:** Setup e Fundamentos
**Objetivo:** Identificar tarefas repetitivas que voce executa manualmente toda vez que o Codex termina uma mudanca (rodar lint, rodar testes, checar typecheck, formatar) e transformar isso em `hooks` configurados em `hooks.json`, de forma que o proprio harness execute automaticamente.
**Quando usar:** Quando voce percebe que toda sessao termina com "rode lint", "rode prettier", "rode tsc --noEmit". Isso e sinal de que voce esta desperdicando tokens pedindo ao Codex o que um hook faria de graca.

---

## Por que este prompt existe

Hooks sao comandos de shell que o Codex executa automaticamente em resposta a eventos (antes de uma tool ser chamada, depois de uma tool, ao terminar resposta). Hooks NAO rodam pelo modelo — rodam pelo harness. Isso significa:

- **Nao consomem tokens** (o modelo nao precisa "pensar" em chamar).
- **Nao esquecem** (rodam sempre, sem depender de o modelo lembrar).
- **Dao feedback estruturado** (Codex ve o resultado e reage).

Automatizar 3 ou 4 hooks comuns elimina boa parte do trabalho repetitivo.

---

## PROMPT

```
Quero configurar hooks no `.codex/hooks.json` deste projeto para automatizar tarefas repetitivas que hoje eu peco manualmente toda sessao.

Faca esta investigacao antes de sugerir qualquer hook:

1. Leia o `AGENTS.md`, `package.json`, `Makefile` (ou equivalente) para identificar os comandos de:
   - format (ex: `prettier --write`, `black`, `gofmt`)
   - lint (ex: `eslint`, `ruff`, `golangci-lint`)
   - typecheck (ex: `tsc --noEmit`, `mypy`, `pyright`)
   - test rapido (ex: `pytest -x --ff`, `npm test -- --watchAll=false`)
2. Verifique se `.codex/hooks.json` ja tem secao `hooks`. Se tiver, nao sobrescreva: merge.
3. Me pergunte: "quais destas tarefas voce quer que rodem automaticamente apos cada edicao de arquivo?"

Depois, proponha uma configuracao de hooks que inclua (apenas os que eu confirmar):

### Hook `PostToolUse` (apos Edit ou Write em arquivo de codigo)

- Rodar `format` no arquivo modificado (nao no projeto inteiro, por performance).
- Apenas para extensoes relevantes (ex: `.ts`, `.tsx`, `.js`, `.py`).
- Usar padrao glob no matcher para escopar.

### Hook `PostToolUse` (apos grupo de edits)

- Rodar `typecheck` incremental no projeto (apenas se o projeto tem typecheck < 5 segundos).
- Se demorar muito, NAO coloque como hook — vira gargalo. Sugira rodar manualmente no final.

### Hook `Stop` (quando Codex termina resposta)

- Rodar lint no escopo do que foi tocado na sessao.
- Opcional: rodar suite de testes relacionada aos arquivos mudados.

### Hook `PreToolUse` (antes de Bash)

- Opcional: bloquear comandos perigosos com pattern `git push --force*`, `rm -rf /`, etc. (defesa em profundidade alinhada ao sandbox e à política de aprovação).

## Formato do Output

Para cada hook proposto:
- Descreva em 1 frase o que ele faz.
- Mostre o JSON que entraria em `.codex/hooks.json`.
- Explique quais sao os custos (tempo de execucao, ruido no output) e quando desligar.

## Regras

- Nao proponha hook que rode suite de testes inteira se voce nao sabe quanto tempo dura. Pergunte antes.
- Nao proponha hook global — tudo escopado a este projeto.
- Hooks devem falhar de forma util: se o format nao existe, o hook nao deve quebrar a sessao.
- Nenhum hook pode fazer `git commit` automatico ou `git push`. Nunca.
- Apos implementar, me mostre exatamente como testar cada hook manualmente.
```

---

## Variacoes e Ajustes

**Projeto com CI lento mas local rapido:** adicione: *"Prefira rodar ferramentas locais agressivas (lint completo, typecheck) que sejam baratas, para nao empurrar problemas para CI."*

**Projeto com monorepo:** escope os hooks por pacote — nao rode lint do monorepo inteiro a cada edit.

**Equipe que nao quer hooks barulhentos:** peca hooks com `--quiet`, redirecionando output, e sugerindo que o Codex so reaja se houver erro.

---

## Dicas de uso

- Comece com 2 hooks simples: `format on Edit` + `lint on Stop`. Rode por 2-3 sessoes. Se nao atrapalhar, evolua.
- Se um hook comeca a atrapalhar mais do que ajudar (ex: lint demora 20 segundos), desligue ou reduza o escopo.
- Versione `.codex/hooks.json` no git para todos os devs do time herdarem.

## Sinal de que deu certo

Voce para de digitar "rode o lint" ao fim de tarefas. O Codex entrega ja formatado, com lint OK, e voce so revisa a mudanca semantica.
