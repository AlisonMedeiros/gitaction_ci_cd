# Gatilho: Push

O gatilho `push` é um dos mais fundamentais no universo do GitHub Actions. Ele aciona um workflow sempre que um ou mais commits são enviados (`git push`) para uma branch ou tag no repositório.

Este gatilho é a base para muitos fluxos de CI/CD, permitindo automações como:
- **Build e Teste**: Compilar o código e rodar testes unitários a cada novo commit em uma branch de desenvolvimento.
- **Análise de Código**: Executar linters e ferramentas de análise estática para garantir a qualidade do código.
- **Deploy Contínuo**: Publicar automaticamente em um ambiente de `staging` ou `develop` quando a branch correspondente é atualizada.

- [Push documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#push)

## Exemplos
- [1_basic_on_push.yml](1_basic_on_push.yml): Dispara em qualquer `push` para qualquer branch.
- [2_branch_on_push.yml](2_branch_on_push.yml): Dispara apenas em `push` para a branch `main`.
- [3_branch_ignore_on_push.yml](3_branch_ignore_on_push.yml): Dispara em `push` para qualquer branch, *exceto* `main`.
- [4_path_on_push.yml](4_path_on_push.yml): Dispara apenas se o `push` modificar arquivos dentro do diretório `docs/`.
- [5_on_push.yml](5_on_push.yml): Um exemplo combinado, usando filtros de branch e path.

## Como Testar os Exemplos

Para ver os workflows de `push` em ação, basta fazer um commit e enviá-lo para o repositório no GitHub.

1.  **Faça uma alteração**: Modifique qualquer arquivo no seu clone local do repositório. Por exemplo, adicione uma linha a este `README.md`.

2.  **Faça o commit da alteração**:
    ```bash
    # Adicione o arquivo modificado
    git add .

    # Crie o commit
    git commit -m "docs: Testa workflow de push"
    ```

3.  **Envie a alteração para o GitHub**:
    ```bash
    # Envie os commits para a branch atual no repositório remoto
    git push
    ```

4.  **Observe a execução**: Vá para a aba "Actions" do seu repositório no GitHub. Você verá o workflow sendo executado para o seu último push.

## Pontos-chave e Boas Práticas

- **Use Filtros para Otimizar**: Evite executar workflows desnecessariamente. Use `branches`, `tags` e `paths` para acionar a automação apenas quando for relevante. Isso economiza tempo e custos de execução.
- **`branches` vs. `branches-ignore`**: Você não pode usar `branches` e `branches-ignore` no mesmo gatilho `push`. Escolha a estratégia que for mais clara para o seu caso de uso.
- **Cuidado com Múltiplos Commits**: Se você enviar vários commits de uma vez (`git push`), o workflow será acionado apenas uma vez, para o commit mais recente (o `HEAD` da branch).
- **Gatilhos de Tags**: Você pode usar `tags` e `tags-ignore` para acionar workflows em `push` de tags (ex: `git push --tags`), o que é muito comum para criar releases.

