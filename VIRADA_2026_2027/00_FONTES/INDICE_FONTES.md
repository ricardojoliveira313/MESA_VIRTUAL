# Índice de fontes — VIRADA_2026_2027

**Caso:** VIRADA_2026_2027  
**Abertura:** 25/10/2026  
**Mediador:** Ricardo  
**Estado:** ABERTA  
**Pergunta central aprovada:** “Qual arquitetura de dados e estratégia de migração permitem separar os anos letivos de 2026 e 2027 no MultEduc, preservando integralmente os registros históricos, a integridade das informações, as funcionalidades existentes e a segurança de acesso, sem comprometer as importações e os relatórios escolares?”

---

## 1. Fontes disponíveis

| # | Fonte | Caminho | Tipo | Resumo | Status |
|---:|---|---|---|---|---|
| 1 | Protocolo da Mesa Virtual | `C:\MESA_VIRTUAL\MESA_VIRTUAL_PROTOCOLO.md` | Norma interna | Papéis, ciclo, estados, regras de evidência, confidencialidade e formato de parecer | Lido integralmente |
| 2 | RASTRO — excerto da virada 2026–2027 | `C:\MESA_VIRTUAL\VIRADA_2026_2027\00_FONTES\RASTRO_2026-10-23_25_EXCERTO_VIRADA.md` | Excerto documental | Prioridade da virada, Fase 0, diagnóstico, Fase 1 pendente, datas e governança | Lido integralmente |
| 3 | Abertura formal | `C:\MESA_VIRTUAL\VIRADA_2026_2027\01_PERGUNTAS\00_ABERTURA.md` | Documento de abertura | Pergunta aprovada, escopo, fontes, critérios de parada | Lido integralmente |

## 2. Fontes essenciais ainda não disponíveis

| # | Fonte | Caminho/Referência | Tipo | O que se espera | Status |
|---:|---|---|---|---|---|
| 4 | `anoLetivo.ts` | Repositório MultEduc — `frontend/src/anoLetivo.ts` (caminho informado por Claude) | Código | Regras atuais de ano letivo, seletor e trava | **A confirmar / fornecer** |
| 5 | `schema.prisma` | Repositório MultEduc; confirmar se é schema real ou `backend/prisma` morto | Código | Modelo de dados declarado | **A confirmar / fornecer** |
| 6 | Usos de `Turma` e `Aluno` no frontend | Repositório MultEduc; Claude relata 11 arquivos | Código | Arquivo, linha e tipo de leitura/escrita | **A confirmar / fornecer** |
| 7 | Esquema real do Supabase | Banco de produção; acesso de leitura não disponível nesta sessão | Banco | Tabelas, colunas, FKs, índices, RLS, grants, funções, views e Storage | **Ausente / a confirmar** |
| 8 | `AGENTS.md` e `docs/MANUAL-MESTRE.md` | Repositório MultEduc | Norma interna | Regras de engenharia, QC/VM/PE/PT/PO/IMF, N3 e aprovação | **A confirmar / fornecer** |
| 9 | PR #602 e `test:ano-letivo` | GitHub do MultEduc / CI | Código e teste | Diferença, commit, resultado do teste e cobertura | **A confirmar** |
| 10 | Relatórios e consultas de 2026 | Banco e frontend do MultEduc | Dados e código | Contagens, somas, amostras e hashes para prova de integridade | **Ausente / a confirmar** |

## 3. Fonte histórica — não é prova

| Fonte | Caminho | Uso permitido | Uso proibido |
|---|---|---|---|
| Discussão preliminar do caso `MULTEDUC_BOT_TELEGRAM` | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\` | Identificar temas, dúvidas e riscos já levantados | Citar como prova, como parecer formal ou como substituto das fontes originais |

**Nota:** a discussão preliminar foi preservada como histórico. Ela não conta como 1ª rodada formal do caso `VIRADA_2026_2027` e não deve ser usada como evidência de fato.

---

## 4. Regras de uso das fontes

1. **Código:** citar `arquivo:linha` ou `arquivo#Lx-Ly`; informar commit/PR quando disponível.
2. **Banco:** citar objeto (tabela/coluna/política/função), consulta somente leitura usada e data da leitura.
3. **RASTRO:** citar data + título da entrada, com citação literal entre aspas.
4. **Teste/CI:** citar nome do teste, comando e resultado observado.
5. **Documento:** citar arquivo e seção; “fls.” apenas se houver paginação real.
6. **Hipótese:** marcar `[HIPÓTESE]` e dizer o que falta para virar fato.
7. **Fonte ausente:** declarar a ausência; não preencher com suposição.
8. **Nenhum analista** pode criar fonte, linha, commit, página ou objeto de banco inexistente.

---

## 5. Critérios de parada ligados às fontes

A Mesa para e pergunta ao Mediador se:

- o esquema real do Supabase não estiver disponível para leitura;
- o `schema.prisma` não puder ser identificado como real ou morto;
- os usos de `Turma`/`Aluno` não puderem ser listados com arquivo e linha;
- o PR #602 e o teste `test:ano-letivo` não puderem ser conferidos;
- os relatórios de 2026 não puderem ser medidos antes/depois;
- houver conflito entre RASTRO e código que não possa ser resolvido por evidência.

---

## 6. Observação sobre independência

ChatGPT e Claude já leram os argumentos um do outro na discussão preliminar. A independência perfeita da nova 1ª rodada é impossível; o caso registra essa limitação. Para reduzir o viés:

- cada analista deve produzir sua análise a partir das fontes originais;
- os pareceres preliminares não podem ser citados como prova;
- o Sistema não deve revelar o novo parecer de um analista aos demais antes de todos entregarem;
- o parecer final deve declarar o que é fato, indício, hipótese e opinião.

DeepSeek produzirá seu parecer formal a partir das fontes autorizadas, sem usar a discussão preliminar como evidência.
