---
name: 47-prompts-avancados-codex
description: "Selecionar e aplicar os prompts migrados de 47-prompts-avancados-claude-code quando o usuário pedir um fluxo coberto por esta coleção. Não usar como substituto de uma skill mais específica já disponível."
---

## Compatibilidade Codex

Leia primeiro as instruções `AGENTS.md` aplicáveis e inspecione a stack real do projeto. Preserve escolhas explícitas do usuário e convenções existentes. As tecnologias citadas abaixo são preferências do material de origem, não autorização para substituir a stack atual.

Use as ferramentas disponíveis no Codex. Não presuma nomes de ferramentas do Claude, não exponha segredos e não execute publicação, envio, exclusão ou mudanças externas sem a autorização apropriada.

# Coleção de prompts para Codex

Escolha somente a referência mais próxima da tarefa atual e leia esse arquivo por completo antes de agir. Adapte placeholders, stack e caminhos ao projeto real; não copie instruções incompatíveis de forma literal.

## Catálogo

- `00-leia-primeiro.md` — LEIA PRIMEIRO — Como tirar o maximo destes 47 prompts
- `01-setup-e-fundamentos-01-criar-codex-md-perfeito.md` — Prompt 01 — Criar AGENTS.md Perfeito para o Projeto
- `01-setup-e-fundamentos-02-estrutura-codex-e-settings.md` — Prompt 02 — Estrutura Ideal de `.codex/` e Settings
- `01-setup-e-fundamentos-03-scope-limites-e-guardrails.md` — Prompt 03 — Definir Scope, Limites e Guardrails
- `01-setup-e-fundamentos-04-onboarding-codex-em-projeto-existent.md` — Prompt 04 — Onboarding do Codex em Projeto Existente
- `01-setup-e-fundamentos-05-configuracao-de-hooks.md` — Prompt 05 — Configuracao de Hooks para Automacao Repetitiva
- `02-economia-de-tokens-06-auditoria-consumo-tokens.md` — Prompt 06 — Auditoria de Consumo de Tokens do Projeto
- `02-economia-de-tokens-07-anti-context-bloat.md` — Prompt 07 — Estrategias Anti-Context-Bloat
- `02-economia-de-tokens-08-delegacao-cirurgica-subagentes.md` — Prompt 08 — Delegacao Cirurgica para Subagentes
- `02-economia-de-tokens-09-leitura-seletiva.md` — Prompt 09 — Leitura Seletiva vs Leitura Completa de Arquivos
- `02-economia-de-tokens-10-reducao-ruido-tool-results.md` — Prompt 10 — Reducao de Ruido em Tool Results
- `02-economia-de-tokens-11-caching-e-reuso-contexto.md` — Prompt 11 — Caching e Reuso de Contexto Entre Sessoes
- `03-assertividade-e-prevencao-12-bug-fix-cirurgico.md` — Prompt 12 — Bug Fix Cirurgico (Zero Scope Creep)
- `03-assertividade-e-prevencao-13-refatoracao-segura.md` — Prompt 13 — Refatoracao Segura sem Quebrar Nada
- `03-assertividade-e-prevencao-14-anti-alucinacao.md` — Prompt 14 — Anti-Alucinacao: Verificacao Obrigatoria
- `03-assertividade-e-prevencao-15-criterios-aceitacao-explicitos.md` — Prompt 15 — Criterios de Aceitacao Explicitos
- `03-assertividade-e-prevencao-16-anti-over-engineering.md` — Prompt 16 — Anti-Over-Engineering
- `03-assertividade-e-prevencao-17-prevencao-acoes-destrutivas.md` — Prompt 17 — Prevencao de Acoes Destrutivas Acidentais
- `03-assertividade-e-prevencao-18-checklist-pre-execucao.md` — Prompt 18 — Checklist Pre-Execucao de Tarefa Critica
- `04-planejamento-e-analise-19-plano-antes-de-codar.md` — Prompt 19 — Plano de Implementacao Antes de Codar
- `04-planejamento-e-analise-20-exploracao-codebase-desconhecido.md` — Prompt 20 — Exploracao Sistematica de Codebase Desconhecido
- `04-planejamento-e-analise-21-auditoria-arquitetural.md` — Prompt 21 — Auditoria Arquitetural de Projeto Legado
- `04-planejamento-e-analise-22-mapeamento-dependencias.md` — Prompt 22 — Mapeamento Completo de Dependencias
- `04-planejamento-e-analise-23-codigo-morto.md` — Prompt 23 — Identificacao de Codigo Morto
- `04-planejamento-e-analise-24-acoplamento-e-coesao.md` — Prompt 24 — Analise de Acoplamento e Coesao
- `05-qualidade-testes-e-seguranca-25-code-review-profundo.md` — Prompt 25 — Code Review Profundo (Nivel Staff Engineer)
- `05-qualidade-testes-e-seguranca-26-testes-unitarios-de-verdade.md` — Prompt 26 — Testes Unitarios que Realmente Testam
- `05-qualidade-testes-e-seguranca-27-testes-integracao-sem-mocks.md` — Prompt 27 — Testes de Integracao sem Mocks Enganosos
- `05-qualidade-testes-e-seguranca-28-auditoria-seguranca-owasp.md` — Prompt 28 — Auditoria de Seguranca (OWASP Top 10)
- `05-qualidade-testes-e-seguranca-29-cobertura-real-vs-aparente.md` — Prompt 29 — Verificacao de Cobertura Real vs Aparente
- `05-qualidade-testes-e-seguranca-30-smells-e-debito-tecnico.md` — Prompt 30 — Deteccao de Smells e Debito Tecnico
- `06-git-commits-e-colaboracao-31-commit-messages-semanticas.md` — Prompt 31 — Commit Messages Semanticamente Corretas
- `06-git-commits-e-colaboracao-32-pr-descriptions-ricas.md` — Prompt 32 — PR Descriptions que Facilitam Review
- `06-git-commits-e-colaboracao-33-resolucao-merge-conflicts.md` — Prompt 33 — Resolucao Segura de Conflitos de Merge
- `06-git-commits-e-colaboracao-34-rebase-interativo-seguro.md` — Prompt 34 — Rebase (e Reescrita de Historia) sem Perder Trabalho
- `06-git-commits-e-colaboracao-35-investigacao-git-log-blame.md` — Prompt 35 — Investigacao Profunda via git log e git blame
- `07-debug-sistematico-36-debug-por-root-cause.md` — Prompt 36 — Debug por Root Cause (Nao por Sintoma)
- `07-debug-sistematico-37-analise-stack-trace.md` — Prompt 37 — Analise Profunda de Stack Trace
- `07-debug-sistematico-38-bug-intermitente.md` — Prompt 38 — Reproducao de Bug Intermitente
- `07-debug-sistematico-39-profiling-performance.md` — Prompt 39 — Profiling de Performance
- `07-debug-sistematico-40-caca-memory-leak.md` — Prompt 40 — Caca ao Memory Leak
- `08-workflows-avancados-41-subagentes-paralelos.md` — Prompt 41 — Uso Correto de Subagentes Paralelos
- `08-workflows-avancados-42-segundo-par-de-olhos.md` — Prompt 42 — Segundo Par de Olhos (Review Independente)
- `08-workflows-avancados-43-red-team-seu-codigo.md` — Prompt 43 — Red Team: Quebrar seu Proprio Codigo
- `08-workflows-avancados-44-pair-programming-iterativo.md` — Prompt 44 — Pair Programming Iterativo
- `09-produtividade-diaria-45-standup-resumo-sprint.md` — Prompt 45 — Standup e Resumo Automatico de Sprint
- `09-produtividade-diaria-46-onboarding-relampago.md` — Prompt 46 — Onboarding Relampago em Repo Novo
- `09-produtividade-diaria-47-me-ensine-este-codigo.md` — Prompt 47 — Me Ensine Este Codigo (Aprendizado Guiado)
