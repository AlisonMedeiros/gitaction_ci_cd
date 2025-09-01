# Aula 02: Gatilhos de Execução (Triggers)

## O que são Triggers e por que são importantes?

Em GitHub Actions, um **gatilho** (ou *trigger*) é o evento específico que inicia a execução de um workflow. Pense nele como o interruptor que liga a automação. Sem um gatilho, nosso pipeline seria apenas um conjunto de instruções sem saber quando agir.

A escolha correta dos gatilhos é fundamental para construir um processo de CI/CD eficiente. Eles garantem que as automações certas aconteçam no momento certo, seja para verificar a qualidade de um novo código, implantar uma versão em produção ou rodar tarefas de manutenção.

---

## Como os Triggers são Utilizados?

No arquivo de workflow `.yml`, os gatilhos são definidos sob a chave principal `on`. Você pode configurar um ou múltiplos eventos para iniciar seu workflow.

# Documentação de triggers
- [Triggers documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)

### Principais Tipos de Gatilhos
Vamos explorar os gatilhos mais comuns que você usará no dia a dia.
#### 1. `push`
- [Push documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#push)

## O que é push?
    É um comando fundamental para enviar seus commits locais para um repositório remoto

## Exemplos
- [1_basic_on_push.yml](ExemplosPush/1_basic_on_push.yml)
- [2_branch_on_push.yml](ExemplosPush/2_branch_on_push.yml)
- [3_branch_ignore_on_push.yml](ExemplosPush/3_branch_ignore_on_push.yml)
- [4_path_on_push.yml](ExemplosPush/4_path_on_push.yml)
- [5_on_push.yml](ExemplosPush/5_on_push.yml)

## Observações importantes
- "branches" e "branches-ignore" não podem ser configurados no mesmo arquivo .ymlEste é o gatilho mais fundamental. Ele aciona o workflow sempre que um commit é enviado (`git push`) para o repositório. É comum filtrar para que ele atue apenas em branches específicas.

#### 2. `pull_request`
Essencial para a Integração Contínua (CI). Este gatilho executa o workflow quando um Pull Request (PR) é aberto, atualizado ou fechado. É perfeito para rodar testes e validações *antes* que o código seja mesclado na branch principal.
- [Pull Request documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#pull_request)

## Exemplos
- [1_basic_on_pull_request.yml](ExemplosPullRequest/1_basic_on_pull_request.yml)
- [2_branch_on_pull_request.yml](ExemplosPullRequest/2_branch_on_pull_request.yml)
- [3_type_on_pull_request.yml](ExemplosPullRequest/3_type_on_pull_request.yml)
- [4_on_pull_request.yml](ExemplosPullRequest/4_on_pull_request.yml)

#### 3. `workflow_dispatch`
Permite que o workflow seja acionado manualmente a partir da aba "Actions" no GitHub. É extremamente útil para tarefas que não devem ser automáticas, como o deploy para um ambiente de produção ou a execução de um teste complexo sob demanda.

- [Workflow dispatch documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#workflow_dispatch)

## Exemplos
- [1_basic_on_workflow_dispatch.yml](ExemploWorkflowDispatch/1_basic_on_workflow_dispatch.yml)
- [2_input_choice_on_workflow_dispatch.yml](ExemploWorkflowDispatch/2_input_choice_on_workflow_dispatch.yml)
- [3_input_string_on_workflow_dispatch.yml](ExemploWorkflowDispatch/3_input_string_on_workflow_dispatch.yml)
- [4_input_bool_on_workflow_dispatch.yml](ExemploWorkflowDispatch/4_input_bool_on_workflow_dispatch.yml)
- [5_on_workflow_dispatch.yml](ExemploWorkflowDispatch/5_on_workflow_dispatch.yml)

#### 4. `schedule`
Executa o workflow em um horário agendado, utilizando a sintaxe cron. Ideal para tarefas recorrentes, como builds noturnos, geração de relatórios ou varreduras de segurança.

- [Workflow dispatch documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#schedule)

## Exemplos
- [1_on_cron.yml](ExemplosCron/1_on_cron.yml)

---

## Explorando Mais a Fundo

Existem dezenas de outros eventos que podem ser usados como gatilhos, como a criação de uma `issue`, a publicação de uma `release`, um comentário em um PR e muito mais.

Para uma lista completa e detalhada de todos os gatilhos disponíveis, a documentação oficial do GitHub é o melhor recurso:

- [Documentação Oficial de Eventos que Acionam Workflows](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)


