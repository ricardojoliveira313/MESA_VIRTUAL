# Índice de fontes

Caso: Auditoria de segurança e paridade dos bancos MultEduc.

- Consultas SQL read-only ao projeto REL_AEE 2026 (`hxmw...`) e ao ENSAIO 2027 (`dvcshnudtzzjjklyejwf`). O primeiro não recebeu escrita.
- Advisors Supabase de segurança e performance executados nos dois projetos em 08/11/2026.
- Inventário de migrations fornecido pela API do Supabase; comparação limitada aos arquivos disponíveis no checkout local e à branch de trabalho.
- Código consultado: `frontend/src/api.ts`, Edge Functions `enviar-codigo-ativacao` e `alerta-push`, migrations e funções SQL pertinentes.
- Teste controlado com contas/escola/turma/alunos sintéticos no ENSAIO. Nenhum dado real de aluno foi incluído.
- Builds: `npx tsc --noEmit` e `npm run build` na pasta frontend.

Limites: o hosting Render não foi inspecionado porque não havia workspace selecionado; histórico integral de branches não estava disponível; a prova autenticada HTTP pós-correção ainda está pendente. Nenhuma credencial, token, hash ou dado pessoal é armazenado neste caso.
