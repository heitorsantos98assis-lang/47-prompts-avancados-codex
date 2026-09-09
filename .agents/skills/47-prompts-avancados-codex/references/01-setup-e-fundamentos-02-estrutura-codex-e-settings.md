# Prompt 02 — Estrutura Ideal de `.codex/` e Settings

**Categoria:** Setup e Fundamentos
**Objetivo:** Criar a pasta `.codex/` do projeto com `config.toml`, `AGENTS.md`, skills e agentes corretamente configuradas para o fluxo do seu time.
**Quando usar:** Ao adotar Codex em um projeto novo, ou ao padronizar o uso em um time para que todo mundo tenha as mesmas permissoes, hooks e skills.

---

## Por que este prompt existe

Muitos usuarios acumulam configuracao no `~/.codex/config.toml` global e esquecem que o projeto pode ter sua propria `.codex/` versionada. Isso gera dois problemas: (1) inconsistencia entre desenvolvedores do mesmo time; (2) o Codex pede confirmacao repetida para acoes que ja foram aprovadas por politica do projeto.

Este prompt faz o Codex auditar o que faz sentido padronizar por projeto e montar a `.codex/` de forma que o time inteiro herde o mesmo comportamento ao clonar o repo.

---

## PROMPT

```
Quero montar a pasta `.codex/` deste projeto, versionada no git, com configuracao padrao para toda a equipe.

Investigue primeiro:

1. Veja se ja existe `.codex/` na raiz. Se existir, liste o conteudo e leia o `config.toml` atual.
2. Leia o `AGENTS.md` se existir, para entender comandos que o time usa.
3. Identifique na stack do projeto:
   - Runner de testes e seu comando
   - Lint e format
   - Typecheck
   - Build
   - Comandos de migracao de banco (se houver)

Depois, crie ou atualize os seguintes arquivos:

### `.codex/config.toml`

Monte apenas configurações oficiais e compatíveis com o Codex:

- `approval_policy`: defina quando o Codex deve solicitar aprovação, sem liberar ações externas de forma genérica.
- `sandbox_mode`: escolha o isolamento mínimo necessário para o trabalho local.
- `[sandbox_workspace_write]`: habilite rede ou raízes adicionais somente quando o projeto realmente precisar.
- Não inclua segredos, tokens, provedores ou autenticação na configuração versionada.

Registre comandos de teste, lint, typecheck e build em `AGENTS.md`; não invente uma lista `permissions.allow/deny/ask`, pois essa estrutura pertence ao formato de origem e não ao `config.toml` do Codex.

### `.agents/skills/` (opcional)

Se identificar 2 ou mais tarefas repetitivas no fluxo do time, crie skills em subpastas próprias, cada uma com `SKILL.md`, nome curto e descritivo. Exemplos de invocação: `$testar-modulo`, `$gerar-migration`, `$checar-deploy`.

### `.gitignore` — adicionar

- Configurações pessoais ficam em `$CODEX_HOME/config.toml` e não devem ser versionadas.
- `.codex/*.log` e outros artefatos locais, quando existirem.

Garanta que `AGENTS.md`, `.agents/skills/`, `.codex/agents/` e somente as configurações de projeto apropriadas sejam versionadas.

REGRAS:
- Não use `danger-full-access` nem `approval_policy = "never" como padrão compartilhado. Prefira o menor privilégio necessário.
- Nao inclua comandos que voce nao observou no projeto (nada de chutar).
- Não crie hooks neste prompt (isso é assunto do Prompt 05).
- Portugues do Brasil nos comentarios.
- Após criar tudo, liste o que foi criado e explique em 5 linhas como o sandbox e a política de aprovação reduzem riscos.
```

---

## Variacoes e Ajustes

**Projeto solo:** mantenha configurações pessoais em `$CODEX_HOME/config.toml` e foque em permissoes que tornam seu fluxo mais rapido.

**Projeto com CI sensivel:** adicione ao prompt: *"Registre em `AGENTS.md` e em hooks de política que qualquer comando que modifique workflows em `.github/workflows/` sem confirmacao."*

**Projeto com banco de dados:** adicione: *"Qualquer comando de migracao deve estar em a política de aprovação, nao como execução local segura, mesmo que seja frequente. Migracoes exigem revisao humana."*

**Monorepo:** peca para o Codex rodar os comandos dentro de `cd packages/<nome>/ && ...` e registrar essa convenção em `AGENTS.md`.

---

## Dicas de uso

- Leia o `.codex/config.toml` gerado antes de commitar. O Codex costuma acertar mas pode ser agressivo demais como execução local segura.
- Prefira comecar restritivo e liberar conforme a confianca aumenta. E mais facil adicionar permissao do que descobrir que o Codex fez algo indevido.
- Documente no `AGENTS.md` que o projeto usa `.codex/` versionada, para novos devs saberem que ao clonar ja herdam a config.

## Sinal de que deu certo

Em uma sessao nova, peca: "rode os testes e o lint". O Codex deve executar direto, sem parar para pedir permissao, quando isso estiver permitido pelo perfil de sandbox e aprovação escolhido. Se ele pede, revise a configuração e o perfil de permissões do Codex.
