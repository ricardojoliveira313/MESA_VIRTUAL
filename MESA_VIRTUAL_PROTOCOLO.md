# MESA VIRTUAL — PROTOCOLO CENTRAL

**Versão:** 1.0
**Local:** `C:\processo_inventario\MESA_VIRTUAL\`
**Mediador:** Ricardo
**Analistas:** ChatGPT, DeepSeek, Claude
**Sistema:** OpenCode (execução, organização, registro)

---

## 1. Objetivo

A Mesa Virtual é o método padrão de análise dos casos do Mediador. Todos os analistas
trabalham sobre **as mesmas fontes**, respondem à **mesma pergunta**, em **rodadas
independentes**, e só depois confrontam seus pareceres. Nenhum documento sai da Mesa
sem autorização do Mediador.

## 2. Papéis

| Papel | Quem | Responsabilidade |
|---|---|---|
| **MEDIADOR** | Ricardo | Abre o caso, define a pergunta, seleciona fontes, intervém quando algo não convence, aprova ou reabre a análise, autoriza saída externa |
| **ANALISTA** | ChatGPT, DeepSeek, Claude | Produz parecer independente, revisa os demais, aponta divergências, sustenta cada afirmação com evidência |
| **SISTEMA** | OpenCode + MCPs | Cria e mantém a estrutura, executa os comandos da Mesa, registra o histórico e o estado do caso |

## 3. Estrutura de pastas

```
MESA_VIRTUAL/
├── MESA_VIRTUAL_PROTOCOLO.md      (este documento)
└── <NOME_DO_CASO>/
    ├── 00_FONTES/                 (fontes do caso + índice)
    ├── 01_PERGUNTAS/              (abertura e pergunta central)
    ├── 02_PARECER_CHATGPT/        (parecer independente)
    ├── 03_PARECER_DEEPSEEK/       (parecer independente)
    ├── 04_PARECER_CLAUDE/         (parecer independente)
    ├── 05_REVISAO_CRUZADA/        (cada analista comenta os demais)
    ├── 06_DIVERGENCIAS/           (matriz de divergências)
    ├── 07_CONSENSO/               (pontos pacificados + pendências)
    └── 08_DOCUMENTO_FINAL/        (só após aprovação do Mediador)
```

## 4. Ciclo da Mesa

1. **ABRIR MESA** — o Mediador define: caso, pergunta concreta e fontes.
2. **DISTRIBUIÇÃO** — as fontes são indexadas em `00_FONTES/INDICE_FONTES.md`.
3. **1ª RODADA (independente)** — cada analista produz seu parecer **sem ler os demais**.
4. **2ª RODADA (revisão cruzada)** — cada analista lê os outros pareceres e aponta
   concordâncias, divergências, erros, lacunas e novas evidências.
5. **MATRIZ DE DIVERGÊNCIAS** — tabela comparativa: ponto → posição de cada analista →
   evidência → risco → recomendação.
6. **REANÁLISE** — o Mediador pode pedir: *"voltem às fls. X"*, *"confiram este documento"*,
   *"essa afirmação pode nos prejudicar?"*, *"isso está realmente provado?"*.
7. **CONSENSO** — pontos pacificados + pontos que exigem decisão do Mediador.
8. **DOCUMENTO FINAL** — só é produzido **após aprovação** do Mediador.
9. **SAÍDA EXTERNA** — somente com autorização explícita do Mediador.

## 5. Estados do caso

```
ABERTA → EM_ANÁLISE → EM_REVISÃO → EM_CONSENSO → CONSOLIDADA → APROVADA
                                  ↑                                    │
                                  └──────────── REABERTA ←────────────┘
```

O estado fica registrado no topo de `01_PERGUNTAS/00_ABERTURA.md` e, futuramente,
será persistido automaticamente no Supabase.

## 6. Regras de evidência (duras)

1. **Toda afirmação relevante cita a fonte**: documento + página/fls.
2. **Dúvida vence suposição.** "Não há evidência nos autos" é uma resposta válida e útil.
3. **Citação literal** vai entre aspas, com localização exata.
4. **Distinção obrigatória** entre: fato provado / indício / hipótese / opinião jurídica.
5. **Nenhum analista cria documento inexistente.** Se faltar fonte essencial, a Mesa para
   e pergunta ao Mediador.

## 7. Confidencialidade e saída externa

- Nada sai da Mesa (e-mail, upload, impressão, envio a terceiros) sem ordem explícita do Mediador.
- O documento final em `08_DOCUMENTO_FINAL/` é gerado somente após aprovação.
- Rascunhos e pareceres intermediários permanecem internos.

## 8. Formato padrão de parecer

```markdown
# PARECER — <CASO> — <ANALISTA>
Data:
Pergunta central:
Resposta curta (1 parágrafo):
Fundamentos (numerados, cada um com fonte/fls.):
Riscos e pontos fracos da própria análise:
Pontos que exigem decisão do Mediador:
Fontes consultadas:
```

## 9. Formato da matriz de divergências

| # | Ponto | ChatGPT | DeepSeek | Claude | Evidência | Risco | Recomendação |
|---|---|---|---|---|---|---|---|

## 10. Comandos do OpenCode

| Comando | O que faz |
|---|---|
| `/abrir-mesa <caso> — <pergunta>` | Cria a estrutura do caso, indexa as fontes, registra a pergunta e produz o parecer DeepSeek |
| `/mesa-status <caso>` | Mostra o estado do ciclo, pareceres existentes/faltantes e o próximo passo |
| `/mesa-consolidar <caso>` | Roda revisão cruzada, matriz de divergências e consenso (exige os 3 pareceres) |

## 11. Registro histórico

Enquanto o Supabase não está ligado, o histórico é mantido nos próprios arquivos
(com data e autor em cada um). Quando a integração existir, ela registrará: quem
disse o quê, em qual versão, com qual evidência e o estado do caso em cada momento.

## 12. Roteiro de evolução

1. ✅ Mesa Virtual local (este protocolo + comandos).
2. ⬜ Espelhar/operar a Mesa no Google Drive (MCP google_workspace).
3. ⬜ GitHub (versionamento dos pareceres e documentos).
4. ⬜ Supabase (registro estruturado: decisões, versões, evidências, estados).
5. ⬜ Playwright (automação de navegação para coletas e conferências).
