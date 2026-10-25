# PARECER — MULTEDUC_BOT_TELEGRAM — DEEPSEEK

**Data:** 25/10/2026  
**Analista:** DeepSeek (OpenCode)  
**Caso:** MULTEDUC_BOT_TELEGRAM  
**Pergunta central:** O bot `@EMEIEFLuizGonzagaAvisos_bot` deve ser implementado agora? Com qual escopo, arquitetura, segurança, piloto interno, critérios de aceite e cronograma, considerando a integração com WhatsApp/MultEduc e a LGPD?  
**Método:** QC → VM → PE → PT → PO → IMF

---

## 1. Resposta curta

**Sim, é tecnicamente viável iniciar um piloto interno de baixo custo; não, não é prudente implementar agora o canal para famílias.** O bot existe, mas não funciona: o RASTRO registra que “nenhuma programação, webhook, integração, resposta automática ou envio de avisos foi implementado” e que “a criação não equivale a funcionamento”. Antes de qualquer código, há bloqueios críticos: (1) o token apareceu em print e a revogação não foi confirmada; (2) o username final precisa ser confirmado; (3) não há base legal/LGPD definida para tratar dados de crianças e responsáveis; (4) não há vínculo família↔criança por código único; (5) não há política de conteúdo, retenção, auditoria e opt-out; (6) a adesão ao Telegram é desconhecida. A recomendação é **não publicar para famílias agora** e executar quatro fases com portões: **Fase 0** — segurança, governança e LGPD; **Fase 1** — piloto interno com dados fictícios; **Fase 2** — piloto controlado com famílias autorizadas; **Fase 3** — produção gradual, mantendo o WhatsApp. O Telegram deve entrar como **canal complementar de avisos gerais e menu determinístico**, não como substituto do atendimento humano, não como grupo de famílias e não como IA livre para casos individuais.

---

## 2. Fundamentos

### 2.1. O que está provado, o que é indício e o que é hipótese

**Fatos registrados no RASTRO (fonte: RASTRO, entrada de 25/10/2026 — “Mesa Virtual: bot Telegram criado”):**

1. O bot `@EMEIEFLuizGonzagaAvisos_bot` foi criado no BotFather e está acessível em `https://t.me/EMEIEFLuizGonzagaAvisos_bot`.
2. Nenhuma programação, webhook, integração, resposta automática ou envio de avisos foi implementado.
3. O estado em produção é “apenas o bot cadastrado no Telegram”; a integração com o MultEduc é inexistente.
4. O primeiro piloto previsto é interno, com o responsável pelo projeto e colegas, usando contas e dados fictícios ou autorizados.
5. O menu candidato é: Avisos da Escola; Calendário Escolar; Matrículas e Documentos; Atendimento às Famílias; Ajuda/Privacidade.
6. A arquitetura a discutir inclui Telegram Bot API, webhook HTTPS autenticado/validado, backend, logs mínimos, permissões de publicação, ambientes separados, idempotência, monitoramento, recuperação de falhas e proteção do token.
7. O RASTRO classifica o caso como **N3**: dados pessoais de crianças e responsáveis, backend novo/edge function com segredo do bot e produto novo.

**Indícios (fonte: RASTRO, entrada de 25/10/2026 — “Proposta: avisos às famílias também por Telegram”):**

8. A API de bots do Telegram é descrita como “oficial e gratuita”, com botões de resposta e retorno da resposta ao sistema.
9. O bot “não pode iniciar conversa”: a família precisa abrir o bot por link/QR e aceitar.
10. Conversas de bot “não têm criptografia ponta a ponta”.
11. A adesão das famílias ao Telegram é desconhecida e deve ser medida antes de investir.
12. A ligação família↔criança deve usar código único, nunca apenas o telefone.

**Hipóteses (não provadas nesta sessão):**

13. O Supabase existente suporta o webhook e o volume do piloto sem custo adicional relevante.
14. A escola conseguirá adesão suficiente para justificar o canal.
15. O conteúdo padronizado resolve a maior parte das dúvidas das famílias sem atendimento individual.
16. Um piloto de 4–8 semanas é suficiente para medir adesão, erros e esforço operacional.
17. A equipe escolar tem disponibilidade para aprovar conteúdo e responder encaminhamentos.

**Observação de evidência:** o RASTRO não tem paginação; as citações são localizadas por data e título da entrada. O RASTRO completo, o código, a documentação oficial do Telegram e o parecer jurídico **não foram fornecidos** nesta abertura.

---

### 2.2. O bot criado não é o bot funcionando

O RASTRO é explícito: “**Nenhuma programação, webhook, integração, resposta automática ou envio de avisos foi implementado até este registro. A criação não equivale a funcionamento.**” (RASTRO, 25/10/2026 — “Mesa Virtual: bot Telegram criado”).

Isso impõe uma distinção que deve atravessar todo o caso:

| Camada | O que existe | O que falta |
|---|---|---|
| Telegram | Bot criado no BotFather | Configuração editorial (`/setdescription`, `/setuserpic`), bloqueio de grupos, confirmação de username |
| Backend | Nada | Webhook, validação, fila, idempotência, logs, monitoramento |
| Dados | Nada | Modelo de contato, consentimento, vínculo, aviso, envio, resposta, auditoria |
| Conteúdo | Menu candidato | Textos aprovados, versionamento, fluxo de publicação |
| Operação | Nada | Responsáveis, horário, fallback, suporte, incidentes |
| Jurídico | Nada | Base legal, consentimento, retenção, transferência internacional, direitos |

**Consequência:** qualquer cronograma que trate “bot criado” como “canal pronto” está errado. O caso é de **produto novo**, não de configuração.

---

### 2.3. O consentimento é o gargalo, não a API

O RASTRO registra que “o responsável normalmente precisa iniciar o bot antes de receber mensagens privadas” e que “não presumir envio irrestrito nem criar grupos com exposição de famílias” (RASTRO, 25/10/2026 — “Mesa Virtual: bot Telegram criado”).

Isso significa:

- a escola **não pode** importar números e começar a disparar;
- a família precisa abrir o bot por link/QR e apertar “Iniciar”;
- o consentimento precisa ser **informado, específico e revogável**;
- o opt-out precisa ser tão fácil quanto o opt-in;
- quem não aderir continua no WhatsApp;
- grupos com famílias não são opção no piloto.

**Erro a evitar:** medir sucesso pelo número de pessoas que “entraram no bot” e não pela compreensão, entrega e utilidade dos avisos. Adesão sem confiança é vaidade operacional.

---

### 2.4. O token exposto é bloqueio de segurança, não detalhe

O RASTRO registra:

> “**Incidente de segurança (token fora de lugar):** durante a criação, um print da conversa com o BotFather mostrou na tela o token de um bot **anterior**. Todo token que apareceu em print deve ser tratado como exposto: `/revoke` no BotFather para gerar outro e não reutilizar o antigo. **Não foi confirmado** se a revogação já foi feita.” (RASTRO, 25/10/2026 — “Telegram: bot de avisos criado no BotFather”).

**Recomendação dura:** nenhum código, webhook ou teste com dados reais antes de:

1. confirmar no BotFather se o token exposto foi revogado;
2. gerar um novo token;
3. guardar o novo token em gerenciador de segredos/secret do backend;
4. garantir que o token não está em print, chat, repositório, RASTRO, log ou histórico de shell;
5. definir rotação e procedimento de incidente.

**Risco:** um bot com token exposto pode ser controlado por terceiro, enviar mensagens em nome da escola, alterar menu e receber atualizações. O impacto reputacional e de privacidade é alto, mesmo em piloto.

---

### 2.5. Escopo mínimo recomendado: avisos gerais e menu determinístico

O RASTRO propõe um menu amplo (Avisos, Calendário, Matrículas e Documentos, Atendimento, Ajuda/Privacidade), mas também alerta: “Conteúdo sempre revisado e aprovado pela escola; não presumir disponibilidade de serviços ainda não implementados. Começar com fluxos determinísticos e respostas oficiais, sem IA livre para questões pessoais” (RASTRO, 25/10/2026 — “Mesa Virtual: bot Telegram criado”).

**Escopo mínimo que faz sentido no piloto interno:**

1. `/start` com boas-vindas, identificação do canal oficial, aviso de privacidade e consentimento.
2. Menu com **Avisos da Escola** e **Calendário Escolar** apenas.
3. **Ajuda/Privacidade** com explicação do canal, opt-out e contato da secretaria.
4. Conteúdo **geral**, sem nome de aluno, sem turma individual, sem nota, sem frequência, sem diagnóstico, sem documento.
5. Respostas por botões padronizados (“Li”, “Vou à reunião”, “Não posso ir”), sem texto livre interpretado por IA.
6. Nenhuma consulta individual de matrícula, frequência ou documento no piloto.
7. Nenhum grupo; bot configurado para não ser adicionado a grupos.
8. Fallback: quem não aderir recebe pelo WhatsApp; a secretaria atende por telefone/presencial.

**Por que não começar com “Atendimento às Famílias” e “Matrículas e Documentos”:** esses itens exigem identificação, base legal, tratamento de dados individuais e integração com sistemas; são N3 mais alto e não são necessários para validar o canal. O piloto deve testar o básico: o aviso chega, é compreendido, a família consegue responder e a escola consegue operar.

---

### 2.6. Arquitetura: comparar três caminhos

#### Opção A — Supabase Edge Function + Postgres + Storage (recomendada para o piloto)

**Desenho:** o Telegram chama um webhook HTTPS hospedado em Edge Function; a função valida o segredo, registra o update, responde ao menu e enfileira envios; o painel administrativo do MultEduc grava avisos em tabela; um worker/função envia e registra status.

**Vantagens:**
- usa a stack já existente (Supabase, RLS, funções, autenticação);
- menor custo marginal e menor superfície operacional;
- permite RLS, auditoria e integração com o MultEduc;
- separa teste e produção por projeto/segredo.

**Riscos:**
- limites de tempo, concorrência e cold start da Edge Function precisam ser confirmados;
- webhook precisa de idempotência e tratamento de retry;
- segredo do bot precisa de gestão rigorosa;
- se o volume crescer, a função pode não ser o melhor worker.

**Status:** **hipótese arquitetural**; exige confirmação no código e no plano Supabase.

#### Opção B — backend dedicado (Render ou equivalente) + fila

**Desenho:** serviço HTTP próprio recebe o webhook, grava em Postgres e usa fila/worker para envios.

**Vantagens:**
- mais controle sobre tempo, concorrência, logs e deploy;
- melhor para volume e jobs longos;
- separação clara do Supabase.

**Riscos:**
- mais infraestrutura, custo e operação;
- novo segredo, novo deploy, nova superfície de ataque;
- exige equipe para manter.

**Status:** alternativa se o piloto crescer ou se a Edge Function não atender aos requisitos.

#### Opção C — automação no-code (n8n, Zapier etc.)

**Desvantagens:** difícil garantir RLS, auditoria, minimização, retenção e controle de acesso; risco de exposição de dados em logs e integrações; baixa previsibilidade. **Não recomendada** para dados de crianças.

**Recomendação:** começar pela **Opção A**, com desenho que permita migrar para B sem reescrever o modelo de dados. A decisão final depende de verificação técnica no código e no plano Supabase.

---

### 2.7. Segurança do webhook e dos envios

Requisitos mínimos antes de qualquer teste com dados reais:

1. **HTTPS** com certificado válido; nada de HTTP.
2. **Segredo do webhook** validado em cabeçalho próprio; rejeitar requisições sem segredo.
3. **Validação de origem** e de formato do update; não confiar em campos livres.
4. **Idempotência** por `update_id`/identificador do aviso; reenvio não duplica mensagem.
5. **Allowlist** de `chat_id` autorizados; nenhum envio para chat não consentido.
6. **Fila com retry e backoff**; falha definitiva vai para dead-letter e alerta.
7. **Logs sem PII**: sem nome, telefone, texto integral de resposta ou token; registrar IDs técnicos e status.
8. **Rate limit** e respeito aos limites do Telegram (a confirmar na documentação oficial).
9. **Separação teste/produção**: bot de teste, token de teste, chats de teste; nenhum dado real no teste.
10. **Monitoramento**: health check, taxa de erro, atraso de fila, mensagens não entregues, alertas para admin.
11. **Runbook de incidente**: token vazado, mensagem errada, exposição de dados, bot bloqueado, indisponibilidade.
12. **Backup e retenção** configuráveis; expurgo testado.

**Não fazer:** confiar apenas no token na URL; usar o mesmo bot para teste e produção; gravar payload completo com dados pessoais; usar grupo como “sala de avisos”; responder automaticamente a texto livre com IA.

---

### 2.8. Modelo de dados mínimo

Proposta para discussão (não implementar sem aprovação):

| Entidade | Campos mínimos | Finalidade | Cuidado |
|---|---|---|---|
| `telegram_contato` | id, chat_id (protegido), status, consentimento_versao, consentimento_em, opt_out_em | Registrar quem aceitou receber | chat_id é dado pessoal; proteger e permitir expurgo |
| `telegram_vinculo` | id, contato_id, codigo_familia, aluno_id (ou referência opaca), status, verificado_em, verificado_por | Ligar família à criança | código único, nunca telefone; acesso restrito; mínimo necessário |
| `telegram_aviso` | id, titulo, corpo, publico, versao, status, aprovado_por, publicado_em | Conteúdo oficial | sem dados individuais; versionamento e aprovação |
| `telegram_envio` | id, aviso_id, contato_id, status, tentativas, erro_codigo, enviado_em | Fila e entrega | idempotência; sem corpo sensível em log |
| `telegram_resposta` | id, envio_id, contato_id, botao, respondido_em | Respostas por botão | não interpretar texto livre; minimizar |
| `telegram_auditoria` | id, ator, acao, entidade, quando, ip_hash | Auditoria | sem PII desnecessária; retenção definida |

**Regras:** RLS ligada; acesso somente por funções/RPCs autenticadas; `chat_id` criptografado ou tokenizado; nenhum nome de criança em log; expurgo por prazo definido; direito de acesso/eliminação conforme LGPD.

**Ponto em aberto:** o vínculo família↔criança é realmente necessário no piloto? Se o piloto só envia avisos gerais, o vínculo individual pode ser **dispensado**. Isso reduz risco e acelera o teste. Recomendo **não criar vínculo individual no piloto interno**; criar apenas contato consentido e preferências. O vínculo entra na Fase 2, com código único e verificação.

---

### 2.9. Conteúdo, governança editorial e permissões

O RASTRO exige “conteúdo sempre revisado e aprovado pela escola” e “fluxo de publicação de avisos por pessoal autorizado” (RASTRO, 25/10/2026 — “Mesa Virtual: bot Telegram criado”).

**Fluxo mínimo:**

1. Pessoa autorizada redige o aviso em template.
2. Conteúdo passa por revisão pedagógica/administrativa.
3. Aprovação registrada (quem, quando, versão).
4. Publicação em ambiente de teste.
5. Envio para grupo de teste.
6. Validação de texto, links, horário e público.
7. Publicação em produção.
8. Registro de envio, respostas e incidentes.

**Permissões:** separar quem **redige**, quem **aprova** e quem **publica**. Não dar ao bot ou à IA poder de publicar sozinho. O menu e os textos devem ser versionados; mudança de texto é mudança de produto.

**Conteúdo proibido no piloto:** nome de aluno, turma individual, nota, frequência, diagnóstico, deficiência, documento, senha, dados bancários, informações de saúde, imagens de crianças.

**Tom:** claro, curto, acolhedor, em português simples; sem ironia, sem urgência falsa, sem excesso de mensagens. A escola não deve pedir senha nem dados bancários pelo bot.

---

### 2.10. LGPD e proteção de crianças

O RASTRO já identifica os pontos: “consentimento/base legal e transparência para comunicados, opt-out/bloqueio, LGPD, proteção reforçada dos dados de crianças, minimização de dados, restrição de acesso, retenção, auditoria e validação de identidade antes de consultas individualizadas” (RASTRO, 25/10/2026 — “Mesa Virtual: bot Telegram criado”).

**Análise preliminar (não é parecer jurídico):**

- **Base legal:** não presumir consentimento como única base. Comunicados escolares podem ter base legal diversa; dados de crianças exigem análise específica. O parecer jurídico deve definir a base para cada finalidade (aviso geral, resposta, vínculo, atendimento).
- **Minimização:** o piloto deve coletar o mínimo: chat_id, consentimento, preferência de canal. Nada de nome de criança, turma, diagnóstico ou frequência.
- **Transparência:** informar quem é o controlador, finalidade, canais, prazo, direitos, contato do encarregado e como sair.
- **Consentimento:** registro de versão, data e texto apresentado; opt-out fácil; não condicionar benefício escolar ao uso do Telegram.
- **Crianças:** o canal fala com responsáveis, não com crianças; ainda assim, dados de crianças são sensíveis. Evitar qualquer dado individual no piloto.
- **Transferência internacional:** o Telegram processa dados em servidores próprios; verificar termos, país, salvaguardas e contrato/DPA aplicável.
- **Segurança:** criptografia em trânsito, segredo do bot, controle de acesso, logs mínimos, auditoria, resposta a incidentes.
- **Retenção:** definir prazo para mensagens, respostas e logs; permitir expurgo; não guardar indefinidamente “para histórico”.
- **Direitos:** acesso, correção, eliminação, informação, revogação; canal para exercer.
- **Papéis:** escola (controladora), MultEduc/fornecedor (operador), Telegram (terceiro/suboperador), equipe (operadores). Formalizar responsabilidades.

**Conclusão preliminar:** sem parecer jurídico e sem definir base legal, retenção e transferência internacional, o piloto com famílias reais **não deve começar**. O piloto interno com dados fictícios pode começar depois da Fase 0.

---

### 2.11. WhatsApp e MultEduc: complementar, não substituir

O RASTRO registra que o WhatsApp já funciona com fila manual “um responsável por vez” e que o Telegram seria “segunda opção de envio”, adotado aos poucos, mantendo o WhatsApp para quem não aderir (RASTRO, 25/10/2026 — “BF — Frequência: envio por WhatsApp em lote” e “Proposta: avisos às famílias também por Telegram”).

**Recomendações:**

1. **Não** migrar todos os avisos para o Telegram de uma vez.
2. **Não** duplicar automaticamente a mesma mensagem nos dois canais sem controle; isso cansa a família e gera erro.
3. Registrar **preferência de canal** por família; quem não escolher Telegram continua no WhatsApp.
4. Unificar o **registro de contato** e o **histórico** (quem recebeu, quando, por qual canal), sem duplicar dados.
5. O painel do MultEduc deve ser a fonte do aviso; o Telegram é uma saída, não uma ilha.
6. Medir adesão, entrega, leitura/resposta e suporte antes de ampliar.
7. Manter o atendimento humano e o telefone/secretaria como fallback.

**Risco de equidade:** famílias sem smartphone, sem internet, sem Telegram ou com letramento digital limitado podem ser excluídas se o canal for tratado como principal. O WhatsApp e o papel/telefone continuam necessários. A escola deve medir quem não adere e por quê, não apenas contar quem aderiu.

---

### 2.12. Custos: o que é barato e o que não é

**Custo direto provável:** a API de bots do Telegram é registrada no RASTRO como “oficial e gratuita”; o Supabase/Render já existem. Isso **não** significa custo zero:

- horas de equipe para arquitetura, código, testes, conteúdo, suporte e treinamento;
- eventual aumento de plano Supabase/Render se o volume crescer;
- parecer jurídico/LGPD;
- monitoramento e resposta a incidentes;
- tempo de secretaria para atendimento e fallback;
- custo de oportunidade: cada semana gasta no bot não é semana gasta em aprendizagem, frequência ou formação.

**Não contratar** IA paga para o piloto. Se no futuro houver IA, ela deve ser opcional, com conteúdo aprovado, transferência mínima de dados e revisão humana. O piloto não precisa de IA para funcionar.

---

### 2.13. Cronograma proposto (não compromisso)

| Fase | Prazo indicativo | Entregável | Portão |
|---|---:|---|---|
| 0 — Segurança e governança | 1 semana | Token revogado/rotacionado, username confirmado, mapa de dados, base legal preliminar, política de retenção, matriz de riscos | Mediador + jurídico aprovam |
| 1 — Piloto interno | 2–3 semanas | Bot de teste, webhook, menu Avisos/Calendário/Ajuda, 5–10 testadores internos, dados fictícios, testes de erro/opt-out/idempotência | 0 incidentes críticos; critérios de aceite atendidos |
| 2 — Piloto com famílias autorizadas | 3–4 semanas | 20–30 famílias consentidas, avisos gerais, respostas por botão, fallback WhatsApp, métricas | Aderência, entrega, compreensão, suporte e LGPD aprovados |
| 3 — Produção gradual | 4–6 semanas | Expansão por grupos, treinamento, monitoramento, runbook, revisão | Mediador autoriza; métricas estáveis |

**Total indicativo:** 8–12 semanas, dependendo de aprovação jurídica, disponibilidade da equipe e acesso ao código. **Não** prometer prazo sem essas condições.

---

### 2.14. Critérios de aceite propostos

| # | Critério | Evidência exigida | Fase |
|---:|---|---|---|
| 1 | Token exposto revogado e novo token em segredo | Captura do BotFather sem token; registro de rotação; varredura no repo/logs | 0 |
| 2 | Username confirmado e bot configurado | `/getme` ou tela do BotFather; descrição, foto e grupos desabilitados | 0 |
| 3 | Base legal, retenção e transferência internacional definidas | Parecer jurídico/LGPD registrado | 0 |
| 4 | `/start` com consentimento, privacidade e opt-out | Teste interno com 5–10 contas; texto aprovado | 1 |
| 5 | Menu Avisos/Calendário/Ajuda funciona | Teste funcional; nenhuma consulta individual | 1 |
| 6 | Webhook valida segredo, idempotência e erro | Testes de replay, segredo inválido, timeout e retry | 1 |
| 7 | Logs sem PII e auditoria de publicação | Revisão de logs; teste com dados fictícios | 1 |
| 8 | Nenhum grupo; bot não adicionável a grupos | Configuração e teste | 1 |
| 9 | Fallback WhatsApp/telefone documentado | Runbook e teste de família sem Telegram | 2 |
| 10 | Piloto com 20–30 famílias consentidas | Métricas de adesão, entrega, resposta, suporte e incidentes | 2 |
| 11 | Nenhum dado de criança em mensagem/log | Auditoria de conteúdo e logs | 2 |
| 12 | Incidente simulado e resposta em tempo definido | Registro de simulação e correção | 2 |
| 13 | Aprovação do Mediador para produção | Registro formal | 3 |

**Critérios quantitativos** (a validar com a escola): entrega ≥ 95% dos avisos do piloto; falha definitiva < 2%; nenhum incidente crítico; suporte < 10% das famílias; compreensão ≥ 80% em questionário curto. **São metas propostas, não fatos.**

---

### 2.15. Matriz de decisão

| Opção | O que é | Vantagem | Risco | Custo | Recomendação |
|---|---|---|---|---|---|
| A | Não implementar agora | Zero risco novo; foco no que já funciona | Perde oportunidade de testar canal | Zero | Aceitável se a escola não tiver equipe/patrocínio |
| B | Piloto interno, dados fictícios | Valida técnica e operação sem expor famílias | Ainda não mede adesão real | Baixo | **Recomendada agora**, após Fase 0 |
| C | Piloto com famílias autorizadas | Mede adesão, compreensão e suporte | Exige LGPD, consentimento, suporte | Médio | **Recomendada após B**, com portões |
| D | Produção ampla imediata | Velocidade aparente | Alto risco de exposição, erro e exclusão | Alto | **Não recomendada** |

**Decisão proposta:** B agora; C somente depois de B e do parecer jurídico; D somente depois de C e de aprovação formal.

---

### 2.16. Responsabilidades (RACI proposto)

| Atividade | Ricardo | ChatGPT | Claude | DeepSeek | Escola/secretaria | Jurídico/DPO |
|---|---|---|---|---|---|---|
| Aprovar escopo e portões | A | C | C | C | C | C |
| Arquitetura e segurança | A | R | C | C | I | C |
| Código e testes | A | C | R | C | I | I |
| Conteúdo e revisão | A | C | I | C | R | C |
| LGPD e retenção | A | C | I | C | C | R |
| Piloto interno | A | C | R | C | C | I |
| Piloto com famílias | A | C | C | C | R | C |
| Publicação/produção | A | I | R | I | C | C |
| Monitoramento e incidentes | A | C | R | C | C | C |

**Legenda:** R = responsável; A = aprovador; C = consultado; I = informado.

---

## 3. Riscos e pontos fracos da própria análise

1. **Não li o código, o schema, as edge functions nem o RASTRO completo.** A avaliação de arquitetura é conceitual; a viabilidade no Supabase precisa ser confirmada.
2. **Não verifiquei a documentação oficial do Telegram nesta sessão.** As afirmações sobre API, botões, webhook e limites vêm do RASTRO e estão marcadas como “a confirmar”.
3. **Não há parecer jurídico/LGPD.** A análise aponta necessidades, não substitui advogado/DPO.
4. **Não há métricas de adesão.** O cronograma e os critérios quantitativos são propostas, não previsões.
5. **Não há orçamento Validado.** Custos indiretos (horas de equipe, suporte, jurídico) não foram estimados em reais.
6. **O token pode continuar exposto.** Se a revogação não foi feita, todo o restante é secundário.
7. **O username pode estar errado.** O RASTRO diz que o username final deveria ser confirmado; um link/QR errado pode levar a outro bot.
8. **O risco de exclusão digital é real.** O Telegram pode reforçar desigualdade se for tratado como canal principal.
9. **O piloto interno pode dar falsa confiança.** Colegas com letramento digital não representam famílias; a Fase 1 não substitui a Fase 2.
10. **A análise pode estar conservadora demais.** Se a escola já tem base legal, equipe e política de dados, alguns portões podem ser mais rápidos; mas isso precisa ser provado, não presumido.

---

## 4. Pontos que exigem decisão do Mediador

1. **Token:** confirmar se o token exposto foi revogado; autorizar a rotação e o armazenamento em segredo.
2. **Username:** confirmar o username público exato e o link/QR oficial.
3. **Escopo do piloto:** aprovar o menu mínimo (Avisos, Calendário, Ajuda/Privacidade) e excluir, no piloto, Matrículas/Documentos e Atendimento individual.
4. **Vínculo família↔criança:** decidir se o piloto interno terá apenas contato consentido (recomendado) ou já terá vínculo por código.
5. **Base legal e retenção:** obter parecer jurídico/DPO e definir prazo de guarda, transferência internacional e direitos.
6. **Arquitetura:** autorizar o estudo da Opção A (Supabase) e a comparação com a Opção B; não escolher antes de ver o código.
7. **Canal:** confirmar que o Telegram é complementar e que o WhatsApp continua; definir quem não aderir.
8. **Conteúdo:** aprovar os textos de `/start`, privacidade, menu, avisos e opt-out.
9. **Piloto:** definir 5–10 testadores internos e, depois, 20–30 famílias autorizadas; nenhum disparo em massa.
10. **Critérios de aceite:** ratificar ou ajustar as metas propostas (entrega, falha, suporte, compreensão).
11. **Orçamento:** aprovar horas de equipe, eventual custo de infraestrutura e parecer jurídico.
12. **Publicação:** autorizar expressamente cada fase; nada vai a produção sem aprovação formal.

---

## 5. Fontes consultadas

1. `C:\processo_inventario\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\00_FONTES\RASTRO_2026-10-25_EXCERTO_TELEGRAM.md` — excerto das entradas de 25/10/2026: Mesa Virtual, bot criado, BotFather, proposta de Telegram, WhatsApp em lote e mantra.
2. `C:\processo_inventario\MESA_VIRTUAL\MESA_VIRTUAL_PROTOCOLO.md` — protocolo da Mesa Virtual, seções 4, 6, 7 e 8.
3. `C:\processo_inventario\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\00_FONTES\INDICE_FONTES.md` — índice e limites das fontes.
4. `C:\processo_inventario\MESA_VIRTUAL\MULTEDUC_BOT_TELEGRAM\01_PERGUNTAS\00_ABERTURA.md` — pergunta central, premissas e critérios de parada.

**Fontes ausentes e declaradas:** RASTRO completo, código/schema do MultEduc, documentação oficial do Telegram, parecer jurídico/LGPD, métricas de adesão. Essas ausências limitam a conclusão e estão registradas nos riscos.

---

## 6. Anexo A — QC/VM/PE/PT/PO/IMF aplicado ao caso

| Camada | Pergunta | Resposta sintética |
|---|---|---|
| **QC — quebra-cabeça** | Quais peças precisam se encaixar? | Token, username, bot, webhook, backend, dados, consentimento, conteúdo, permissões, suporte, LGPD, piloto, métricas |
| **VM — visão macro** | Qual é o lugar do Telegram na comunicação da escola? | Canal complementar, não substituto; WhatsApp continua; foco em avisos gerais e menu determinístico |
| **PE — planejamento estratégico** | O que se quer alcançar? | Comunicação mais rápida e organizada, sem expor dados, sem excluir famílias e sem substituir atendimento humano |
| **PT — planejamento tático** | Como fazer sem risco? | Quatro fases com portões; piloto interno antes de famílias; arquitetura mínima; conteúdo aprovado |
| **PO — planejamento operacional** | Quem faz o quê, quando e com qual evidência? | RACI, critérios de aceite, runbook, monitoramento, treinamento, fallback |
| **IMF — início, meio e fim** | Como começar, medir e encerrar? | Início com Fase 0/1; meio com piloto e métricas; fim com decisão go/no-go e produção gradual ou encerramento |

---

## 7. Anexo B — Sequência de implementação recomendada (se aprovada)

1. **Dia 0:** confirmar revogação do token; gerar novo; guardar em segredo; confirmar username; desabilitar grupos; revisar descrição/foto.
2. **Dias 1–3:** mapear dados, base legal, retenção e transferência internacional; reunir jurídico/DPO.
3. **Dias 3–7:** definir escopo mínimo, textos, menu, fluxo de aprovação e critérios de aceite.
4. **Semana 2:** montar ambiente de teste; webhook com segredo; idempotência; logs sem PII; testes unitários e de replay.
5. **Semana 2–3:** piloto interno com 5–10 pessoas; dados fictícios; testar `/start`, menu, aviso, calendário, ajuda, opt-out, erro, bot bloqueado, timeout.
6. **Semana 3:** corrigir; revisar com o Mediador; decidir go/no-go para Fase 2.
7. **Semana 4–7:** piloto com 20–30 famílias consentidas; avisos gerais; respostas por botão; fallback WhatsApp; métricas.
8. **Semana 7–8:** avaliar; se aprovado, preparar produção gradual; se não, encerrar ou redesenhar.
9. **Produção:** expandir por grupos, com monitoramento e revisão; nunca “big bang”.

---

## 8. Anexo C — O que não fazer

- Não publicar o bot para famílias antes da Fase 0 e do parecer jurídico.
- Não usar o token exposto; não colar token em chat, print, repo ou RASTRO.
- Não criar grupos de famílias.
- Não importar números e disparar sem consentimento.
- Não enviar nome de aluno, turma, nota, frequência, diagnóstico, documento ou dado de saúde.
- Não usar IA livre para responder questões individuais.
- Não prometer prazo, custo zero ou adesão antes de medir.
- Não tratar “bot criado” como “canal funcionando”.
- Não substituir o WhatsApp nem o atendimento humano.
- Não considerar o piloto interno como prova de adesão das famílias.

---

> **Nota final:** este parecer é independente, produzido antes de ler os pareceres de ChatGPT e Claude. Ele não autoriza implementação, publicação ou envio. A decisão é do Mediador, após a revisão cruzada e o consenso.
