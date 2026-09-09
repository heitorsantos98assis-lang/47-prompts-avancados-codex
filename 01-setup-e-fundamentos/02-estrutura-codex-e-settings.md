# Prompt 02 — Estrutura Ideal de `.codex/` e Settings

**Categoria:** Setup e Fundamentos
**Objetivo:** Criar a pasta `.codex/` do projeto com `settings.json`, permissoes de ferramentas e subpastas de commands/agents/skills corretamente configuradas para o fluxo do seu time.
**Quando usar:** Ao adotar Codex em um projeto novo, ou ao padronizar o uso em um time para que todo mundo tenha as mesmas permissoes, hooks e skills.

---

## Por que este prompt existe

Muitos usuarios acumulam configuracao no `~/.codex/settings.json` global e esquecem que o projeto pode ter sua propria `.codex/` versionada. Isso gera dois problemas: (1) inconsistencia entre desenvolvedores do mesmo time; (2) o Codex pede confirmacao repetida para acoes que ja foram aprovadas por politica do projeto.

Este prompt faz o Codex auditar o que faz sentido padronizar por projeto e montar a `.codex/` de forma que o time inteiro herde o mesmo comportamento ao clonar o repo.

---

## PROMPT

```
Quero montar a pasta `.codex/` deste projeto, versionada no git, com configuracao padrao para toda a equipe.

Investigue primeiro:

1. Veja se ja existe `.codex/` na raiz. Se existir, liste o conteudo e leia o `settings.json` atual.
2. Leia o `AGENTS.md` se existir, para entender comandos que o time usa.
3. Identifique na stack do projeto:
   - Runner de testes e seu comando
   - Lint e format
   - Typecheck
   - Build
   - Comandos de migracao de banco (se houver)

Depois, crie ou atualize os seguintes arquivos:

### `.codex/settings.json`

Monte o settings com:

- `permissions.allow`: lista de comandos que o Codex pode rodar sem pedir autorizacao, limitada ao ESSENCIAL e SEGURO. Exemplos seguros tipicos:
  - `Bash(npm test)`, `Bash(npm run lint)`, `Bash(npm run typecheck)`, `Bash(npm run build)`
  - `Bash(git status)`, `Bash(git diff*)`, `Bash(git log*)`, `Bash(git branch*)`
  - Permissoes equivalentes da stack (pnpm, yarn, pytest, cargo, go test, etc)
- `permissions.deny`: lista de comandos perigosos que o Codex NAO pode executar mesmo se pedir autorizacao. Exemplos:
  - `Bash(rm -rf *)`, `Bash(git push --force*)`, `Bash(git reset --hard*)`
  - Qualquer comando que toque producao, deploy ou banco de producao
- `permissions.ask`: comandos que sempre pedem confirmacao explicita (git push normal, migracoes de banco, envio de email).
- `env`: variaveis de ambiente seguras para a sessao (NAO inclua secrets, apenas flags e caminhos).

### `.codex/commands/` (opcional)

Se identificar 2 ou mais tarefas repetitivas no fluxo do time, crie slash commands .md correspondentes com nome curto e descritivo. Exemplo: `/testar-modulo`, `/gerar-migration`, `/checar-deploy`.

### `.gitignore` — adicionar

- `.codex/local/` (para sobrescritas pessoais de cada dev)
- `.codex/*.log`

E garantir que `.codex/settings.json`, `.codex/commands/` e `.codex/agents/` SEJAM versionados (nao ignore).

REGRAS:
- Nada de permissao generica do tipo `Bash(*)`. Seja explicito.
- Nao inclua comandos que voce nao observou no projeto (nada de chutar).
- Nao crie hooks neste prompt (isso e assunto do Prompt 05).
- Portugues do Brasil nos comentarios.
- Apos criar tudo, liste o que foi criado e explique em 5 linhas por que cada permissao esta no allow (justifique a seguranca).
```

---

## Variacoes e Ajustes

**Projeto solo:** ignore a parte de `.codex/local/` e foque em permissoes que tornam seu fluxo mais rapido.

**Projeto com CI sensivel:** adicione ao prompt: *"Inclua em deny qualquer comando que modifique workflows em `.github/workflows/` sem confirmacao."*

**Projeto com banco de dados:** adicione: *"Qualquer comando de migracao deve estar em `permissions.ask`, nao em allow, mesmo que seja frequente. Migracoes exigem revisao humana."*

**Monorepo:** peca para o Codex rodar os comandos dentro de `cd packages/<nome>/ && ...` e registrar essa convencao no settings.

---

## Dicas de uso

- Leia o `settings.json` gerado antes de commitar. O Codex costuma acertar mas pode ser agressivo demais em allow.
- Prefira comecar restritivo e liberar conforme a confianca aumenta. E mais facil adicionar permissao do que descobrir que o Codex fez algo indevido.
- Documente no `AGENTS.md` que o projeto usa `.codex/` versionada, para novos devs saberem que ao clonar ja herdam a config.

## Sinal de que deu certo

Em uma sessao nova, peca: "rode os testes e o lint". O Codex deve executar direto, sem parar para pedir permissao, porque as acoes estao em `allow`. Se ele pede, seu `settings.json` ainda esta incompleto.
