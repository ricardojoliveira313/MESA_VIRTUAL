# RASTRO — excerto da virada 2026–2027

**Tipo:** excerto documental do RASTRO fornecido pelo Mediador em 25/10/2026.  
**Uso:** fonte primária do caso `VIRADA_2026_2027`.  
**Limite:** não é o RASTRO completo; preserva apenas as entradas que fundamentam a pergunta. O RASTRO completo permanece no repositório do MultEduc, fora de `C:\MESA_VIRTUAL`.

---

## 1. 2026-10-23 — Mesa Virtual MultEduc — pauta integral 24/10/2026

**Prioridade máxima registrada:**

> “**PRIORIDADE MÁXIMA — virada 2026 → 2027:** decisão expressa: ‘zerar 2027’ é **abrir novo ano, nunca apagar ou sobrescrever 2026**. Preservar íntegros e consultáveis registros de 2026 em qualquer aba, menu, submenu e documento, inclusive para auditoria. Criar seletor de ano e arquitetura que funcione também para 2028 em diante. Não duplicar identidade permanente de estudantes/docentes; separar matrículas, turmas, vínculos e lançamentos específicos de cada ano. Conferir todos os módulos (frequência, Bolsa Família, histórico escolar, documentos, avaliações, uniformes, bilhetes, Censo, digitalização, matrículas SED/creches, contatos e relatórios), distinguindo dados permanentes, anuais e históricos.”

**Trabalho existente:**

> “**Trabalho existente, não refazer:** PR [#602](https://github.com/ricardojoliveira313/diario_classe/pull/602), merged em `main`, concluiu a Fase 0, travando importação SED de anos diferentes de 2026 enquanto o cadastro não tem segregação. Há teste `test:ano-letivo` no CI. Não testado em navegador autenticado; no HTML/XLS sem cabeçalho do ano, a checagem é pelo seletor.”

**Diagnóstico:**

> “**Diagnóstico anotado em 22/10:** `Falta`, `LancamentoFaltas`, `BFFrequenciaRegistro`, `Bilhete`, cruzamentos, `Pendente`, `HistoricoAluno`, Censo e Digitalização já apresentam ano. `Turma` e `Aluno` não tinham segregação de ano. NÃO dizer que `Falta` tem só mês. Revalidar esquema atual antes de mudanças.”

**Fases pendentes:**

> “**Virada pendente:** Fase 1 — plano e migração aditiva de `Turma`/`Aluno` e vínculos, com cópia de segurança testada e rollback (N3, requer autorização específica); Fase 2 — importações, telas, APIs e relatórios filtrados pelo ano; Fase 3 — prova comparativa de integridade de 2026 antes/depois, isolamento 2027, testes de duplicidade/reexecução, permissões e fluxo completo.”

**Governança:**

> “**Governança e produção:** a discussão e este registro NÃO aprovam alterações no banco. Respeitar Manual-Mestre QC → VM → PE → PT → PO → IMF, classificação N3 para migrações/dados de produção, backups, reversão, testes, conferências e aprovação explícita do dono antes de aplicar/publicar.”

---

## 2. 2026-10-22 — Virada para 2027, Fase 0: trava da importação SED fora de 2026

**Diagnóstico literal:**

> “`Falta`, `LancamentoFaltas`, `BFFrequenciaRegistro`, `Bilhete`, cruzamentos, `Pendente`, `HistoricoAluno`, Censo e Digitalização já têm ano. `Turma` (nome único) e `Aluno` **não têm ano**: importar o SED de 2027 hoje moveria cada aluno (por RA) para a turma nova, reaproveitaria turmas de mesmo nome com a professora antiga e o ‘espelho fiel’ apagaria ATIVOS sem falta — a composição de 2026 se perderia. O seletor de ano não é salvo e abre no ano do relógio (em 01/01 abre 2027 sozinho).”

**Trava:**

> “A importação só aceita quando o ano do topo é 2026 **e** nenhum relatório traz ‘Ano Letivo’ diferente de 2026 (lido do cabeçalho dos PDFs do SED e do HTML). Fora disso, aviso vermelho e botão desabilitado. Nada muda para quem importa 2026 hoje.”

**Teste:**

> “Teste `test:ano-letivo` (caso: seletor em 2027; classe: arquivo 2027 com seletor em 2026; datas soltas não viram ano) no CI. Extração conferida nos PDFs reais do SED (Alunos por Classe, INFANTIL, FUNDAMENTAL, AEE → 2026). `tsc` ok.”

**Limite:**

> “Não verificado em navegador logado. O `.xls` ‘Relação de Alunos por Classe’ (HTML) não traz ‘Ano Letivo’ no texto: para ele vale só a checagem do seletor.”

**Pendente:**

> “Fase 1 (coluna de ano em `Turma`/`Aluno`, migração aditiva com cópia de segurança, só com aprovação do dono), Fase 2 (importação e telas pelo ano), Fase 3 (prova de que os números de 2026 não mudam).”

---

## 3. 2026-10-01 — Datas de matrícula: 1.095 correções

**Fato registrado:**

> “Causa: a planilha SED chega do XLSX com a data como texto no formato padrão do Excel (M/D/AA, ano de 2 dígitos); `fmtDate` não tratava esse formato e gravava como veio. […] Dados: 1.095 matrículas corrigidas (início e/ou fim) por migration; valor antigo guardado em `_bkp_aluno_datas_matricula_20261001` (RLS ligada, sem grants) para desfazer.”

**Risco para a virada:**

> “Impacto escondido: as funções de cálculo (`parseData`, entradas/saídas mensais) só aceitam DD/MM/AAAA; datas ‘2/4/26’ eram ignoradas por elas. Os números de entradas/matrículas por mês devem ser reconferidos na tela.”

---

## 4. 2026-10-25 — Definição da Mesa Virtual

**Regra de método:**

> “a Mesa Virtual é o método de discussão técnica e tomada de decisão **antes de implementar** alterações no MultEduc.”

> “(1) descrever problema, objetivos e restrições; (2) elaborar parecer inicial; (3) submeter aos demais integrantes; (4) confrontar análises, divergências, riscos e custos; (5) registrar consenso técnico, decisões e pendências; (6) obter aprovação expressa de Ricardo; (7) implementar apenas o escopo aprovado, testar, auditar e documentar; (8) registrar o estado real de publicação.”

---

## 5. Limites deste excerto

1. Não contém o RASTRO completo, o código, o schema real do Supabase, as três planilhas de Aprendizagem ou os repositórios do Vida Escolar 360.
2. As citações são das entradas do RASTRO fornecidas pelo Mediador; a localização é por data e título, não por página.
3. O excerto não prova que o estado atual do banco corresponde ao descrito; a auditoria de leitura é necessária.
4. Nenhum token, credencial ou dado pessoal é reproduzido aqui.
