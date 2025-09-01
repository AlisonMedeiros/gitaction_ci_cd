# Gatilho: Pull Request

O gatilho `pull_request` é, sem dúvida, um dos pilares da Integração Contínua (CI). Ele permite que executemos automações cruciais no momento em que uma nova contribuição de código é proposta, agindo como um verdadeiro guardião da qualidade da nossa branch principal.
- [Pull Request documentação](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#pull_request)

## O que é um Pull Request no contexto de CI?

Um Pull Request (PR) é mais do que uma simples "solicitação para merge". É um fórum de discussão e validação de código. No contexto de CI, ele se torna o ponto de verificação automático onde garantimos que as novas alterações:

- **Passam nos testes**: Executamos testes unitários, de integração e E2E.
- **Seguem os padrões de código**: Rodamos linters e formatadores para garantir a consistência.
- **Não introduzem vulnerabilidades**: Ferramentas de análise de segurança (SAST) podem ser acionadas.

Ao automatizar essas verificações, a equipe ganha agilidade e segurança, pois a revisão humana pode focar na lógica de negócio, e não em erros básicos.

## Exemplos
- [1_basic_on_pull_request.yml](1_basic_on_pull_request.yml)
- [2_branch_on_pull_request.yml](2_branch_on_pull_request.yml)
- [3_type_on_pull_request.yml](3_type_on_pull_request.yml)
- [4_on_pull_request.yml](4_on_pull_request.yml)

## Otimizando a Execução com Filtros

Assim como no gatilho `push`, podemos usar filtros para otimizar quando o workflow de PR deve rodar, economizando recursos e tempo.
- **`paths` / `paths-ignore`**: Execute o workflow apenas se arquivos em um determinado diretório forem modificados. Por exemplo, rodar testes de backend somente se arquivos em `src/api/**` forem alterados.
- **`branches` / `branches-ignore`**: Execute o workflow apenas para PRs direcionados a branches específicas, como `main` ou `develop`.

## Como Testar os Exemplos

Para ver os workflows de `pull_request` em ação, você precisa criar um Pull Request no repositório. Siga os passos abaixo:

1.  **Crie uma nova branch**: A partir da branch `main`, crie uma branch para sua alteração.
    ```bash
    git checkout -b minha-feature-pr
    ```
2.  **Faça uma alteração**: Modifique ou crie um arquivo. Por exemplo, adicione um comentário em qualquer `README.md`.

3.  **Faça o commit da alteração**:
    ```bash
    git add .
    git commit -m "feat: Testa workflow de pull request"
    ```
4.  **Envie a nova branch para o GitHub**:
    ```bash
    git push origin minha-feature-pr
    ```
5.  **Abra um Pull Request**:
    - Vá para a página do seu repositório no GitHub.
    - O GitHub geralmente mostrará um aviso para criar um PR a partir da sua nova branch. Clique em "Compare & pull request".
    - Certifique-se de que a branch base (`base`) é a `main` e a branch de comparação (`compare`) é a `minha-feature-pr`.
    - Clique em "Create pull request".

6.  **Observe a execução**: Ao criar o PR, os workflows configurados com o gatilho `pull_request` (que apontam para a branch `main`) serão iniciados. Você pode acompanhar o progresso na aba "Actions" ou diretamente na página do Pull Request, na seção de *checks*.
