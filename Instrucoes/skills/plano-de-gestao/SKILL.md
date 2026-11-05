---
name: plano-de-gestao
description: Monta um plano de gestão da mudança personalizado, com marcos de comunicação, checkpoints de acompanhamento de status e cronograma exportável para o Google Agenda (CSV). Use quando a pessoa quiser organizar iniciativas de mudança, estruturar as ações de um projeto ou mitigar impactos até uma data final.
---

# [IMPORTANTE] UTILIZAR ESTA SKILL PARA ORGANIZAR INICIATIVAS DE MUDANÇA

Esta skill transforma os dados de um projeto em um **plano de gestão da mudança executável**: ações com data e hora, mapeamento de impactos, checkpoints de status e um arquivo CSV que pode ser importado direto no Google Agenda.

Um plano genérico não ajuda ninguém. Por isso, antes de planejar qualquer coisa, você precisa conhecer o projeto da pessoa.

## Regra de Ativação (Gating Rule)

Verifique se a conversa já contém **todas** as informações obrigatórias abaixo:

| # | Informação obrigatória | Exemplo |
|---|---|---|
| 1 | **Título**: o nome da iniciativa de mudança | "Migração para o novo sistema ERP" |
| 2 | **Descrição**: contexto e impactos esperados | "Muda a rotina de 50 pessoas do time de vendas" |
| 3 | **Prazo**: data de entrega ou go-live | "go-live em 20/11" |
| 4 | **Status**: a situação atual do projeto | "planejamento inicial" / "em andamento" |

- **Se faltar alguma informação:** **NÃO chame** a ferramenta `load_skill_resource` e não carregue nenhum arquivo de `references/` ou `assets/`. Faça **uma única mensagem**, curta e profissional, pedindo **apenas** o que está faltando (não repita perguntas já respondidas). 
- **Se a pessoa disser "tanto faz" ou "você decide"** para algum item que não seja o título, assuma um padrão razoável, **declare a suposição** no início do plano e siga em frente.
- **Se todas as informações estiverem presentes:** prossiga imediatamente para as instruções abaixo.

## Instruções (apenas depois que o gating for satisfeito)

1. Use a ferramenta `load_skill_resource` para ler `references/workflow.md`.
2. Siga **estritamente** os passos do `references/workflow.md`. Ele indica o momento certo de carregar `references/tecnicas.md`, `assets/plano_template.md` e `references/salvar_agenda.md`.
3. Se for o primeiro plano da conversa, você pode consultar `assets/exemplo_plano.md` para calibrar o nível de detalhe esperado.
4. Salve a agenda com a ferramenta `WriteFile` no arquivo `plano-de-gestao.csv`, seguindo `references/salvar_agenda.md` (indicado no Passo 5 do workflow).

## Regras gerais

- Responda sempre em **português do Brasil**, num tom de especialista em gestão da mudança e produtos digitais: direto, estratégico e sem enrolação.
- **Nunca invente datas.** Calcule cada data a partir da data de início e do prazo informado para a iniciativa.
- **Nunca ignore os impactos** informados. Se o volume de ações de gestão da mudança não couber no prazo, diga isso com clareza e proponha o que priorizar ou cortar (veja o Passo 2 do workflow).
- Toda ação precisa ter um **entregável concreto** (comunicado enviado, reunião de alinhamento realizada, treinamento concluído). "Acompanhar a equipe" não é um entregável.