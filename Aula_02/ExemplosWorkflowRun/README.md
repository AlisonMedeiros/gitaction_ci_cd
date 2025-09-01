# Gatilho: Workflow Run

O gatilho `workflow_run` é a ferramenta essencial para **encadear workflows** no GitHub Actions. Ele permite que um workflow seja iniciado automaticamente com base no resultado (como sucesso ou falha) de outro workflow no mesmo repositório.

Essa capacidade de orquestração é a base para pipelines de CI/CD robustos e desacoplados.

**Principais Casos de Uso:**
- **CI/CD Clássico:** Um workflow de CI (build, teste, lint) é executado em um Pull Request. Após o merge, um `push` na branch `main` executa o mesmo workflow de CI. Se ele passar, o `workflow_run` aciona um segundo workflow, de CD, que faz o deploy para produção.
- **Notificações Centralizadas:** Um único workflow de notificação que é acionado pela conclusão de vários outros pipelines, informando o status para uma equipe no Slack ou Teams.
- **Pós-processamento:** Após um workflow gerar artefatos, um segundo workflow é acionado para baixar esses artefatos, processá-los e publicá-los em um registro ou storage.

- Workflow run documentação

## Exemplos
- [1_basic_on_workflow_run.yml](1_basic_on_workflow_run.yml): Demonstra como reagir de forma diferente ao **sucesso** ou **falha** de outro workflow.
- [2_types_on_workflow_run.yml](2_types_on_workflow_run.yml): Mostra como executar ações quando um workflow é **solicitado** (`requested`) e quando é **concluído** (`completed`).
- [3_branch_on_workflow_run.yml](3_branch_on_workflow_run.yml): Um exemplo crucial de CI/CD, acionando um deploy apenas quando o CI passa na branch `main`.
- [4_workflows_on_workflow_run.yml](4_workflows_on_workflow_run.yml): Demonstra como um único workflow pode ser acionado por **múltiplos** workflows de origem.

## Como Testar os Exemplos

Para testar, você precisa de um "workflow de origem" que irá acionar os exemplos desta pasta. Vamos usar o "Meu Primeiro Workflow de CI" (criado no exercício da Aula 01) como origem.

1.  **Verifique o Workflow de Origem:** Certifique-se de que você tem um workflow (ex: `primeiro-workflow.yml`) com o nome `name: Meu Primeiro Workflow de CI`. Se o nome for diferente, ajuste a chave `workflows:` nos arquivos de exemplo.

2.  **Acione o Workflow de Origem:** Faça um `push` para a branch `main` do seu repositório.
    ```bash
    git commit --allow-empty -m "chore: Trigger CI workflow"
    git push origin main
    ```
3.  **Observe a Execução em Cadeia:**
    - Vá para a aba **"Actions"** do seu repositório.
    - Você verá o "Meu Primeiro Workflow de CI" sendo executado.
    - Assim que ele for concluído, os workflows desta pasta (como "1. Basic Workflow Run") serão acionados automaticamente. Você os verá aparecer na lista de execuções.

## Conceitos e Pontos Importantes

- **Filtros são Essenciais:** Use sempre filtros para controlar a execução:
  - `workflows: ['Nome do Workflow']`: **Obrigatório**. Especifica qual workflow de origem pode acionar este.
  - `types: [completed, requested]`: Filtra pelo estágio do ciclo de vida. O padrão é `completed`.
  - `branches: [main]`: Filtra pela branch em que o workflow de origem foi executado. Crucial para segurança.

- **Acessando Dados do Workflow de Origem:** Dentro do job, você pode usar o contexto `github.event.workflow_run` para obter informações:
  - `github.event.workflow_run.conclusion`: O resultado final (`success`, `failure`, `cancelled`). Disponível apenas no `type: completed`.
  - `github.event.workflow_run.workflow.name`: O nome do workflow que o acionou.
  - `github.event.action`: O tipo de evento que acionou (`requested` ou `completed`).

- **Limitações:**
  - Um workflow não pode acionar a si mesmo.
  - O encadeamento é limitado. Um workflow acionado por `workflow_run` não pode acionar um terceiro workflow da mesma forma se usar o `GITHUB_TOKEN` padrão.