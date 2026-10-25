# REVISÃO CRUZADA — Claude sobre o parecer do DeepSeek

**Caso:** MULTEDUC_BOT_TELEGRAM  
**Recebido em:** 25/10/2026  
**Natureza:** revisão cruzada da rodada 1; não é o parecer formal da rodada 1 do Claude  
**Transcrição:** fiel ao texto enviado pelo Mediador

---

REVISÃO CRUZADA — MULTEDUC_BOT_TELEGRAM — Claude sobre o parecer do DeepSeek

Data: 25/10/2026

Escopo: li 00_ABERTURA.md e PARECER_DEEPSEEK.md. O parecer do ChatGPT para este caso não foi enviado a mim, então não o reviso. O meu parecer da rodada 1 foi escrito antes de eu ler o do DeepSeek. Aqui só indico o que mudei depois de lê-lo.

1. Concordâncias

| # | Ponto | Comentário |
|---|---|---|
| C1 | “Piloto interno sim, famílias ainda não” | É o mesmo resultado a que cheguei, por caminho separado. |
| C2 | Token revogado como pré-condição | Idêntico ao meu F5. |
| C3 | Sem vínculo família↔criança no piloto interno (§2.8) | Coincide com o meu F9. |
| C4 | Menu mínimo: Avisos, Calendário, Ajuda/Privacidade | Coincide com o meu F8. |
| C5 | Opção A (Supabase) para começar, no-code descartado | Concordo, e a evidência abaixo reforça. |
| C6 | Fases 0 a 3 com portões | Adoto a estrutura dele. Ela é mais clara que a minha. |
| C7 | Critérios de aceite 1 a 13 | Quase todos servem. Proponho ajustes em §2. |
| C8 | Telegram complementar, não substituto do WhatsApp nem do atendimento humano | Concordo. Risco de exclusão digital é bem apontado. |

2. Divergências e correções

**D1. Jurídico como portão da Fase 0 (§2.13).** O quadro de fases põe “Mediador + jurídico aprovam” ao fim da Fase 0, antes do piloto interno. Mas o próprio §2.10 diz que o piloto interno com dados fictícios “pode começar depois da Fase 0”, e o §2.14 só exige parecer jurídico completo nos critérios 3 (Fase 0).

Minha posição (opinião): no piloto com dados fictícios bastam o token revogado, o mapa de dados e a ciência por escrito dos testadores. O parecer jurídico completo deve ser portão da Fase 2, a que envolve famílias reais.

Exigir o parecer completo antes da Fase 1 atrasa só para proteger dados que não existem ainda. Se o Mediador preferir manter o rigor dele, a escolha é dele.

**D2. Tabelas telegram_* em snake_case (§2.8).** Pelo RASTRO, as tabelas recentes do projeto usam PascalCase (ContatoResponsavel, AlunoContato, ContatoRegistro) e funções em snake_case. Sugiro seguir a convenção. Além disso, as tabelas novas precisam entrar na cobertura do Rastro de usuários. O RASTRO diz que a cobertura macro está em 47 tabelas e a regra é cobrir todas.

**D3. chat_id “criptografado ou tokenizado” (§2.8).** Concordo com proteger. Mas criptografia de coluna no Postgres exige gestão de chave, e o envio precisa do valor em claro. O padrão já usado no projeto é RLS ligado, revoke all para anon e authenticated, acesso só por RPC com sessão (doc MESA-VIRTUAL-AGENTE-TELEGRAM.md §3). Sugiro começar por esse padrão e só criptografar se houver um plano real de chave. Também falta o tipo: o doc §5 item 4 diz que o chat_id pode ter mais de 32 bits, então o campo deve ser bigint.

**D4. RACI: “Claude é R pela publicação em produção” (§2.16).** Discordo. O RASTRO mostra, em 02/10 e 06/10, que agentes tiveram a aplicação de SQL e deploy de função bloqueados por permissão, e quem publicou foi o dono ou a equipe com acesso ao Supabase. Publicação, segredos e setWebhook devem ficar com o Ricardo ou com quem ele indicar. Eu preparo e testo; não publico.

**D5. Fase 2 e o vínculo.** O §2.8 diz que o vínculo “entra na Fase 2”, mas o §2.13 descreve a Fase 2 como avisos gerais e respostas por botão, o que não precisa de vínculo. Há uma inconsistência. Proponho declarar que o vínculo é uma fase própria, depois da Fase 2, e dependente da virada de ano letivo (o vínculo deve apontar para a matrícula do ano, não para o cadastro atual).

**D6. Separar teste e produção “por projeto/segredo” (§2.6 e §2.7).** O RASTRO fala de um só projeto Supabase de produção. Um segundo projeto ou branch pode ter custo e plano desconhecidos. Isso fica como decisão do Mediador e não deve ser tratado como dado.

3. Lacunas e evidência nova

**Fonte nova.** `docs/MESA-VIRTUAL-AGENTE-TELEGRAM.md` (já no repositório) não está no índice. Ele traz fatos de API que o dono colou da documentação oficial em 19/10: webhook e getUpdates são exclusivos; sem redirecionamento; secret_token; my_chat_member quando o bot é bloqueado; deep link `?start=` de 1 a 64 caracteres; Paid Broadcasts deve ficar desativado. O DeepSeek marcou os limites da API como “a confirmar”, e esse documento reduz essa lacuna, mas não a elimina: eu também não conferi a página oficial.

**Precedente de webhook público.** A pasta `supabase/functions` tem 13 funções. Várias estão com `verify_jwt = false` no `config.toml` (por exemplo `censo-oficial-bridge`, `censo-submit-guard`, `enviar-aviso-demo`). Isso confirma que o padrão existe, mas nenhuma delas é um endpoint público de terceiros. O webhook do Telegram será o primeiro, então precisa de validação de segredo, limite de taxa e resposta rápida.

**Segredos.** Criar segredo e publicar função são passos manuais (o merge não os faz). Isso entra no runbook da Fase 1.

**Reversão.** Falta no parecer uma linha de reversão pronta: `deleteWebhook`, revogar o token, revogar a execução das funções e preservar as tabelas.

**Capacidade humana.** O cronograma de 8 a 12 semanas não considera que as mesmas pessoas aprovam a virada de ano e a Aprendizagem. Se Telegram e virada correrem juntos, as migrações devem ser aplicadas em sequência, mesmo com tabelas independentes.

**Afirmação sem fonte.** “O Telegram processa dados em servidores próprios” (§2.10) é razoável, mas não está citada. Marcar como hipótese.

4. O que mudei no meu parecer depois de ler o dele

- Adoto a estrutura de Fases 0 a 3.
- Aceito que a medição de adesão fique na Fase 2, e não antes. Mantenho como opção para o Mediador uma enquete de interesse sem o bot.
- Retiro a ideia de que Telegram e virada não possam andar juntos. Podem, com as migrações em sequência.
- Mantenho a divergência D1 (jurídico completo só antes da Fase 2).

5. Matriz resumida para a consolidação

| # | Ponto | DeepSeek | Claude | Recomendação |
|---|---|---|---|---|
| 1 | Piloto interno antes de famílias | Sim | Sim | Consenso |
| 2 | Token revogado antes de tudo | Sim | Sim | Consenso |
| 3 | Vínculo no piloto | Não | Não | Consenso |
| 4 | Jurídico completo antes da Fase 1 | Sim | Não, antes da Fase 2 | Decisão do Mediador |
| 5 | Nomes de tabela | snake_case | PascalCase (convenção do repositório) | Seguir o repositório |
| 6 | Criptografia do chat_id | Sim | RLS e RPC primeiro | Decisão técnica |
| 7 | Quem publica em produção | Claude (R) | Ricardo ou equipe Supabase | Ajustar RACI |
| 8 | Vínculo em fase própria | Dentro da Fase 2 | Depois da Fase 2 | Resolver a inconsistência |

6. Pontos que continuam sendo decisão do Mediador

Os 12 pontos do DeepSeek (§4 dele) e os meus 7 permanecem. Acrescento apenas: aceitar ou não o jurídico completo só antes da Fase 2 (D1), e quem faz a publicação (D4).

7. Limites desta revisão

Li só os dois arquivos enviados e os fatos do repositório citados. Não vi o parecer do ChatGPT. Não conferi o Telegram nem o Supabase em produção. A conexão do Supabase falhou nesta sessão.

Posso gravar este parecer e o anterior como arquivos .md no repositório, se você quiser.
