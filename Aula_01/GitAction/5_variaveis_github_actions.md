# Variáveis no GitHub Actions

## Introdução

As variáveis no GitHub Actions permitem armazenar e reutilizar valores em seus workflows. Elas podem ser definidas em diferentes níveis e contextos.

## Tipos de Variáveis

### 1. Variáveis de Ambiente (env)

São definidas usando a seção `env` e podem ser usadas em workflows, jobs e steps.

```yaml
# Variáveis globais do workflow
env:
  NOME_PROJETO: "Meu Projeto"
  VERSAO: "1.0.0"
  AMBIENTE: "desenvolvimento"

jobs:
  meu-job:
    runs-on: ubuntu-latest
    
    # Variáveis específicas do job
    env:
      JOB_ID: "job-001"
      
    steps:
      - name: Usa variáveis
        run: |
          echo "Projeto: ${{ env.NOME_PROJETO }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "Job ID: ${{ env.JOB_ID }}"
```

### 2. Variáveis do Contexto GitHub

São variáveis automáticas fornecidas pelo GitHub Actions:

```yaml
steps:
  - name: Exibe informações do GitHub
    run: |
      echo "Repository: ${{ github.repository }}"
      echo "Branch: ${{ github.ref_name }}"
      echo "Commit: ${{ github.sha }}"
      echo "Actor: ${{ github.actor }}"
      echo "Event: ${{ github.event_name }}"
```

### 3. Variáveis do Contexto Runner

Informações sobre o ambiente de execução:

```yaml
steps:
  - name: Exibe informações do runner
    run: |
      echo "OS: ${{ runner.os }}"
      echo "Workspace: ${{ runner.workspace }}"
      echo "Temp: ${{ runner.temp }}"
```

### 4. Secrets

Variáveis sensíveis armazenadas no repositório:

```yaml
steps:
  - name: Usa secret
    run: |
      echo "Token: ${{ secrets.MY_TOKEN }}"
```

## Exemplos Práticos

### Exemplo 1: Variáveis Básicas

```yaml
name: Exemplo Variáveis Básicas

on: [push]

env:
  NODE_VERSION: "18"
  PYTHON_VERSION: "3.11"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
```

### Exemplo 2: Variáveis Dinâmicas

```yaml
env:
  DATA_BUILD: ${{ format('{0:yyyy-MM-dd}', github.event.head_commit.timestamp) }}
  BRANCH_NAME: ${{ github.ref_name }}
```

### Exemplo 3: Variáveis Condicionais

```yaml
env:
  AMBIENTE: ${{ github.ref == 'refs/heads/main' && 'produção' || 'desenvolvimento' }}
```

## Boas Práticas

1. **Use nomes descritivos**: `NOME_PROJETO` em vez de `NP`
2. **Organize por contexto**: Agrupe variáveis relacionadas
3. **Use secrets para dados sensíveis**: Nunca exponha tokens ou senhas
4. **Documente variáveis importantes**: Adicione comentários explicativos
5. **Use variáveis para valores reutilizáveis**: Evite hardcoding

## Contextos Disponíveis

### github
- `github.repository`: Nome do repositório
- `github.ref_name`: Nome da branch/tag
- `github.sha`: SHA do commit
- `github.actor`: Usuário que disparou o workflow
- `github.event_name`: Nome do evento

### runner
- `runner.os`: Sistema operacional
- `runner.workspace`: Diretório de trabalho
- `runner.temp`: Diretório temporário

### env
- Variáveis de ambiente customizadas

### secrets
- Secrets configurados no repositório

## Exercício Prático

Crie um workflow que:
1. Define variáveis para nome, versão e ambiente
2. Exibe informações do repositório
3. Cria um arquivo de configuração usando as variáveis
4. Usa variáveis em diferentes steps

```yaml
name: Exercício Variáveis

on: [workflow_dispatch]

env:
  NOME_APP: "Minha Aplicação"
  VERSAO: "1.0.0"

jobs:
  exercicio:
    runs-on: ubuntu-latest
    steps:
      - name: Exibe variáveis
        run: |
          echo "App: ${{ env.NOME_APP }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "Repo: ${{ github.repository }}"
      
      - name: Cria config
        run: |
          echo "APP_NAME=${{ env.NOME_APP }}" > .env
          echo "VERSION=${{ env.VERSAO }}" >> .env
          cat .env
```
