# Fluxo de Trabalho: Montagem do Plano de Gestão da Mudança

Siga os passos na ordem. Não pule etapas e não mostre os cálculos intermediários para a pessoa, a menos que ela peça. Mostre só o resultado final no formato do template.

---

## Passo 1: Diagnóstico de Prazo e Capacidade

Com as informações do gating, calcule:

* **Dias úteis disponíveis**: conte apenas os dias úteis entre o status/data atual e o **dia anterior** ao prazo final (o dia do go-live tem ações próprias).
* **Margem de segurança** = reserve ao menos 1 a 2 dias antes da virada oficial sem ações pesadas de treinamento, focando apenas em estabilização e plantões de dúvidas (Hypercare).

Se o prazo tiver menos de 5 dias úteis, avise que o plano vai ser um **intensivão de contingência** e foque só no alinhamento crítico e suporte imediato.

## Passo 2: Decomposição das Fases

1. Quebre o objetivo da mudança em **3 a 5 fases**, ordenadas por ordem de execução (ex: Conscientização, Engajamento, Capacitação, Virada).
2. Estime o esforço de cada fase considerando os **impactos descritos**:
   * Impacto Baixo: foque mais em comunicação assíncrona.
   * Impacto Alto: priorize oficinas, treinamentos síncronos e testes piloto.
3. **Se o volume de ações não couber nos dias úteis:** aplique a regra de priorização. Mantenha os alinhamentos e treinamentos essenciais, marque ações secundárias como `Opcional` e liste-as na seção "O que ficou de fora" do template, explicando por quê.

## Passo 3: Distribuição das Ações

1. Use `load_skill_resource` para ler `references/tecnicas.md`.
2. Distribua as ações pelas fases e dias disponíveis, respeitando:
   * **Lógica temporal**: comunicações amplas sempre precedem treinamentos técnicos.
   * **Formato e entregável**: cada ação tem um formato correspondente em `tecnicas.md` e um entregável concreto.
   * **Etapa final**: as ações na véspera do prazo são de monitoramento e plantão de dúvidas, sem inserção de novos processos complexos.
3. Não agende reuniões extensas nos finais de semana, a menos que seja uma janela técnica exigida pelo usuário.

## Passo 4: Checkpoints de Acompanhamento

No último dia útil de cada semana do plano, inclua um **checkpoint** com:

* **1 a 3 perguntas de validação** (status report) sobre o engajamento e a execução técnica da semana.
* **Critério de sucesso**: o que a equipe precisa ter atingido para avançar com segurança (ex.: "100% da base principal recebeu o comunicado e os líderes aprovaram a matriz").
* **Ação se não passar**: qual frente revisar ou intensificar e quando encaixar essa ação corretiva.

## Passo 5: Montagem da Resposta

1. Use `load_skill_resource` para ler `assets/plano_template.md`.
2. Preencha **todas** as seções do template, na ordem.
3. Monte o **CSV para o Google Agenda** seguindo exatamente as regras descritas no template:
   * Se o plano tiver **até 20 ações**, inclua todas as sessões.
   * Se tiver **mais de 20**, inclua as essenciais das primeiras semanas e avise: "Quer que eu gere o CSV detalhado das próximas fases?".
4. Use `load_skill_resource` para ler `references/salvar_agenda.md` e salve o CSV com a ferramenta `WriteFile` em `plano-de-gestao.csv`, seguindo o checklist e as regras de caminho desse arquivo.
   * Se a ferramenta `WriteFile` não estiver disponível, mostre o CSV num bloco ```` ```csv ```` na seção 5 do template.
5. Na resposta, preencha a seção 5 do template com o **caminho do arquivo salvo** e o número de ações agendadas. Não repita o CSV inteiro na conversa quando o arquivo tiver sido salvo.

## Passo 6: Ajustes e Replanejamento

Depois de entregar o plano, a pessoa pode pedir mudanças. Siga estas regras:

* **"O projeto atrasou"**: não empilhe todas as ações na mesma semana. Redistribua os treinamentos e use a folga para reforço de comunicação. Nunca corte os testes piloto sem alertar sobre o risco.
* **"Mudou o status / escopo"**: refaça a partir do Passo 1, mantendo as comunicações e alinhamentos que já foram feitos.
* **"Quero mais/menos detalhe nas ações"**: ajuste só a apresentação, sem refazer as datas do cronograma.
* Em qualquer ajuste, devolva a tabela de ações atualizada **e** regrave o `plano-de-gestao.csv`, não só a explicação do que mudou. Siga a seção "Replanejamento" de `references/salvar_agenda.md`.