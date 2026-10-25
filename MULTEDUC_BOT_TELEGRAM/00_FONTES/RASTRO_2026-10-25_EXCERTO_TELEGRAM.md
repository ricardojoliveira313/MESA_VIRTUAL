# RASTRO — excerto das entradas relevantes ao caso Telegram/MultEduc

**Tipo:** excerto documental do RASTRO fornecido pelo Mediador em 25/10/2026.  
**Observação:** o RASTRO completo permanece no repositório do MultEduc, fora de `C:\processo_inventario`; este arquivo preserva apenas as entradas que fundamentam o caso.  
**Uso:** fonte primária do caso, com citação por data e título da entrada. O RASTRO não tem paginação; a localização é feita pelo título e pela data.

---

## Mantra de implementação (informado no topo do RASTRO)

> **MANTRA DE IMPLEMENTAÇÃO -----> QC = QUEBRA-CABEÇA - VM = VISÃO MACRO = PE = PLANEJAMENTO ESTRATÉGICO = PT = PLANEJAMENTO TÁTICO - PO = PLANEJAMENTO OPERACIONAL = IMF = INICIO - MEIO - FIM**

**Leitura:** o método exigido pelo Mediador combina QC (quebra-cabeça), VM (visão macro), PE (planejamento estratégico), PT (planejamento tático), PO (planejamento operacional) e IMF (início, meio e fim). O parecer deve explicitar essas camadas, não apenas listar tarefas.

---

## 2026-10-25 — Mesa Virtual: bot Telegram criado; planejar implementação e piloto interno

**Quem:** ChatGPT, a pedido do responsável pelo MultEduc.  
**Onde:** pauta da Mesa Virtual — Comunicação Digital com as Famílias.

Trechos literais relevantes:

> “bot `@EMEIEFLuizGonzagaAvisos_bot` criado no BotFather, acessível em https://t.me/EMEIEFLuizGonzagaAvisos_bot.”

> “**Nenhuma programação, webhook, integração, resposta automática ou envio de avisos foi implementado até este registro.** A criação não equivale a funcionamento.”

> “planejar, debater com ChatGPT, Claude, DeepSeek e equipe escolar e posteriormente implementar, sob aprovação, um canal de avisos e atendimento orientado aos responsáveis, articulado com WhatsApp e MultEduc/PWA, sem substituir atendimento humano.”

> “**Primeiro piloto restrito:** testes com o responsável pelo projeto e colegas da escola, usando contas e dados fictícios ou autorizados; validar /start, boas-vindas, menu, consulta de comunicados de teste, calendário, orientações gerais, tratamento de erros, mensagens fora de horário, encaminhamento à secretaria e fluxo de publicação de avisos por pessoal autorizado. Nenhum disparo a famílias reais nesta fase.”

> “**Menu candidato para deliberação:** Avisos da Escola; Calendário Escolar; Matrículas e Documentos; Atendimento às Famílias; Ajuda/Privacidade.”

> “**Arquitetura a discutir:** Telegram Bot API e webhook HTTPS autenticado/validado, serviço backend e logs mínimos, integração controlada ao MultEduc, gestão de permissões para publicar avisos, ambientes de teste e produção separados, idempotência de atualizações, monitoramento, recuperação de falhas e proteção do token do BotFather (nunca em GitHub/RASTRO).”

> “**Regras de segurança e operação:** o responsável normalmente precisa iniciar o bot antes de receber mensagens privadas; não presumir envio irrestrito nem criar grupos com exposição de famílias; consentimento/base legal e transparência para comunicados, opt-out/bloqueio, LGPD, proteção reforçada dos dados de crianças, minimização de dados, restrição de acesso, retenção, auditoria e validação de identidade antes de consultas individualizadas. Evitar notas, frequência, documentos e dados pessoais no piloto inicial.”

> “**Encaminhamento à Mesa Virtual:** Claude e DeepSeek devem elaborar pareceres independentes sobre escopo, arquitetura, segurança, testes, riscos, custos, integração WhatsApp/MultEduc, responsabilidades de publicação e cronograma. Produzir matriz de decisão e critérios de aceite antes de programar ou publicar.”

> “**Estado em produção:** apenas o bot cadastrado no Telegram; funcionalidade operacional e integração com MultEduc inexistentes segundo o responsável.”

---

## 2026-10-25 — Definição da Mesa Virtual de Discussão e Implementação

Trechos literais relevantes:

> “a Mesa Virtual é o método de discussão técnica e tomada de decisão **antes de implementar** alterações no MultEduc. **Não** é o bot de atendimento às famílias nem um módulo de interface.”

> “Ricardo coordena, define prioridades e dá a aprovação final; ChatGPT formula e revisa propostas, arquitetura e segurança; Claude avalia a base de código, a viabilidade e a execução técnica; DeepSeek faz crítica independente, identifica riscos e alternativas.”

> “(1) descrever problema, objetivos e restrições; (2) elaborar parecer inicial; (3) submeter aos demais integrantes; (4) confrontar análises, divergências, riscos e custos; (5) registrar consenso técnico, decisões e pendências; (6) obter aprovação expressa de Ricardo; (7) implementar apenas o escopo aprovado, testar, auditar e documentar; (8) registrar o estado real de publicação.”

> “não confundir proposta com implementação; não alegar consenso sem ouvir os participantes; preservar o que já funciona; verificar dados e código antes de concluir; proteger informações pessoais; manter histórico de decisões e divergências; não publicar mudanças em produção sem autorização exigida pelo projeto.”

**Pauta inicial registrada:** integração gradual de WhatsApp e Telegram ao MultEduc, com possibilidade futura de atendimento automatizado aos responsáveis; primeiro discutir arquitetura, vinculação segura, permissões, LGPD, limites da API, custos, operação e plano de implantação. **Nenhuma implementação de Telegram foi realizada por esta entrada.**

---

## 2026-10-25 — Telegram: bot de avisos criado no BotFather (passo 0)

Trechos literais relevantes:

> “o **token** fica guardado só em local seguro do responsável (gerenciador de senhas ou nota privada) e depois como **segredo do Supabase**; nunca em chat, print, repositório ou RASTRO.”

> “**Incidente de segurança (token fora de lugar):** durante a criação, um print da conversa com o BotFather mostrou na tela o token de um bot **anterior**. Todo token que apareceu em print deve ser tratado como exposto: `/revoke` no BotFather para gerar outro e não reutilizar o antigo. **Não foi confirmado** se a revogação já foi feita.”

> “**Não verificado:** o username final do bot foi dito como ‘consegui’, sem ser repassado; confirmar o username exato.”

> “Pendências (nada disso foi feito): `/setdescription`, `/setuserpic` e `/setjoingroups` → Disable; plano **N3** com aprovação do dono (vínculo família↔criança por código único, webhook das respostas, consentimento, opção de parar de receber, LGPD); definir o ícone de cada botão de envio.”

> “**Estado em produção:** somente registro no RASTRO.md. Nenhum código, banco, edge function, segredo ou fluxo foi criado ou publicado; o bot existe no Telegram, mas **não está conectado ao sistema**.”

---

## 2026-10-25 — Telegram: procedimento de criação do bot no BotFather (passo 0)

Trechos literais relevantes:

> “O token é a senha do bot: nunca colar em chat, repositório, RASTRO, print ou documento.”

> “`/setjoingroups` → **Disable** (ninguém adiciona o bot em grupos).”

> “plano **N3** para aprovação do dono antes de qualquer código: tabelas de vínculo família↔criança por código único, edge function que recebe as respostas dos botões (webhook), consentimento (Iniciar) e opção de parar de receber, mensagens no mesmo nível mínimo de dados do WhatsApp (sem deficiência nem dado sensível), e o WhatsApp mantido para quem não aderir.”

---

## 2026-10-25 — Proposta: avisos às famílias também por Telegram (somente pauta)

Trechos literais relevantes:

> “manter o **WhatsApp** (já implementado, fila de um responsável por vez) e **acrescentar o Telegram como segunda opção de envio**, cada uma com seu botão e ícone, adotado **aos poucos**.”

> “a API de bots do Telegram é oficial e gratuita, aceita botões de resposta e retorna a resposta da família ao sistema, sem a trava de envio manual e sem o risco de bloqueio por disparo que o WhatsApp impõe a números comuns.”

> “o bot **não pode iniciar conversa**: a família precisa abrir o bot por um link/QR da escola e aceitar (consentimento), então a adesão é o ponto crítico e o WhatsApp continua necessário para quem não aderir.”

> “conversas de bot **não têm criptografia ponta a ponta**, então a mensagem deve ficar no mesmo nível mínimo de hoje (sem deficiência, diagnóstico ou dados sensíveis).”

> “a ligação família↔criança deve usar código único por família, nunca só o número de telefone.”

> “a adesão no Telegram entre as famílias da escola é desconhecida: medir antes de investir.”

> “Classificação: **N3** (dados pessoais de crianças e responsáveis, backend novo/edge function com segredo do bot, produto novo). Exige plano, QC/Visão Macro, aprovação do dono e checagem de LGPD antes de implementar.”

---

## 2026-10-25 — BF — Frequência: envio por WhatsApp “em lote”

Trecho relevante para o desenho de integração:

> “Abre uma fila (mesmo padrão da fila das creches): um responsável por vez, a pessoa aperta ‘Abrir WhatsApp’ e depois **enviar** no WhatsApp. Nada é enviado sozinho; sem temporizador.”

**Uso no caso:** o Telegram não deve ser tratado como substituto automático do WhatsApp; a lógica de fila, registro de contato e confirmação humana é precedente útil.

---

## Limites deste excerto

1. Não contém o RASTRO completo; contém apenas as entradas que o caso cita.
2. Não contém o código do MultEduc, o schema do banco, as credenciais, o token ou os documentos internos do projeto.
3. As afirmações sobre a API do Telegram são as registradas no RASTRO; a verificação na documentação oficial do Telegram não foi feita nesta sessão e deve ser tratada como **a confirmar**.
4. O token exposto não é reproduzido aqui, por segurança.
