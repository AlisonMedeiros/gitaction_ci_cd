# Exercício 01: Criando Nosso Primeiro Pipeline de CI

## Objetivo

O objetivo deste exercício é criar nosso primeiro workflow de Integração Contínua (CI) utilizando GitHub Actions. Vamos construir um pipeline simples que é acionado automaticamente sempre que enviamos código (`push`) para a branch `main`.

Este workflow irá executar algumas tarefas básicas para nos familiarizarmos com a estrutura de um arquivo de Actions e com o ambiente de execução.

---

## Preparação do Repositório

Para este exercício, você pode utilizar um repositório que já possua no GitHub ou criar um novo. Caso precise criar um, siga as instruções vistas na aula de  revisão de GitHub.

## Construindo o Workflow

Sua tarefa é criar um arquivo de workflow que execute as ações descritas abaixo.

1.  **Crie a estrutura de diretórios**: Na raiz do seu repositório, crie o diretório `.github/` e, dentro dele, o diretório `workflows/`.
2.  **Crie o arquivo de workflow**: Dentro de `.github/workflows/`, crie um novo arquivo com a extensão `.yml` (por exemplo, `primeiro-workflow.yml`).
3.  **Defina a estrutura do workflow**: Dentro do arquivo, construa seu workflow com as seguintes especificações:
    *   **`name`**: Dê um nome descritivo para o seu workflow, como "Meu Primeiro Workflow de CI".
    *   **`on`**: Configure o gatilho (`trigger`) para que o workflow seja executado em duas situações:
        *   Quando houver um `push` para a branch `main`.
        *   Manualmente, através do `workflow_dispatch`.
    *   **`jobs`**: Crie um único job. Você pode chamá-lo de `build` ou `meu-primeiro-job`.
        *   **`runs-on`**: Especifique que este job deve rodar em uma máquina virtual Linux, utilizando `ubuntu-latest`.
        *   **`steps`**: Defina uma sequência de três passos (`steps`):
            1.  O primeiro passo deve ter um nome (`name`) como "Imprime mensagem de boas-vindas" e usar o comando `run` para imprimir uma mensagem na tela (ex: `echo "Iniciando meu primeiro job!"`).
            2.  O segundo passo deve ter um nome como "Mostra o diretório de trabalho" e usar o comando `run` para executar `pwd`.
            3.  O terceiro passo deve ter um nome como "Lista arquivos no runner" e usar o comando `run` para executar `ls -la`.

> **Dica do Professor:** Lembre-se da estrutura de um arquivo YAML. A indentação (espaçamento no início da linha) é crucial para que o GitHub Actions entenda a hierarquia entre `jobs`, `steps`, etc.

---

## Verificando o Resultado

Agora, vamos ver a automação em ação!

1.  Adicione o novo arquivo ao Git: `git add .github/workflows/seu-arquivo.yml`
2.  Faça o commit da alteração: `git commit -m "feat: Adiciona primeiro workflow de CI"`
3.  Envie a alteração para o GitHub: `git push origin main`
4.  Abra seu repositório no navegador e clique na aba **Actions**.

Você verá seu workflow "01 - Nosso Primeiro Workflow" em execução. Clique nele para explorar os logs de cada passo (`step`) e veja as mensagens que definimos sendo impressas no console! Parabéns, você acabou de criar e executar seu primeiro pipeline de CI!