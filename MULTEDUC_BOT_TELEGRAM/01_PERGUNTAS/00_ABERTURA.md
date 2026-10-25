# ABERTURA — MULTEDUC_BOT_TELEGRAM

**Estado:** EM_REVISÃO  
**Data de abertura:** 25/10/2026  
**Mediador:** Ricardo  
**Sistema:** OpenCode  
**Caso:** MULTEDUC_BOT_TELEGRAM — bot de avisos e atendimento no Telegram, integrado ao WhatsApp/MultEduc  
**Método exigido:** QC → VM → PE → PT → PO → IMF

---

## 1. Pergunta central

**O bot `@EMEIEFLuizGonzagaAvisos_bot` deve ser implementado agora? Com qual escopo, arquitetura, segurança, piloto interno, critérios de aceite e cronograma, considerando a integração com WhatsApp/MultEduc e a LGPD?**

Pergunta derivada pelo Sistema a partir da entrada mais recente do RASTRO (25/10/2026), que registra a criação do bot e o encaminhamento de pareceres independentes. **Sujeita à ratificação do Mediador.**

---

## 2. Problema que o caso examina

O RASTRO registra que o bot foi criado no BotFather, mas que **“nenhuma programação, webhook, integração, resposta automática ou envio de avisos foi implementado”**. A criação do bot não é funcionamento. O caso deve decidir se, quando e sob quais condições o canal pode sair do papel, sem:

- substituir o atendimento humano;
- expor dados de crianças e responsáveis;
- criar grupos com famílias;
- presumir adesão ou envio irrestrito;
- transformar uma proposta em publicação.

---

## 3. Objetivos do caso

1. Separar **fato provado**, **indício**, **hipótese** e **opinião** sobre o bot.
2. Definir o **escopo mínimo** de um piloto interno seguro.
3. Comparar alternativas de arquitetura e custo.
4. Identificar riscos técnicos, operacionais, jurídicos e de governança.
5. Propor critérios de aceite verificáveis.
6. Propor cronograma e responsabilidades.
7. entregar uma matriz de decisão para o Mediador.

---

## 4. Fora de escopo

- Implementar código, criar webhook, publicar função ou alterar banco neste caso.
- Enviar mensagens a famílias reais.
- Definir sozinho a base legal da LGPD; o caso aponta a necessidade de parecer jurídico.
- Substituir o WhatsApp ou o atendimento humano.
- Usar IA livre para responder questões individuais de famílias.
- Criar grupos de Telegram com responsáveis.
- Reproduzir token, credenciais ou dados pessoais.

---

## 5. Fontes disponíveis

| Fonte | Caminho | Uso |
|---|---|---|
| RASTRO — excerto Telegram/Mesa Virtual | `00_FONTES/RASTRO_2026-10-25_EXCERTO_TELEGRAM.md` | Fatos, propostas e citações literais do projeto |
| Protocolo da Mesa Virtual | `C:\MESA_VIRTUAL\MESA_VIRTUAL_PROTOCOLO.md` | Regras de evidência, ciclo e formato |
| `docs/MESA-VIRTUAL-AGENTE-TELEGRAM.md` | Repositório do MultEduc; citado por Claude | API do Telegram; **a confirmar** |
| `supabase/functions` e `config.toml` | Repositório do MultEduc; citado por Claude | Funções e `verify_jwt`; **a confirmar** |
| Revisão cruzada de Claude | `05_REVISAO_CRUZADA/REVISAO_CRUZADA_CLAUDE.md` | Concordâncias, divergências e evidência nova |
| Resposta do DeepSeek | `05_REVISAO_CRUZADA/RESPOSTA_DEEPSEEK_A_REVISAO_CLAUDE.md` | Posição revisada do DeepSeek |
| RASTRO completo, código, documentação oficial do Telegram, parecer LGPD, parecer ChatGPT e parecer rodada 1 do Claude | Não fornecidos nesta abertura | **A confirmar / ausente**; ausência declarada |

---

## 6. Premissas de trabalho

1. O bot existe no Telegram, mas **não está conectado ao MultEduc** (fato registrado no RASTRO).
2. O token do bot apareceu em um print; a revogação **não foi confirmada** (fato registrado no RASTRO).
3. O username público informado é `@EMEIEFLuizGonzagaAvisos_bot`; o RASTRO também registra que o username final deveria ser confirmado.
4. O piloto inicial é **interno**, com responsável pelo projeto e colegas, usando dados fictícios ou autorizados.
5. O WhatsApp permanece como canal principal para quem não aderir ao Telegram.
6. O caso é **N3** pela classificação do próprio RASTRO: dados de crianças/responsáveis, backend novo, segredo do bot e produto novo.

---

## 7. Critérios de parada

O caso para e devolve ao Mediador se:

- o token exposto não tiver sido revogado;
- não houver base legal/clareza mínima de LGPD para o piloto;
- o Mediador não confirmar o escopo do piloto;
- não houver acesso ao código/schema necessário para avaliar a integração;
- a documentação oficial do Telegram contradizer alguma premissa do RASTRO;
- surgir risco de exposição de dados de crianças que não possa ser eliminado no piloto.

---

## 8. Resultado esperado

`03_PARECER_DEEPSEEK/PARECER_DEEPSEEK.md`, seguido dos pareceres de ChatGPT (`02_PARECER_CHATGPT/`) e Claude (`04_PARECER_CLAUDE/`), da revisão cruzada (`05_REVISAO_CRUZADA/`), da matriz de divergências (`06_DIVERGENCIAS/`), do consenso (`07_CONSENSO/`) e, somente após aprovação do Mediador, do documento final (`08_DOCUMENTO_FINAL/`).

---

## 9. Controle de estado

| Data | Estado | Evento |
|---|---|---|
| 25/10/2026 | ABERTA | Estrutura criada; fontes indexadas; pergunta registrada |
| 25/10/2026 | EM_ANÁLISE | Parecer DeepSeek produzido |
| 25/10/2026 | EM_REVISÃO | Pareceres de ChatGPT e Claude recebidos; revisão cruzada DeepSeek registrada em `05_REVISAO_CRUZADA/PARECER_DEEPSEEK_RODADA2.md` |
| 25/10/2026 | EM_REVISÃO | Revisão cruzada de Claude depositada; resposta do DeepSeek depositada; parecer formal do ChatGPT e parecer da rodada 1 do Claude ainda ausentes como arquivo |

> **Regra:** nada sai da Mesa sem autorização explícita do Mediador. Nenhum arquivo deste caso deve ser enviado a terceiros, publicado ou transformado em implementação por iniciativa do Sistema.
