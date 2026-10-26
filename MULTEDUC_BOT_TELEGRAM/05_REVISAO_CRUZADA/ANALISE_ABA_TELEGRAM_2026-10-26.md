# ANÁLISE — PÁGINA “TELEGRAM (AVISOS)” DO MULTEDUC

**Data:** 26/10/2026  
**Analista:** DeepSeek  
**Escopo:** código e documentação da página `/telegram-avisos` no `origin/main` do repositório `diario_classe` (commit `234b0a2`), confrontados com o RASTRO de 25/10/2026.  
**Limite:** não abri a tela autenticada, não acessei o Supabase em produção e não executei os testes. As afirmações de produção são **relatos do RASTRO**, não verificação independente minha. Nada foi alterado.

---

## 1. O que é

Não é uma aba principal do aplicativo. É uma **página** em:

- rota: `/telegram-avisos`
- menu: **Documentos → Telegram (avisos)**
- permissão: `telegram_avisos`
- título: **✈️ Telegram da escola (piloto interno)**

A página tem três abas internas:

1. **📢 Avisos** — criar rascunho, aprovar, cancelar e “Enviar agora”.
2. **📅 Tem aula? (calendário)** — registrar/remover dias com alteração.
3. **❓ Perguntas frequentes** — criar/editar rascunho, aprovar, despublicar e remover.

Evidências: `frontend/src/main.tsx:90` e `:520`; `frontend/src/AuthContext.tsx:78`; `frontend/src/pages/TelegramAvisos.tsx:366-383`.

---

## 2. Estado atual

| Item | Estado | Evidência |
|---|---|---|
| Página publicada em `multeduc.com.br/telegram-avisos` | **Sim, segundo o RASTRO** | `RASTRO.md:88` |
| Item no menu Documentos | **Sim** | `main.tsx:90` |
| Permissão `telegram_avisos` | **Sim** | `AuthContext.tsx:78`; `main.tsx:520` |
| Aba Avisos | **Implementada** | `TelegramAvisos.tsx:44-186` |
| Aba Calendário | **Implementada** | `TelegramAvisos.tsx:203-268` |
| Aba Perguntas frequentes | **Implementada** | `TelegramAvisos.tsx:273-363` |
| Bot com `/start`, `/menu`, `/avisos`, `/aula`, `/perguntas`, `/ajuda`, `/privacidade`, `/parar` | **Implementado; testado pelo dono** | `logica.ts:295-361`; `RASTRO.md:90` |
| Webhook com `secret_token` | **Ativo, segundo o RASTRO** | `logica.ts:449-456`; `RASTRO.md:89` |
| Migrations 1–3 aplicadas | **Sim, segundo o RASTRO** | `RASTRO.md:88-94` |
| Função `telegram-bot` v2 publicada | **Sim, segundo o RASTRO** | `RASTRO.md:94` |
| Envio de aviso da tela até o Telegram | **NÃO verificado** | `RASTRO.md:92` |
| Calendário com dias cadastrados | **NÃO verificado** | `RASTRO.md:92` |
| FAQ com pergunta aprovada | **NÃO verificado** | `RASTRO.md:92` |
| Teste com 2 outras pessoas | **NÃO verificado** | `RASTRO.md:92` |
| Permissão restrita a 3 pessoas | **NÃO verificado** | `RASTRO.md:92` |
| Famílias convidadas | **Não** | `RASTRO.md:94` |
| Parecer jurídico/LGPD | **Pendente** | `RASTRO.md:93` |
| Anexos (#619) | **Fora desta fase** | `RASTRO.md:93` |
| Vínculo com aluno, notas, frequência, IA | **Não existe** | `docs/telegram-piloto-interno.md:58-61` |

**Resumo:** a tela está publicada e o bot responde. O núcleo operacional — criar aviso, outra pessoa aprovar, clicar em “Enviar agora” e o aviso chegar ao Telegram — **ainda não foi provado em produção**. O RASTRO confirma apenas testes de conversa e de tela com uma pessoa (o dono).

---

## 3. O que a tela faz

### 3.1 Avisos

- Cria rascunho com título de até 120 caracteres e corpo de até 3.500.
- Mostra status: rascunho, aprovado, concluído ou cancelado.
- Mostra contagem de enviados, pendentes e falhas.
- Mostra resumo de contatos: ativos, parados e bloqueados.
- “Aprovar” enfileira um envio para cada contato ativo; **quem redigiu não pode aprovar**, exceto administrador.
- “Enviar agora” chama a Edge Function `telegram-bot` com a ação `enviar_pendentes`.
- “Cancelar” descarta envios pendentes.

Evidências: `TelegramAvisos.tsx:44-186`; SQL `20261025180000_telegram_piloto_interno.sql:120-181`; `logica.ts:366-407`.

### 3.2 Calendário “Tem aula?”

- Registra uma data com `sem_aula`, `horario_especial` ou `aula_normal`.
- Exige explicação para `sem_aula` e `horario_especial`.
- Mostra os próximos 120 dias cadastrados.
- O bot responde “hoje”, “amanhã” ou “próximos 14 dias”.
- Dia sem registro devolve **“não há aviso de alteração”**, sem afirmar que há aula.
- Usa o fuso `America/Sao_Paulo` no bot.

Evidências: `TelegramAvisos.tsx:203-268`; `logica.ts:179-249`; SQL `20261025180000...:375-431`.

### 3.3 Perguntas frequentes

- Cria rascunho; não publica direto.
- Outra pessoa aprova; administrador pode aprovar a própria.
- Editar o texto de uma pergunta publicada a tira do bot até nova aprovação.
- Despublicar é livre; remover também.
- O bot mostra até 10 perguntas ativas.

Evidências: `TelegramAvisos.tsx:273-363`; SQL `20261025200000_telegram_faq_aprovacao.sql:20-101`.

---

## 4. Pontos fortes

1. **RLS e `revoke` nas seis tabelas.** `anon` e `authenticated` não leem nem escrevem direto; o acesso é por funções. (`20261025180000...:80-87`)
2. **Separação de papéis:** funções do bot são `service_role`; funções da equipe validam sessão + `telegram_avisos`. (`20261025180000...:90-117`, `:516-563`)
3. **Webhook com segredo e comparação em tempo constante.** (`logica.ts:107-115`, `:449-456`)
4. **Idempotência de entrada por `update_id`.** (`logica.ts:460-468`; tabela `TelegramUpdate`)
5. **Sem log de `chat_id`, texto de usuário ou token.** (`logica.ts:8`, `:43-64`, `:272`, `:405`)
6. **Limite de corpo e CORS restrito.** (`logica.ts:18`, `:129-139`, `:415-425`)
7. **Fila com reserva e `skip locked`**, evitando envio duplo por duas chamadas simultâneas. (`20261025180000...:305-334`)
8. **Calendário não afirma aula quando não há registro.** (`logica.ts:218-229`)
9. **Página com aviso explícito de piloto e proibição de dados de aluno.** (`TelegramAvisos.tsx:129-133`)

---

## 5. Achados e riscos

### 5.1 O fluxo principal ainda não foi provado

O RASTRO é explícito: **“Enviar agora” não verificado**. Foram testados `/start`, aceite, `/parar`, `/aula` sem registros, FAQ vazio, avisos recentes, ajuda e privacidade. Falta o caminho:

> criar aviso → outra pessoa aprovar → “Enviar agora” → mensagem chegar ao Telegram → `/avisos` mostrar.

Esse é o teste que decide se o piloto funciona de ponta a ponta. Sem ele, a tela está publicada, mas o produto não está provado.

### 5.2 A proibição de dados de aluno é só humana

A tela avisa “nunca escreva nome de aluno, turma, nota, frequência, saúde ou documentos” (`TelegramAvisos.tsx:130-131`), mas **não há validação técnica** no frontend nem no SQL. O backend só verifica tamanho e vazio. Um operador pode colar um nome, CPF ou telefone sem bloqueio.

Isso é aceitável no piloto interno com 3 pessoas, mas é um risco relevante antes de famílias. Para a Fase 2, precisa de trava determinística: regex de CPF/telefone, bloqueio de nomes da lista de alunos e/ou revisão obrigatória por segunda pessoa com checklist.

### 5.3 O texto de privacidade não descreve tudo o que é guardado

O bot diz que guarda “apenas o identificador técnico desta conversa e a data do seu aceite” (`logica.ts:47-52`). A tabela `TelegramContato` guarda também `status`, `consentimento_versao`, `parou_em`, `criado_em` e `atualizado_em` (`20261025180000...:14-24`). Não é dado sensível, mas a frase está incompleta. Antes de famílias, o texto deve refletir o conjunto real e o prazo de retenção.

### 5.4 Retenção e eliminação

- `telegram_limpar_updates()` existe, mas **não há agendamento** (`pg_cron` ou similar) no repositório. A tabela `TelegramUpdate` cresce indefinidamente.
- Não há rotina de retenção/eliminação para `TelegramContato`, `TelegramAviso` ou `TelegramEnvio`.
- `/parar` marca status, mas não apaga dados.
- Não há fluxo de direito de eliminação.

Isso precisa ser resolvido antes da Fase 2. No piloto interno, o volume é baixo; ainda assim, a ausência de política é uma lacuna de LGPD.

### 5.5 Limite de taxa e idempotência de saída

- O handler não tem limitador de taxa para a ação autenticada `enviar_pendentes`; só o limite de corpo e a pausa de 60 ms entre envios (`logica.ts:18-21`, `:366-407`). Um usuário com permissão pode chamar repetidamente; a fila limita o dano, mas não impede abuso.
- `TelegramEnvio` não guarda `message_id` do Telegram. Se a função enviar a mensagem e cair antes de gravar `telegram_envio_resultado`, a reserva expira em 5 minutos e o envio pode ser repetido. É um risco de duplicidade, não uma certeza.
- `telegram_aviso_cancelar` marca como falha apenas envios `pendente`; um envio já `enviando` pode continuar e chegar depois do cancelamento (`20261025180000...:166-180`).

### 5.6 Data local × fuso da escola

A tela calcula “hoje” com `new Date()` no fuso do navegador (`TelegramAvisos.tsx:192-196`). O bot usa `America/Sao_Paulo` (`logica.ts:180-186`). Em um dispositivo com outro fuso, a lista de 120 dias pode começar um dia antes/depois. Baixo risco no uso escolar normal, mas é uma inconsistência.

### 5.7 Documentação operacional desatualizada

`docs/telegram-piloto-interno.md:3` ainda diz “PR em rascunho; só a 1ª migration foi aplicada”. O RASTRO mais recente diz #618 mesclado, migrations 1–3 aplicadas, função v2 e webhook ativo (`RASTRO.md:88-94`). Quem for operar o piloto pode se confundir. O RASTRO é a fonte mais recente; o doc precisa ser atualizado antes de outra pessoa assumir.

### 5.8 Mensagem duplicada

O RASTRO registra que “Pronto!” e a escolha de dia apareceram em dobro após toques em botão; hipótese de toque duplo, não confirmada (`RASTRO.md:91`). A idempotência por `update_id` impede reprocessar o **mesmo** update, mas dois toques geram dois updates. Se incomodar, a solução é desabilitar o botão após o primeiro toque ou ignorar callback repetido em janela curta. Não é bloqueador.

---

## 6. O que eu faria antes de chamar de “piloto concluído”

1. Rodar o roteiro de 9 passos com 2–3 pessoas, especialmente o “Enviar agora”.
2. Cadastrar um dia real no calendário e conferir `/aula` nos três recortes.
3. Criar uma pergunta, aprovar com outra pessoa e conferir `/perguntas`.
4. Testar `/parar`, novo `/start`, bloqueio do bot e reenvio de “Enviar agora” sem duplicar.
5. Conferir que só as 3 pessoas têm a permissão `telegram_avisos`.
6. Atualizar o `docs/telegram-piloto-interno.md` para o estado real.
7. Definir retenção e agendar `telegram_limpar_updates`.
8. Antes de famílias: trava de PII, texto de privacidade completo, rate limit, correção da corrida do cancelamento e parecer jurídico/LGPD.

---

## 7. Situação do seu checkout local

O `C:\diario_classe` está na branch `main` no commit `fb14cb9`, **720 commits atrás** de `origin/main` (`234b0a2`). A pasta `frontend/src/pages/TelegramAvisos.tsx` **não existe no seu working tree local**. Se você rodar o app localmente sem atualizar, a página não aparece.

Não atualizei sua branch porque há arquivos modificados e não commitados (`BOLSA_FAMILIA.pdf`, `CENTRAL/`, `REVISAO/`, etc.). Para ver a versão de produção sem mexer no que está aberto, o caminho seguro é um worktree separado ou um clone temporário.

---

## 8. Conclusão

**A página existe, está publicada e tem uma base de segurança boa para piloto interno.** O bot responde e a tela tem as três abas. O que falta é a prova do fluxo principal de aviso e a operação com mais de uma pessoa.

**Não convidaria famílias ainda.** O piloto interno pode continuar, mas antes da Fase 2 precisam ser resolvidos: PII, retenção/eliminação, texto de privacidade, rate limit, corrida do cancelamento e parecer jurídico/LGPD.

Nada foi implementado ou alterado nesta análise.
