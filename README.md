# Mesa Virtual MultEduc

Repositório de discussão técnica, revisão cruzada e governança da Mesa Virtual MultEduc.

**Natureza:** documentos de análise e decisão. Não é código de produção, não é documentação oficial do MultEduc e não autoriza implementação.

## Casos em andamento

| Caso | Estado | Documento de abertura |
|---|---|---|
| `MULTEDUC_BOT_TELEGRAM` | EM_REVISÃO | [`01_PERGUNTAS/00_ABERTURA.md`](MULTEDUC_BOT_TELEGRAM/01_PERGUNTAS/00_ABERTURA.md) |
| `VIRADA_2026_2027` | ABERTA | [`01_PERGUNTAS/00_ABERTURA.md`](VIRADA_2026_2027/01_PERGUNTAS/00_ABERTURA.md) |

## Protocolo

O método da Mesa está em [`MESA_VIRTUAL_PROTOCOLO.md`](MESA_VIRTUAL_PROTOCOLO.md):

**QC → VM → PE → PT → PO → IMF**

1. Qualificação e contexto.
2. Visão e modelo.
3. Pareceres independentes.
4. Teste e confrontação.
5. Posição e decisão.
6. Implementação e acompanhamento.

## Estrutura

```text
MESA_VIRTUAL/
├── MESA_VIRTUAL_PROTOCOLO.md
├── MULTEDUC_BOT_TELEGRAM/
│   ├── 00_FONTES/
│   ├── 01_PERGUNTAS/
│   ├── 03_PARECER_DEEPSEEK/
│   └── 05_REVISAO_CRUZADA/
└── VIRADA_2026_2027/
    ├── 00_FONTES/
    └── 01_PERGUNTAS/
```

As pastas `02_PARECER_CHATGPT`, `04_PARECER_CLAUDE`, `06_DIVERGENCIAS`, `07_CONSENSO` e `08_DOCUMENTO_FINAL` serão preenchidas conforme o caso avançar.

## Regras

- Nenhum documento deste repositório autoriza implementação, migração, deploy, webhook ou alteração de banco.
- Pareceres preliminares são histórico, não prova.
- Cada afirmação deve ser classificada como fato provado, indício, hipótese ou opinião.
- Fontes ausentes devem ser declaradas, não preenchidas por suposição.
- Nada sai da Mesa sem autorização expressa do Mediador.
- Não há tokens, credenciais, senhas ou dados pessoais de crianças/responsáveis neste repositório.

## Aviso de segurança

Este repositório contém análise de riscos e achados de segurança em nível de arquitetura, sem credenciais e sem dados pessoais. Se a visibilidade for alterada para pública, os documentos devem ser revisados antes da publicação. Achados de segurança não devem ser usados para explorar sistemas.

## Estado

Discussão em andamento. Não há consenso final nem autorização de implementação.
