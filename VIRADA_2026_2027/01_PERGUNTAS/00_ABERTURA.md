# ABERTURA — VIRADA_2026_2027

**Estado:** ABERTA  
**Data de abertura:** 25/10/2026  
**Mediador:** Ricardo  
**Sistema:** OpenCode  
**Caso:** VIRADA_2026_2027 — separação dos anos letivos 2026 e 2027 no MultEduc  
**Método:** QC → VM → PE → PT → PO → IMF

---

## 1. Pergunta central

**Qual arquitetura de dados e estratégia de migração permitem separar os anos letivos de 2026 e 2027 no MultEduc, preservando integralmente os registros históricos, a integridade das informações, as funcionalidades existentes e a segurança de acesso, sem comprometer as importações e os relatórios escolares?**

**Aprovação:** pergunta aprovada pelo Mediador em 25/10/2026, a partir da formulação proposta pelo ChatGPT, que aperfeiçoou a sugestão inicial do Claude. A formulação inclui expressamente integridade, funcionalidades existentes, segurança de acesso, importações e relatórios.

A pergunta é específica da virada 2026–2027. Não inclui, neste caso, a implementação de Aprendizagem, Telegram, Matrícula Digital, Portfólio ou reaproveitamento do Vida Escolar 360; esses temas podem ser cases separados.

---

## 2. Problema

O RASTRO registra que a Fase 0 da virada foi concluída com uma trava que impede a importação SED de anos diferentes de 2026, enquanto `Turma` e `Aluno` não têm segregação por ano. Também registra que o “espelho fiel” poderia apagar ATIVOS sem falta e que a composição de 2026 se perderia. A Fase 1 — modelo de ano, migração aditiva, backup, rollback e prova de integridade — permanece pendente e exige decisão arquitetural.

O caso deve responder:

- qual modelo de dados representa aluno, matrícula, turma, vínculo e ano letivo sem duplicar a pessoa nem perder o histórico;
- como migrar dados e código sem alterar 2026;
- como importar 2027 sem reabrir a janela de risco;
- como provar que relatórios e contagens de 2026 não mudaram;
- em que ordem executar, o que pode correr em paralelo e o que não pode;
- quais são os critérios de aceite e as condições de parada.

---

## 3. Objetivos

1. Definir a arquitetura conceitual e física da separação por ano.
2. Separar identidade permanente de matrícula anual e de vínculo com turma.
3. Definir o tratamento de transferência, remanejamento, abandono, retorno e mudança de turma no mesmo ano.
4. Definir a estratégia de migração, backup, rollback e prova de integridade.
5. Definir a ordem e o paralelismo permitido.
6. Definir critérios de aceite verificáveis.
7. Identificar riscos, dependências e decisões que exigem o Mediador.

---

## 4. Fora de escopo

- Implementar código, migração, deploy ou alteração de banco neste caso.
- Aprendizagem e as três planilhas (case separado).
- Telegram (case separado).
- Matrícula Digital, Portal das Famílias, Portfólio e Relatórios Descritivos (cases separados, se abertos).
- Reaproveitamento do Vida Escolar 360 (auditoria separada).
- Alterar critérios pedagógicos.
- Publicar qualquer documento fora da Mesa.

---

## 5. Fontes

| # | Fonte | Caminho/Referência | Tipo | Status |
|---:|---|---|---|---|
| 1 | Protocolo da Mesa Virtual | `C:\MESA_VIRTUAL\MESA_VIRTUAL_PROTOCOLO.md` | Norma interna | Disponível |
| 2 | RASTRO — excerto da virada 2026–2027 | `00_FONTES/RASTRO_2026-10-23_25_EXCERTO_VIRADA.md` | Excerto documental | Disponível |
| 3 | `anoLetivo.ts` | Repositório MultEduc, caminho informado: `frontend/src/anoLetivo.ts` | Código | **A confirmar / fornecer** |
| 4 | `schema.prisma` | Repositório MultEduc; verificar se é schema real ou `backend/prisma` morto | Código | **A confirmar / fornecer** |
| 5 | Usos de `Turma` e `Aluno` no frontend | Repositório MultEduc; Claude relata 11 arquivos | Código | **A confirmar / fornecer** |
| 6 | Esquema real do Supabase | Banco de produção; acesso de leitura não disponível nesta sessão | Banco | **Ausente / a confirmar** |
| 7 | `AGENTS.md` e `docs/MANUAL-MESTRE.md` | Repositório MultEduc | Norma interna | **A confirmar / fornecer** |
| 8 | PR #602 e teste `test:ano-letivo` | Repositório MultEduc / GitHub | Código e CI | **A confirmar** |
| 9 | Relatórios e consultas de 2026 | Banco e frontend do MultEduc | Dados e código | **Ausente / a confirmar** |
| 10 | Discussão preliminar do caso `MULTEDUC_BOT_TELEGRAM` | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\` | Histórico | **Não é prova** |

**Regra:** nenhum analista pode criar fonte, página, linha, commit ou objeto de banco inexistente. Se a fonte essencial faltar, a Mesa para e pergunta ao Mediador.

---

## 6. Adaptação das regras de evidência para código e banco

O protocolo nasceu para casos documentais. Para este caso, a localização da evidência passa a ser:

| Tipo de evidência | Formato obrigatório |
|---|---|
| Código | `arquivo:linha` ou `arquivo#Lx-Ly`, com commit/PR quando disponível |
| Banco | objeto (tabela/coluna/política/função), consulta usada e data da leitura |
| RASTRO | data + título da entrada, com citação literal entre aspas |
| CI/teste | nome do teste, comando e resultado observado |
| Documento | arquivo e seção; “fls.” apenas se houver paginação real |
| Hipótese | marcada como `[HIPÓTESE]`, com o que falta para virar fato |

Classificação obrigatória de cada afirmação:

- **FATO PROVADO** — evidência reproduzível;
- **INDÍCIO** — evidência parcial, não conclusiva;
- **HIPÓTESE** — proposta a testar;
- **OPINIÃO** — julgamento do analista, sem pretensão de fato.

---

## 7. Independência da 1ª rodada

A discussão anterior no caso `MULTEDUC_BOT_TELEGRAM` foi **exploratória** e não conta como 1ª rodada formal deste caso. Nela, os analistas leram uns aos outros antes de concluir, o que reduz o valor da comparação.

**Limitação registrada:** ChatGPT e Claude já leram os argumentos um do outro; DeepSeek também teve acesso à discussão preliminar. A independência perfeita desta rodada é impossível. O caso não vai fingir que ela existe.

Regras desta rodada:

1. Cada analista parte das **fontes originais**, não dos pareceres preliminares.
2. Os pareceres preliminares podem indicar temas e dúvidas, mas **não podem ser citados como prova** de fato.
3. Cada parecer deve declarar a limitação de contaminação e o que foi efetivamente verificado.
4. O Sistema entrega a mesma pergunta e as mesmas fontes a todos.
5. O Sistema não revela o novo parecer de um analista aos outros antes de todos entregarem.
6. Depois de todos entregarem, começa a revisão cruzada.
7. DeepSeek produzirá seu parecer formal somente a partir das fontes autorizadas, sem usar a discussão preliminar como evidência.

---

## 8. Critérios de parada

A Mesa para e pergunta ao Mediador se:

- o esquema real do Supabase não estiver disponível para leitura;
- o `schema.prisma` não puder ser identificado como real ou morto;
- os usos de `Turma`/`Aluno` não puderem ser listados com arquivo e linha;
- o PR #602 e o teste `test:ano-letivo` não puderem ser conferidos;
- houver conflito entre RASTRO e código que não possa ser resolvido por evidência;
- a migração proposta exigir alteração destrutiva sem backup/rollback;
- o Mediador não aprovar o escopo.

---

## 9. Resultado esperado

1. `02_PARECER_CHATGPT/PARECER_CHATGPT.md`
2. `03_PARECER_DEEPSEEK/PARECER_DEEPSEEK.md`
3. `04_PARECER_CLAUDE/PARECER_CLAUDE.md`
4. `05_REVISAO_CRUZADA/` — cada analista comenta os demais
5. `06_DIVERGENCIAS/MATRIZ_DIVERGENCIAS.md`
6. `07_CONSENSO/CONSENSO.md`
7. `08_DOCUMENTO_FINAL/` — somente após aprovação do Mediador

Formato mínimo de cada parecer: o da seção 8 do protocolo, adaptado para código/banco.

---

## 10. Controle de estado

| Data | Estado | Evento |
|---|---|---|
| 25/10/2026 | ABERTA | Caso formal criado; pergunta e fontes registradas; 1ª rodada independente convocada |
| 25/10/2026 | ABERTA | Nome `VIRADA_2026_2027` e pergunta central aprovados pelo Mediador; limitação de independência registrada |

> **Regra:** nada sai da Mesa e nada é implementado sem autorização explícita do Mediador.
