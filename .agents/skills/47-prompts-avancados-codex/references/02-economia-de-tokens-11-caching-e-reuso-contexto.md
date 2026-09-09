# Prompt 11 — Caching e Reuso de Contexto Entre Sessoes

**Categoria:** Economia Inteligente de Tokens
**Objetivo:** Estabelecer uma pratica de "memoria persistida" em arquivos `.md` versionados, para que conhecimento adquirido em uma sessao cara (ex: onboarding, auditoria, mapeamento de fluxo) seja reutilizado em sessoes futuras sem precisar reconstruir.
**Quando usar:** Sempre que voce gasta muitos tokens para o Codex entender algo que voce pretende consultar de novo no futuro.

---

## Por que este prompt existe

A cada sessao nova, o Codex "esquece" tudo — exceto o que estiver persistido em arquivos do repo (como `AGENTS.md` e outros docs que voce aponte). Muita gente gasta 30 mil tokens explicando arquitetura, pedindo mapeamento de fluxo, fazendo diagnostico, e nao salva nada. Na proxima sessao, paga de novo.

A estrategia e: tratar certas saidas do Codex como ativos, e persistir em um diretorio tipo `docs/codex/` que vira parte do "cache" do projeto.

---

## PROMPT

```
Quero estabelecer uma pratica de cache persistente de contexto neste projeto. Meu objetivo e que conhecimento caro gerado em uma sessao vire ativo reutilizavel.

## Etapa 1 — Estrutura

Crie (se nao existir) a pasta `docs/codex/` com os seguintes arquivos-indice vazios (ou preservando o que ja existe):

- `docs/codex/README.md` — indice descritivo de todos os arquivos de contexto
- `docs/codex/arquitetura.md` — visao de alto nivel (quando gerada)
- `docs/codex/fluxos.md` — fluxos de negocio mapeados
- `docs/codex/decisoes.md` — decisoes tecnicas e porque (ADRs leves)
- `docs/codex/glossario.md` — termos de dominio usados no codigo
- `docs/codex/dependencias.md` — o que cada dependencia externa faz e onde e usada

## Etapa 2 — Regras de persistencia

Daqui em diante, adotamos estas regras:

### Regra A — Ao mapear um fluxo

Se eu pedir "mapeie o fluxo de [X]", voce mapeia e DEPOIS adiciona uma secao em `docs/codex/fluxos.md` seguindo o template:

```
## Fluxo: [nome do fluxo]
**Entry point:** caminho:linha
**Descricao em 3 linhas:** ...
**Passos:**
1. ...
2. ...
**Pontos de erro:** ...
**Ultima verificacao:** [data]
```

### Regra B — Ao tomar decisao tecnica

Se em uma conversa aparecer uma decisao ("vamos usar Redis em vez de memoria"), voce adiciona em `docs/codex/decisoes.md`:

```
## DEC-XXX: [titulo curto]
**Data:** [hoje]
**Decisao:** ...
**Por que:** ...
**Alternativas consideradas:** ...
```

### Regra C — Ao descobrir termo de dominio

Se voce encontrar termo especifico do negocio cujo significado nao e obvio do codigo (ex: "shard de inventario", "ciclo fiscal"), adicione ao `glossario.md`:

```
- **termo**: definicao em 1-2 linhas. Onde aparece: caminho:linha
```

### Regra D — Ao gastar tokens em exploracao cara

Se voce fez uma investigacao que consumiu muita leitura (5+ arquivos) e que eu posso querer consultar de novo, PERGUNTE ao final: "quer que eu persista essa investigacao em `docs/codex/`?" Se eu disser sim, voce cria um arquivo novo ou adiciona ao existente.

## Etapa 3 — Como usar o cache

No comeco de toda sessao nova em que eu pedir algo relacionado a area ja mapeada, CONSULTE primeiro o `docs/codex/*` relevante antes de explorar codigo. Se a resposta estiver la, use-a. Se estiver desatualizada, confirme no codigo e atualize o doc.

## Regras rigidas

- Nada de cache gigante. Cada arquivo de cache tem no maximo 500 linhas. Se passar, divida.
- Nao cache coisas que mudam toda semana (ex: estado do board, bugs abertos). Cache apenas conhecimento estrutural.
- Sempre registre a data da ultima verificacao. Cache velho e armadilha.
- Portugues do Brasil.

## Confirme

Crie a estrutura, me mostre o README.md do indice, e depois confirme "cache de contexto ativo".
```

---

## Variacoes e Ajustes

**Projeto multi-repo:** crie um `docs/codex/` em cada repo, mas mantenha um unico no repo "principal" com links para os outros.

**Projeto que roda CI automatizado:** inclua no cache um `ci.md` descrevendo os workflows, porque explorar `.github/workflows/` e caro.

**Projeto com muita mudanca estrutural:** adicione hook semanal para revisar os caches e marcar o que esta obsoleto.

---

## Dicas de uso

- Versione `docs/codex/` no git. E conhecimento compartilhado.
- A cada X meses, dedique uma sessao a revisar os caches e remover obsoletos.
- Adicione no `AGENTS.md`: "Antes de explorar, verifique se ha informacao relevante em `docs/codex/`."

## Sinal de que deu certo

Sessoes em que voce pergunta algo que ja foi mapeado comecam com o Codex respondendo direto, sem precisar abrir codigo, porque ele consultou o cache.
