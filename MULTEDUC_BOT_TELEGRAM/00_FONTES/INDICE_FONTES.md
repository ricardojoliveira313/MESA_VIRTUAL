# Índice de fontes — MULTEDUC_BOT_TELEGRAM

**Caso:** MULTEDUC_BOT_TELEGRAM  
**Abertura:** 25/10/2026  
**Mediador:** Ricardo  
**Estado:** EM_REVISÃO

| # | Fonte | Caminho | Tipo | Resumo objetivo | Status |
|---:|---|---|---|---|---|
| 1 | RASTRO — excerto Telegram/Mesa Virtual | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\00_FONTES\RASTRO_2026-10-25_EXCERTO_TELEGRAM.md` | Excerto documental de registro interno | Preserva as entradas de 25/10/2026 sobre a Mesa Virtual, a criação do bot, o procedimento no BotFather, a proposta de avisos por Telegram e o mantra QC/VM/PE/PT/PO/IMF | Lido integralmente |
| 2 | Protocolo da Mesa Virtual | `C:\MESA_VIRTUAL\MESA_VIRTUAL_PROTOCOLO.md` | Norma interna | Define papéis, ciclo, estados, regras de evidência, confidencialidade e formato de parecer | Lido integralmente |
| 3 | RASTRO completo do MultEduc | Repositório do MultEduc, fora de `C:\MESA_VIRTUAL`; não fornecido nesta abertura | Registro interno | Histórico completo do aplicativo; não foi disponibilizado como arquivo neste caso | **Ausente / a confirmar** |
| 4 | Código, banco e edge functions do MultEduc | Repositório do MultEduc; não fornecido nesta abertura | Código e schema | Necessários para avaliar viabilidade de webhook, permissões, RLS, logs e integração | **Ausente / a confirmar** |
| 5 | Documentação oficial do Telegram Bot API | Não fornecida nesta abertura | Documentação técnica externa | Necessária para confirmar limites, webhook, `secret_token`, botões, deep link, política de dados e termos | **Ausente / a confirmar** |
| 6 | Parecer jurídico/LGPD | Não fornecido nesta abertura | Análise jurídica | Necessário para base legal, consentimento de crianças/responsáveis, transferência internacional, retenção e direitos | **Ausente / a confirmar** |
| 7 | Métricas de adesão ao Telegram na comunidade escolar | Não fornecidas | Pesquisa de campo | O RASTRO registra que a adesão é desconhecida; medir antes de investir | **Ausente / a confirmar** |
| 8 | `docs/MESA-VIRTUAL-AGENTE-TELEGRAM.md` | Repositório do MultEduc; citado por Claude | Documento técnico interno | Fatos de API colados da documentação em 19/10: webhook e `getUpdates` exclusivos; sem redirecionamento; `secret_token`; `my_chat_member`; deep link `?start=` de 1 a 64 caracteres; Paid Broadcasts desativado | **Citado por Claude; não lido nesta sessão** |
| 9 | `supabase/functions` e `config.toml` | Repositório do MultEduc; citado por Claude | Código/configuração | 13 funções; algumas com `verify_jwt = false` (exemplos: `censo-oficial-bridge`, `censo-submit-guard`, `enviar-aviso-demo`); nenhum endpoint público de terceiros | **Citado por Claude; não verificado** |
| 10 | Revisão cruzada de Claude | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\05_REVISAO_CRUZADA\REVISAO_CRUZADA_CLAUDE.md` | Revisão cruzada | Concordâncias, divergências D1–D6, evidência nova, matriz resumida e limites | Depositada em 25/10/2026 |
| 11 | Resposta do DeepSeek à revisão de Claude | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\05_REVISAO_CRUZADA\RESPOSTA_DEEPSEEK_A_REVISAO_CLAUDE.md` | Revisão cruzada | Posição revisada sobre D1–D6, evidência nova, matriz provisória e decisões pedidas ao Mediador | Depositada em 25/10/2026 |
| 12 | Parecer do ChatGPT | `02_PARECER_CHATGPT/` | Parecer formal | Não depositado como arquivo nesta Mesa | **Ausente** |
| 13 | Parecer da rodada 1 do Claude | `04_PARECER_CLAUDE/` | Parecer formal | Citado na revisão (F5, F8, F9), mas não depositado como arquivo separado | **Ausente como arquivo** |
| 14 | Análise da página `/telegram-avisos` | `C:\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\05_REVISAO_CRUZADA\ANALISE_ABA_TELEGRAM_2026-10-26.md` | Análise técnica | Estado da tela, do bot, do calendário e das perguntas; achados e riscos | Depositada em 26/10/2026 |

## Observações de evidência

1. O RASTRO é a única fonte primária efetivamente disponível neste caso. Ele registra **fatos do projeto** e **propostas**, mas não substitui código, documentação oficial do Telegram ou parecer jurídico.
2. As afirmações do RASTRO sobre a API do Telegram (“oficial e gratuita”, botões de resposta, webhook) estão identificadas como **registro do RASTRO**, não como verificação independente nesta sessão.
3. O token do bot não é reproduzido em nenhuma fonte deste caso. O RASTRO registra que um token apareceu em print e que a revogação **não foi confirmada**.
4. O documento `MESA-VIRTUAL-AGENTE-TELEGRAM.md`, as 13 funções e o `verify_jwt = false` são **relatos de Claude**, não fatos verificados nesta sessão. Reduzem a lacuna da API, mas não a eliminam.
5. Não há dados pessoais de crianças, responsáveis ou servidores neste índice.
6. O parecer do ChatGPT e o parecer da rodada 1 do Claude precisam ser depositados antes da matriz de divergências final. A revisão cruzada de Claude não substitui seu parecer formal.
7. O `schema.prisma` pode ser código morto; o schema real do Supabase continua ausente. Nenhuma conclusão física de banco deve ser tratada como provada antes dessa leitura.
8. A análise da página Telegram foi feita sobre `origin/main` commit `234b0a2`. O `main` local em `C:\diario_classe` estava em `fb14cb9`, 720 commits atrás, e não continha `frontend/src/pages/TelegramAvisos.tsx`. As afirmações de produção são relatos do RASTRO; não houve acesso à tela autenticada nem ao Supabase.
