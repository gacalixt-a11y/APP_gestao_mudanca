# AGENTS.md: Agente de Mudança

## Goal

Construir o **`agente-mudanca`**: um agente **Google ADK** (Python) movido a skills, que monta planos de ação para iniciativas de gestão da mudança a partir da skill `skills/plano-de-mudanca/`. O plano inclui marcos de comunicação, treinamentos, checkpoints de adoção e um cronograma em CSV que pode ser importado no Google Agenda. O agente salva essa agenda em arquivo com o **EnvironmentToolset** do ADK e roda localmente no **ADK Web**.

## Context

* **Quem é você:** um engenheiro de software especialista em IA e produtos digitais, fazendo pair programming com quem está desenvolvendo o agente. Explique **o que** está fazendo e **por quê**, em português do Brasil, de forma curta e objetiva.
* **Ambiente:** Google Cloud Shell. O `agy` roda na raiz do repositório, e o projeto do agente fica em `./agente-mudanca/`, criado com as convenções do **agents-cli**.
* **Skills do ADK** ([adk.dev/skills](https://adk.dev/skills/)): carregadas com `load_skill_from_dir` e expostas pelo `SkillToolset`, que dá ao agente as ferramentas `load_skill` e `load_skill_resource`. A skill usa divulgação progressiva:
  * **L1:** `name` e `description` no frontmatter do `SKILL.md`.
  * **L2:** o corpo do `SKILL.md`, com a regra de gating.
  * **L3:** `references/workflow.md`, `references/tecnicas.md`, `references/salvar_agenda.md`, `assets/plano_template.md` e `assets/exemplo_plano.md`.
* **EnvironmentToolset** ([adk.dev/integrations/environment-toolset](https://adk.dev/integrations/environment-toolset/)): dá ao agente ferramentas de arquivo (`ReadFile`, `WriteFile`, `EditFile`) e de shell (`Execute`) dentro de um `LocalEnvironment`. O agente usa `WriteFile` para gravar `plano-de-mudanca.csv` e `ReadFile` para conferir o arquivo antes de reescrever no replanejamento. É um recurso **experimental** (ADK Python ≥ 1.29) e emite `UserWarning` ao iniciar, o que é esperado.
* **Regra de gating da skill:** sem **título**, **descrição (contexto e impactos)**, **prazo** e **status**, o agente pergunta só o que falta e não carrega nenhum recurso L3. Com tudo em mãos, ele segue o `workflow.md` até entregar o plano no formato do `plano_template.md` e salvar o CSV com `WriteFile`.
* **Autenticação local:** API Key do Google AI Studio no arquivo `agente-mudanca/.env`:
  ```env
  GOOGLE_GENAI_USE_VERTEXAI=FALSE
  GEMINI_API_KEY=<sua-api-key>
  MODEL=gemini-3.8-flash

  * **Execução local:**
  ```bash
  cd agente-gestao
  uv run adk web --allow_origins="*"   # libera o proxy do Web Preview do Cloud Shell
  ```

## Constraints

* **Framework:** use só o **Google ADK** em Python, seguindo a especificação de ADK Skills (`load_skill_from_dir` + `SkillToolset`).
* **Divulgação progressiva:** **não** cole o conteúdo de `references/` ou `assets/` na instrução do agente. O conteúdo deve ser carregado sob demanda pelas ferramentas da skill.
* **Empacotamento da skill:** copie `skills/plano-de-gestao/` para `agente-gestao/app/skills/plano-de-gestao/`, porque o ADK resolve o caminho a partir do `agent.py`. A pasta precisa ter **exatamente** o nome do campo `name` do frontmatter. Ao alterar uma skill, mantenha as duas cópias sincronizadas.
* **Ferramentas de arquivo:** use `EnvironmentToolset(environment=LocalEnvironment(working_dir=...))`, com `working_dir` apontando para `agente-gestao/planos/` (caminho absoluto calculado a partir do `agent.py`). Sem `working_dir`, o ADK usa uma pasta temporária que é apagada quando o agente encerra.
* **Sem shell:** exponha só `ReadFile` e `WriteFile`. O agente não precisa executar comandos. No ADK 2.x, o `tool_filter` **não** é aplicado pelo `EnvironmentToolset`, então filtre com uma subclasse que sobrescreve `get_tools`:
  ```python
  class FerramentasDeArquivo(EnvironmentToolset):
      async def get_tools(self, readonly_context=None):
          tools = await super().get_tools(readonly_context)
          return [t for t in tools if t.name in ("ReadFile", "WriteFile")]
  ```
* **Modelo:** leia o modelo da variável de ambiente `MODEL`, com padrão `gemini-3.8-flash`. Não deixe o nome do modelo fixo no código.
* **Data atual:** injete a data de hoje na instrução do agente, via *instruction provider* do ADK, para que ele entenda expressões como "segunda que vem".
* **Segredos:** **nunca** versione o `.env` nem a pasta `planos/` (ela guarda dados pessoais de agenda) nem escreva a API Key em código, README, testes ou logs.
* **Dependências:** gerencie com `uv` dentro de `agente-gestao/` (`uv sync`).
* **Simplicidade:** prefira código simples e legível a abstrações sofisticadas. Confirme o entendimento do pedido antes de gerar muito código.
* **Fora de escopo:** deploy no Agent Runtime, Gemini Enterprise e infraestrutura (Terraform, CI/CD). Não gere esses artefatos, a menos que isso seja pedido explicitamente.

## Done-when

- [ ] `agente-gestao/app/agent.py` define um `root_agent` com o `SkillToolset` carregando `app/skills/plano-de-gestao` e o `EnvironmentToolset` expondo só `ReadFile` e `WriteFile`.
- [ ] O modelo vem da variável `MODEL`, e a instrução do agente contém a data de hoje.
- [ ] `uv sync` e `uv run adk web --allow_origins="*"` rodam sem erros, e o agente `app` aparece no ADK Web.
- [ ] **Gating:** com a mensagem "Quero organizar minha mudança de JavaScript.", o agente pede só as informações que faltam e **não** chama `load_skill_resource`.
- [ ] **Plano completo:** com todas as informações, o agente carrega `workflow.md`, `tecnicas.md`, `plano_template.md` e `salvar_agenda.md`, entrega módulos, cronograma e checkpoints e chama `WriteFile`, criando `agente-gestao/planos/plano-de-gestao.csv` com um CSV válido para importação no Google Agenda.
- [ ] **Replanejamento:** quando o usuário informa sessões perdidas, o agente devolve a tabela atualizada e sobrescreve o `plano-de-gestao.csv`, sem empilhar o conteúdo atrasado em um único dia.
- [ ] Nenhum `.env`, API Key ou arquivo de `planos/` aparece em `git status` nem em arquivos versionados.