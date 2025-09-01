# Gatilho: Workflow dispatch

O gatilho `workflow_dispatch` oferece o poder de iniciar um workflow manualmente, diretamente pela interface do GitHub. Ele transforma seus pipelines de automação em ferramentas sob demanda, dando controle total à equipe para executar ações críticas quando necessário. É a ponte perfeita entre a automação completa e a intervenção humana controlada.

- Workflow dispatch documentação

## Conceitos e Casos de Uso

A principal força do `workflow_dispatch` é permitir a execução de tarefas que não se encaixam no fluxo de `push` ou `pull_request`.

**Quando usar?**
- **Deploys em Produção:** Para ter um "botão de deploy" que só é acionado após todas as validações manuais e aprovações.
- **Tarefas de Manutenção:** Executar scripts que limpam caches, rodam migrações de banco de dados ou fazem backups sob demanda.
- **Execução de Testes Específicos:** Rodar suítes de testes longas ou custosas (como testes de carga) apenas quando solicitado.
- **Geração de Relatórios:** Criar relatórios de performance ou de negócios que não precisam ser gerados a cada commit.

### Parâmetros de Entrada (`inputs`)
A grande vantagem do `workflow_dispatch` é a capacidade de receber parâmetros. Ao acionar o workflow, o usuário pode preencher campos que serão usados durante a execução. Os tipos mais comuns são:
- `string`: Um campo de texto livre.
- `choice`: Uma lista de opções pré-definidas.
- `boolean`: Uma caixa de seleção (verdadeiro/falso).

## Exemplos
- [1_basic_on_workflow_dispatch.yml](1_basic_on_workflow_dispatch.yml): Exemplo básico sem parâmetros de entrada.
- [2_input_choice_on_workflow_dispatch.yml](2_input_choice_on_workflow_dispatch.yml): Demonstra o uso de um input do tipo `choice` (lista de opções).
- [3_input_string_on_workflow_dispatch.yml](3_input_string_on_workflow_dispatch.yml): Demonstra o uso de um input do tipo `string` (texto).
- [4_input_bool_on_workflow_dispatch.yml](4_input_bool_on_workflow_dispatch.yml): Demonstra o uso de um input do tipo `boolean` (verdadeiro/falso).
- [5_on_workflow_dispatch.yml](5_on_workflow_dispatch.yml): Um exemplo completo combinando múltiplos tipos de inputs.

## Como Testar os Exemplos

1.  **Navegue até a aba "Actions"** do seu repositório no GitHub.
2.  **Selecione o Workflow:** Na lista à esquerda, clique no nome do workflow que você deseja executar (ex: "5. Manual Trigger com Inputs").
3.  **Acione o Workflow:** Você verá uma mensagem "This workflow has a `workflow_dispatch` event". Clique no botão **"Run workflow"** que aparece à direita.
4.  **(Se aplicável) Preencha os Parâmetros:** Um menu dropdown aparecerá com os `inputs` definidos no arquivo. Selecione a branch e preencha os valores desejados.
5.  **Execute:** Clique no botão verde **"Run workflow"** para iniciar a execução. Você poderá acompanhar o progresso em tempo real.

## Pontos Importantes

- **Branch Padrão é Essencial:** Para que o botão "Run workflow" apareça na interface do GitHub, o arquivo do workflow contendo o gatilho `workflow_dispatch` **precisa estar na branch padrão** do repositório (geralmente `main` ou `master`).
- **Permissões:** Apenas usuários com permissão de escrita no repositório podem acionar workflows manualmente.
- **Acessando os Inputs:** Dentro do seu workflow, você pode acessar os valores dos parâmetros usando a sintaxe `${{ github.event.inputs.NOME_DO_INPUT }}`.

