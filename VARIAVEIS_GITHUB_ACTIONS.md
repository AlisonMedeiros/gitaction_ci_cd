# 📋 Guia Completo: Variáveis no GitHub Actions

## 🎯 Objetivo

Este documento fornece um guia completo sobre como usar variáveis no GitHub Actions, incluindo exemplos práticos e melhores práticas.

## 📚 Índice

1. [Introdução](#introdução)
2. [Tipos de Variáveis](#tipos-de-variáveis)
3. [Contextos Disponíveis](#contextos-disponíveis)
4. [Exemplos Práticos](#exemplos-práticos)
5. [Boas Práticas](#boas-práticas)
6. [Troubleshooting](#troubleshooting)

---

## 🚀 Introdução

As variáveis no GitHub Actions são essenciais para criar workflows dinâmicos e reutilizáveis. Elas permitem:

- ✅ Armazenar valores reutilizáveis
- ✅ Personalizar comportamentos
- ✅ Manter configurações centralizadas
- ✅ Evitar hardcoding de valores

---

## 🔧 Tipos de Variáveis

### 1. Variáveis de Ambiente (env)

**Definição**: Variáveis customizadas definidas pelo usuário.

**Escopo**: Workflow, Job ou Step.

```yaml
# Nível Workflow
env:
  NOME_PROJETO: "Meu Projeto"
  VERSAO: "1.0.0"
  AMBIENTE: "desenvolvimento"

jobs:
  build:
    runs-on: ubuntu-latest
    
    # Nível Job
    env:
      JOB_ID: "build-001"
      
    steps:
      - name: Exemplo
        # Nível Step
        env:
          STEP_NAME: "build-step"
        run: |
          echo "Projeto: ${{ env.NOME_PROJETO }}"
          echo "Job: ${{ env.JOB_ID }}"
          echo "Step: ${{ env.STEP_NAME }}"
```

### 2. Variáveis do Contexto GitHub

**Definição**: Variáveis automáticas fornecidas pelo GitHub.

```yaml
steps:
  - name: Informações do GitHub
    run: |
      echo "Repository: ${{ github.repository }}"
      echo "Branch: ${{ github.ref_name }}"
      echo "Commit: ${{ github.sha }}"
      echo "Actor: ${{ github.actor }}"
      echo "Event: ${{ github.event_name }}"
      echo "Workflow: ${{ github.workflow }}"
      echo "Run ID: ${{ github.run_id }}"
```

### 3. Variáveis do Contexto Runner

**Definição**: Informações sobre o ambiente de execução.

```yaml
steps:
  - name: Informações do Runner
    run: |
      echo "OS: ${{ runner.os }}"
      echo "Architecture: ${{ runner.arch }}"
      echo "Workspace: ${{ runner.workspace }}"
      echo "Temp: ${{ runner.temp }}"
      echo "Tool Cache: ${{ runner.tool_cache }}"
```

### 4. Secrets

**Definição**: Variáveis sensíveis armazenadas no repositório.

```yaml
steps:
  - name: Usa Secret
    run: |
      echo "Token configurado: ${{ secrets.MY_TOKEN != '' }}"
      # Nunca exiba o valor do secret diretamente
```

---

## 🌐 Contextos Disponíveis

### Contexto `github`

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `github.repository` | Nome do repositório | `owner/repo` |
| `github.ref_name` | Nome da branch/tag | `main` |
| `github.sha` | SHA do commit | `abc123...` |
| `github.actor` | Usuário que disparou | `username` |
| `github.event_name` | Nome do evento | `push` |
| `github.workflow` | Nome do workflow | `CI` |
| `github.run_id` | ID da execução | `1234567890` |
| `github.run_number` | Número da execução | `42` |

### Contexto `runner`

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `runner.os` | Sistema operacional | `Linux` |
| `runner.arch` | Arquitetura | `X64` |
| `runner.workspace` | Diretório de trabalho | `/home/runner/work/repo` |
| `runner.temp` | Diretório temporário | `/home/runner/_temp` |
| `runner.tool_cache` | Cache de ferramentas | `/opt/hostedtoolcache` |

### Contexto `env`

Variáveis de ambiente customizadas definidas pelo usuário.

### Contexto `secrets`

Secrets configurados no repositório ou organização.

---

## 💡 Exemplos Práticos

### Exemplo 1: Workflow com Variáveis Básicas

```yaml
name: Exemplo Básico

on: [push]

env:
  NODE_VERSION: "18"
  PYTHON_VERSION: "3.11"
  AMBIENTE: "desenvolvimento"

jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ env.PYTHON_VERSION }}
          
      - name: Exibe configuração
        run: |
          echo "Node.js: ${{ env.NODE_VERSION }}"
          echo "Python: ${{ env.PYTHON_VERSION }}"
          echo "Ambiente: ${{ env.AMBIENTE }}"
```

### Exemplo 2: Variáveis Dinâmicas

```yaml
env:
  DATA_BUILD: ${{ format('{0:yyyy-MM-dd}', github.event.head_commit.timestamp) }}
  BRANCH_NAME: ${{ github.ref_name }}
  AMBIENTE: ${{ github.ref == 'refs/heads/main' && 'produção' || 'desenvolvimento' }}
```

### Exemplo 3: Criação de Arquivo de Configuração

```yaml
steps:
  - name: Cria config.json
    run: |
      cat > config.json << EOF
      {
        "aplicacao": "${{ env.NOME_APLICACAO }}",
        "versao": "${{ env.VERSAO }}",
        "ambiente": "${{ env.AMBIENTE }}",
        "repositorio": "${{ github.repository }}",
        "branch": "${{ github.ref_name }}",
        "commit": "${{ github.sha }}",
        "executado_por": "${{ github.actor }}",
        "data_build": "${{ env.DATA_BUILD }}"
      }
      EOF
      
      echo "Configuração criada:"
      cat config.json
```

### Exemplo 4: Variáveis Condicionais

```yaml
env:
  # Define ambiente baseado na branch
  AMBIENTE: ${{ github.ref == 'refs/heads/main' && 'produção' || 'desenvolvimento' }}
  
  # Define versão baseada no evento
  VERSAO: ${{ github.event_name == 'release' && github.event.release.tag_name || 'dev' }}

jobs:
  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy para produção
        run: |
          echo "Deployando ${{ env.NOME_APLICACAO }} v${{ env.VERSAO }}"
          echo "Ambiente: ${{ env.AMBIENTE }}"
```

---

## ✅ Boas Práticas

### 1. Nomenclatura
```yaml
# ✅ Bom
env:
  NOME_PROJETO: "Meu Projeto"
  VERSAO_APLICACAO: "1.0.0"
  
# ❌ Evite
env:
  NP: "Meu Projeto"
  V: "1.0.0"
```

### 2. Organização
```yaml
env:
  # Configurações da aplicação
  NOME_APLICACAO: "Minha App"
  VERSAO: "1.0.0"
  
  # Configurações de ambiente
  NODE_VERSION: "18"
  PYTHON_VERSION: "3.11"
  
  # Configurações de deploy
  AMBIENTE: "desenvolvimento"
  REGIAO: "us-east-1"
```

### 3. Segurança
```yaml
# ✅ Use secrets para dados sensíveis
steps:
  - name: Deploy
    run: |
      echo "Token: ${{ secrets.DEPLOY_TOKEN }}"
      
# ❌ Nunca exponha dados sensíveis
env:
  TOKEN: "abc123"  # NUNCA faça isso!
```

### 4. Documentação
```yaml
env:
  # Versão do Node.js para o projeto
  NODE_VERSION: "18"
  
  # Ambiente de execução (dev/staging/prod)
  AMBIENTE: "desenvolvimento"
  
  # Região AWS para deploy
  REGIAO_AWS: "us-east-1"
```

---

## 🔍 Troubleshooting

### Problema: Variável não encontrada
```yaml
# ❌ Erro
run: echo "Versão: ${{ env.VERSAO }}"

# ✅ Solução: Verifique se a variável está definida
run: |
  if [ -n "${{ env.VERSAO }}" ]; then
    echo "Versão: ${{ env.VERSAO }}"
  else
    echo "Versão não definida"
  fi
```

### Problema: Variável vazia
```yaml
# ✅ Verificação de variável vazia
run: |
  if [ -z "${{ env.MINHA_VAR }}" ]; then
    echo "Variável MINHA_VAR está vazia"
    exit 1
  fi
```

### Problema: Escape de caracteres
```yaml
# ✅ Escape correto
run: |
  echo "Path: \${{ env.PATH }}"
  echo "Branch: ${{ github.ref_name }}"
```

---

## 📝 Exercícios Práticos

### Exercício 1: Workflow Básico
Crie um workflow que:
1. Define variáveis para nome, versão e ambiente
2. Exibe informações do repositório
3. Cria um arquivo `.env` com as variáveis

### Exercício 2: Workflow Condicional
Crie um workflow que:
1. Define ambiente baseado na branch
2. Usa variáveis condicionais
3. Executa steps diferentes por ambiente

### Exercício 3: Workflow com Secrets
Crie um workflow que:
1. Usa secrets para autenticação
2. Valida se secrets estão configurados
3. Executa deploy seguro

---

## 📚 Recursos Adicionais

- [Documentação Oficial GitHub Actions](https://docs.github.com/en/actions)
- [Contextos e Sintaxe](https://docs.github.com/en/actions/learn-github-actions/contexts)
- [Variáveis de Ambiente](https://docs.github.com/en/actions/learn-github-actions/environment-variables)
- [Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)

---

## 🤝 Contribuição

Para contribuir com este guia:
1. Faça um fork do repositório
2. Crie uma branch para sua feature
3. Adicione seus exemplos e melhorias
4. Faça um pull request

---

**Última atualização**: $(date)
**Versão**: 1.0.0
