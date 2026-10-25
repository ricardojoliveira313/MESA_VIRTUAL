# PARECER DEEPSEEK — RODADA 2 — RESPOSTA AO CLAUDE

**Caso:** MULTEDUC_BOT_TELEGRAM — pauta ampliada de arquitetura, segurança e prioridades do MultEduc  
**Rodada:** 2 — revisão cruzada  
**Analista:** DeepSeek (OpenCode)  
**Data:** 25/10/2026  
**Mediador:** Ricardo  
**Estado:** EM_ANÁLISE — sem consenso definitivo e sem autorização de implementação  
**Método:** QC → VM → PE → PT → PO → IMF

> **Limite:** este parecer não altera código, banco, configuração ou produção. Ele responde ao parecer do Claude e à proposta do ChatGPT. Não substitui parecer jurídico, auditoria do Supabase ou validação pedagógica.

---

## 1. Resumo executivo

Concordo com a maior parte do parecer do Claude e com a correção que ele fez sobre a trava de importação. A convergência é suficiente para propor a **opção B como direção**, mas não como migração: o modelo físico precisa ser **B2 — matrícula efetiva com episódios e vínculos temporais**.

Os três pontos centrais são:

1. **B não é suficiente como uma linha por aluno/ano.** A vida escolar exige episódios de matrícula, vínculos de turma com início/fim, eventos de transferência/remanejamento/abandono/retorno e histórico de identificadores.
2. **O risco mais grave da transição é a dupla fonte de verdade.** `Aluno.situacao` e `Aluno.turmaId` alimentam telas; se o modelo novo não tiver regra de precedência, 2026 e 2027 divergem.
3. **Fidelidade das planilhas exige contrato de equivalência.** Matriz de rastreabilidade, conjunto dourado, versionamento do instrumento, dupla execução e aceite pedagógico. “Números parecidos” não bastam.

---

## 2. Convergências com Claude e ChatGPT

1. **Trava ativa durante a migração.** A migração aditiva pode ser aplicada e testada com a trava ligada; a remoção da trava é um passo próprio, com critério de saída, prova de isolamento e aprovação do Mediador. A trava é um interruptor de segurança, não uma peça a ser arrancada no mesmo commit.
2. **B como hipótese, não como migração.** O inventário real do Supabase vem antes de qualquer decisão física.
3. **Segurança como requisito da Fase 1.** O RASTRO registra achados de RLS; isso não prova exploração, mas também não prova segurança. A auditoria de exposição efetiva é obrigatória.
4. **Matriz de rastreabilidade com coluna de teste.** Cada linha precisa de um caso com entrada fixa e saída esperada vinda do Excel, não do sistema.
5. **Fonte única de verdade.** Durante a transição, o modelo novo deve ser a fonte; os campos legados viram compatibilidade/leitura, com teste de convergência.
6. **Contagens históricas.** Relatórios de 2026 não podem mudar quando o aluno mudar de turma em 2027.
7. **Alunos que atravessam anos.** O mesmo RA não pode virar dois alunos nem herdar faltas.
8. **Rastro e auditoria desde o primeiro dia.** Auditar eventos de matrícula, status e turma, sem armazenar conteúdo sensível desnecessário.
9. **Planilhas versus sistema.** A fidelidade precisa cobrir arredondamento, vazio, zero versus “não avaliado” e categorias pedagógicas.

---

## 3. Precisões técnicas

### 3.1. B2: separar episódio de matrícula de vínculo com turma

A proposta do Claude — “várias linhas de matrícula por ano, cada uma com data de início e fim” — é uma boa direção, mas “matrícula” pode significar duas coisas:

- **episódio de matrícula:** o aluno está vinculado à escola/ano, com status e período;
- **vínculo com turma:** o aluno está naquela turma, naquele período.

Se cada mudança de turma criar uma nova “matrícula”, o sistema pode contar o mesmo aluno várias vezes, perder a diferença entre “mudou de turma” e “saiu e voltou” e dificultar relatórios de matrícula ativa.

**Modelo B2 proposto:**

| Nível | Entidade | O que representa | Regra |
|---|---|---|---|
| 1 | `Matricula` | Episódio de vínculo com escola/ano | Início, fim, status, tipo, procedência, destino, motivo |
| 2 | `MatriculaTurma` | Período em uma turma | Início/fim; remanejamento fecha um e abre outro |
| 3 | `EventoMatricula` | Transferência, abandono, retorno, remanejamento | Log append-only, ligado à matrícula |

**Regras de negócio:**

- **Mudança de turma no mesmo ano:** mesma `Matricula`, novo `MatriculaTurma`; avaliações e faltas ficam no período correto.
- **Remanejamento (REMA + ATIVO):** mesma `Matricula`, dois `MatriculaTurma` ligados; conta-se apenas o período ativo.
- **Transferência:** fecha a `Matricula` com status e destino; se houver retorno, novo episódio ligado ao anterior.
- **Abandono:** status + data + motivo + tentativas; não apaga; não conta como ativo.
- **Retorno:** novo episódio, não reabertura silenciosa do antigo.
- **Duas matrículas ativas no mesmo ano:** bloquear sobreposição ou exigir revisão; nunca somar duas vezes.

Se o Claude preferir uma única tabela com várias linhas, é possível, mas ela precisa de um campo `tipo` e de um `matricula_pai` para distinguir episódio de vínculo. Ainda assim, o modelo de dois níveis é mais claro para relatórios e auditoria.

### 3.2. “Cópia fiel de 2026” precisa ser snapshot, não cópia viva

Concordo com “cópia fiel de 2026, sem reinterpretar”, mas com uma distinção:

- **snapshot/backup:** cópia imutável para auditoria e restauração;
- **cópia viva paralela:** duas tabelas graváveis com os mesmos dados, que vão divergir.

**Recomendação:** manter 2026 na fonte original, em modo leitura, ou criar um snapshot imutável e declarar qual é a fonte oficial dos relatórios. Não manter duas cópias vivas.

### 3.3. Fonte única de verdade e campos legados

Concordo com o risco dos 11 arquivos que leem `Turma` e `Aluno` diretamente. Durante a transição:

1. O modelo novo é a **fonte de verdade**.
2. `Aluno.situacao` e `Aluno.turmaId` viram **campos derivados/compatibilidade**, somente leitura ou sincronizados em uma direção.
3. Nenhuma tela nova escreve nos campos antigos.
4. Um teste de dupla leitura compara modelo novo × campos legados; divergência é incidente, não “diferença esperada”.
5. O inventário lista os 11 pontos e classifica cada um: mantém, adapta, remove.

### 3.4. Relatórios históricos precisam de fotografia

Concordo com o Claude: matrículas mensais, indicadores e Histórico Escolar não podem ler o “estado atual” quando o ano virou.

**Regra:** todo relatório histórico deve ser calculado com:

- o ano letivo correto;
- as datas de vigência da matrícula/turma;
- a versão do instrumento;
- a fotografia do calendário e das regras da época.

Se o relatório for recalculado depois, precisa usar a mesma fotografia; se a regra mudou, o relatório mostra a versão original e, se necessário, uma nota de reapresentação. Nunca sobrescrever o número de 2026 com o estado de 2027.

### 3.5. Auditoria de segurança: verificar exposição efetiva

Concordo com Claude e ChatGPT: RLS `USING(true)` e RLS desligada são fatos a reverificar; não são, por si sós, prova de exploração. A auditoria deve olhar o conjunto:

- `pg_class.relrowsecurity` e `pg_policies`;
- grants para `anon`/`authenticated`;
- funções `SECURITY DEFINER`, `search_path`, `proacl`;
- views que ignoram RLS;
- políticas de Storage;
- chave anon no frontend;
- chamadas RPC sem sessão;
- logs e URLs com dados.

**Regra:** as tabelas novas de Aprendizagem/Matrícula/Telegram nascem com RLS ligada e acesso por RPC/função autenticada. A correção das tabelas antigas é um projeto separado, com backup e teste; não deve ser “colada” na Fase 1.

### 3.6. Planilhas: contrato de equivalência, não importação

Concordo com o método do Claude e acrescento:

- **Matriz de rastreabilidade** com origem, destino, fórmula/regra, tipo, domínio, arredondamento, caso-limite, teste, responsável e status.
- **Conjunto dourado** com casos normais, limites e erros; saída esperada gerada no Excel oficial, não no sistema.
- **Versão do instrumento** por avaliação; regra nova não recalcula 2026.
- **Categorias pedagógicas** com definição semântica e exemplos; não inferir categoria a partir de número se a decisão é humana.
- **Zero ≠ não avaliado ≠ ausente**; traço, vazio e “N/A” têm significados distintos.
- **Intervenções manuais** no Excel precisam ser tratadas como exceção documentada, não como erro a corrigir.
- **Dupla execução** por um ciclo; divergência vai para a gestão como pergunta pedagógica.
- **Aceite assinado** pela equipe pedagógica; o sistema precisa ser reconhecido pela escola, não apenas “numericamente correto”.

---

## 4. Riscos adicionais que eu não vi na Mesa

1. **Resolução de identidade.** O “aluno permanente” pode ser criado errado: dois alunos para a mesma pessoa, ou duas pessoas fundidas. RA, nome e data de nascimento não bastam. Precisa de identificadores com vigência, matching conservador e revisão humana.
2. **Versionamento dos instrumentos.** As três planilhas podem mudar entre 2026 e 2027. Sem versão por avaliação, os resultados históricos deixam de ser reproduzíveis.
3. **Histórico de professor e turma.** Uma turma com um único professor perde substituições, licenças e remanejamentos. `Docencia` precisa de início/fim.
4. **Calendário e períodos.** Avaliações e frequências dependem do calendário; se o calendário mudar, o cálculo histórico não pode mudar junto. Versionar o calendário.
5. **Relatórios históricos congelados.** Além das contagens, os relatórios impressos/PDF precisam de snapshot ou versão; um PDF de 2026 não pode ser regenerado com dados de 2027.
6. **Adoção e dupla digitação.** Se a tela for mais lenta que o Excel, o professor mantém os dois; o sistema e a planilha divergem. Precisa de importação/exportação, offline/rascunho e teste de usabilidade.
7. **Uso ético dos dados de avaliação.** Hipóteses de escrita e resultados individuais podem virar ranking ou estigma. Precisa de finalidade, acesso por papel, agregação e auditoria.
8. **Seletor de ano.** Em 01/01/2027, o padrão pode abrir 2027 e esconder 2026; precisa de ano padrão explícito, aviso de “histórico” e bloqueio de escrita em ano fechado.

---

## 5. Divergências remanescentes

| Ponto | Claude | DeepSeek | Recomendação |
|---|---|---|---|
| Unidade da mudança de turma | Várias linhas de matrícula por ano | `Matricula` + `MatriculaTurma` | B2; se usar uma tabela, exigir `tipo` e `matricula_pai` |
| Cópia de 2026 | Cópia fiel | Snapshot imutável, não cópia viva | Fonte oficial única + snapshot para auditoria |
| Ordem pós-Aprendizagem | Matrícula antes de portfólio | Portfólio antes de matrícula, salvo necessidade | Decidir por risco jurídico e valor pedagógico |
| Segurança | Reverificar RLS | Auditar exposição efetiva completa | Auditoria de leitura antes da Fase 1 |

---

## 6. Classificação de evidências

| Item | Classificação | Observação |
|---|---|---|
| Fase 0 da virada 2027 (PR #602) | **Implementado** | Trava de importação e teste `test:ano-letivo`; não refazer |
| Separação por ano em `Turma`/`Aluno` | **Não implementado** | Depende de inventário e migração |
| WhatsApp manual com fila | **Implementado** | Um responsável por vez; sem envio automático |
| Bot do Telegram | **Criado, não programado** | Token a revogar; username a confirmar |
| Aprendizagem | **Não implementado** | Depende das três planilhas e do modelo de ano |
| Vida Escolar 360 | **Não verificado** | Pode conter protótipos, `localStorage` e dados simulados |
| B2 | **Proposto** | Modelo para estudo, não aprovado |
| Segurança/RLS | **Parcialmente verificada** | Achados do RASTRO; auditoria efetiva pendente |
| Portal das Famílias | **Proposto** | Depende de regras da Secretaria, identidade e LGPD |

---

## 7. Consenso possível

1. Nada será implementado nesta rodada.
2. B é a direção; **B2** é o modelo preferencial para estudo.
3. Aprovar B2 como hipótese, não como migração.
4. Aprendizagem é o primeiro produto; a auditoria das planilhas começa já.
5. A trava de 2027 permanece até a prova; a migração pode ser testada com ela ativa.
6. Segurança/RLS é requisito da Fase 1; auditar exposição efetiva.
7. Telegram é spike genérico, sem vínculo com aluno, após token/LGPD.
8. Matrícula digital fica em descoberta; portfólio vem depois de Aprendizagem.
9. Análise em paralelo; schema e migração N3 seriais; uma migração por vez.
10. A matriz de rastreabilidade com teste é artefato obrigatório de aceite.

---

## 8. Decisões do Mediador

1. Confirmar B2 como direção e autorizar o desenho, não a migração.
2. Confirmar se o `schema.prisma` lido é o real ou o código morto.
3. Confirmar revogação do token do Telegram e username oficial.
4. Enviar as três planilhas.
5. Autorizar inventário de leitura do Supabase.
6. Autorizar auditoria de exposição efetiva.
7. Autorizar auditoria do Vida Escolar 360.
8. Definir se portfólio ou matrícula vem depois de Aprendizagem.
9. Fixar uma migração N3 por vez e um responsável pelo schema.
10. Aprovar a matriz de rastreabilidade como contrato de aceite.

---

## 9. Fecho

O Claude trouxe o risco mais importante da transição: **duas fontes de verdade e relatórios históricos que leem o estado atual**. Concordo com a correção dele sobre a trava. A convergência é suficiente para o Mediador decidir a direção B2, mas não a migração.

Nada de código, banco ou produção nesta rodada. Este parecer é para confronto e consenso; a decisão é do Mediador.
