# 🚀 Exemplos Práticos de Workflows com Variáveis

Este arquivo contém exemplos práticos de workflows do GitHub Actions que demonstram o uso eficiente de variáveis.

## 📋 Índice

1. [Workflow Básico](#workflow-básico)
2. [Workflow com Múltiplos Jobs](#workflow-com-múltiplos-jobs)
3. [Workflow Condicional](#workflow-condicional)
4. [Workflow de Deploy](#workflow-de-deploy)
5. [Workflow com Secrets](#workflow-com-secrets)
6. [Workflow de Testes](#workflow-de-testes)

---

## 🔧 Workflow Básico

### Exemplo: Build e Teste Simples

```yaml
name: Build e Teste Básico

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  NODE_VERSION: "18"
  PYTHON_VERSION: "3.11"
  NOME_PROJETO: "Minha Aplicação"
  VERSAO: "1.0.0"

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
          
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ env.PYTHON_VERSION }}
          
      - name: Instalar dependências
        run: |
          npm install
          pip install -r requirements.txt
          
      - name: Executar testes
        run: |
          npm test
          python -m pytest
          
      - name: Build
        run: |
          npm run build
          echo "Build concluído para ${{ env.NOME_PROJETO }} v${{ env.VERSAO }}"
```

---

## 🔄 Workflow com Múltiplos Jobs

### Exemplo: CI/CD Completo

```yaml
name: CI/CD Completo

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  NOME_APLICACAO: "Minha Aplicação Web"
  VERSAO: "2.1.0"
  REGISTRY: "ghcr.io"
  IMAGE_NAME: "minha-app"

jobs:
  test:
    runs-on: ubuntu-latest
    
    env:
      NODE_VERSION: "18"
      COVERAGE_THRESHOLD: "80"
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          
      - name: Instalar dependências
        run: npm ci
        
      - name: Executar testes
        run: |
          npm run test:coverage
          echo "Cobertura de testes: ${{ env.COVERAGE_THRESHOLD }}%"
          
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/lcov.info

  build:
    needs: test
    runs-on: ubuntu-latest
    
    env:
      DOCKER_VERSION: "20.10"
      
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Docker
        uses: docker/setup-buildx-action@v3
        
      - name: Login no Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
          
      - name: Build e Push da imagem
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ env.REGISTRY }}/${{ github.repository_owner }}/${{ env.IMAGE_NAME }}:${{ env.VERSAO }}
            ${{ env.REGISTRY }}/${{ github.repository_owner }}/${{ env.IMAGE_NAME }}:latest
          labels: |
            version=${{ env.VERSAO }}
            build-date=${{ github.event.head_commit.timestamp }}
            commit=${{ github.sha }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/develop'
    
    env:
      AMBIENTE: "staging"
      REGIAO: "us-east-1"
      
    steps:
      - name: Deploy para staging
        run: |
          echo "Deployando ${{ env.NOME_APLICACAO }} v${{ env.VERSAO }}"
          echo "Ambiente: ${{ env.AMBIENTE }}"
          echo "Região: ${{ env.REGIAO }}"
          # Comandos de deploy aqui

  deploy-production:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    env:
      AMBIENTE: "produção"
      REGIAO: "us-east-1"
      
    steps:
      - name: Deploy para produção
        run: |
          echo "Deployando ${{ env.NOME_APLICACAO }} v${{ env.VERSAO }}"
          echo "Ambiente: ${{ env.AMBIENTE }}"
          echo "Região: ${{ env.REGIAO }}"
          # Comandos de deploy aqui
```

---

## 🎯 Workflow Condicional

### Exemplo: Deploy Inteligente

```yaml
name: Deploy Inteligente

on:
  push:
    branches: [ main, develop, feature/* ]
  workflow_dispatch:
    inputs:
      ambiente:
        description: 'Ambiente para deploy'
        required: true
        default: 'staging'
        type: choice
        options:
        - staging
        - production

env:
  NOME_APLICACAO: "API Backend"
  VERSAO: "3.0.0"
  
  # Variáveis condicionais baseadas na branch
  AMBIENTE: ${{ github.ref == 'refs/heads/main' && 'produção' || github.ref == 'refs/heads/develop' && 'staging' || 'desenvolvimento' }}
  REGIAO: ${{ github.ref == 'refs/heads/main' && 'us-west-2' || 'us-east-1' }}
  
  # Variáveis baseadas no input manual
  AMBIENTE_MANUAL: ${{ github.event.inputs.ambiente || 'staging' }}

jobs:
  validacao:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Validar configuração
        run: |
          echo "Aplicação: ${{ env.NOME_APLICACAO }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "Ambiente (branch): ${{ env.AMBIENTE }}"
          echo "Ambiente (manual): ${{ env.AMBIENTE_MANUAL }}"
          echo "Região: ${{ env.REGIAO }}"
          echo "Branch: ${{ github.ref_name }}"
          echo "Evento: ${{ github.event_name }}"

  deploy-staging:
    needs: validacao
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/develop' || github.event.inputs.ambiente == 'staging'
    
    env:
      AMBIENTE_DEPLOY: "staging"
      URL_API: "https://api-staging.exemplo.com"
      
    steps:
      - name: Deploy para staging
        run: |
          echo "🚀 Deploy para STAGING"
          echo "Aplicação: ${{ env.NOME_APLICACAO }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "URL: ${{ env.URL_API }}"
          echo "Região: ${{ env.REGIAO }}"

  deploy-production:
    needs: validacao
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || github.event.inputs.ambiente == 'production'
    
    env:
      AMBIENTE_DEPLOY: "produção"
      URL_API: "https://api.exemplo.com"
      
    steps:
      - name: Deploy para produção
        run: |
          echo "🚀 Deploy para PRODUÇÃO"
          echo "Aplicação: ${{ env.NOME_APLICACAO }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "URL: ${{ env.URL_API }}"
          echo "Região: ${{ env.REGIAO }}"
          
      - name: Notificar equipe
        run: |
          echo "✅ Deploy concluído com sucesso!"
          echo "📧 Enviando notificação para a equipe..."
```

---

## 🔐 Workflow com Secrets

### Exemplo: Deploy Seguro

```yaml
name: Deploy Seguro

on:
  push:
    branches: [ main ]
  workflow_dispatch:

env:
  NOME_APLICACAO: "Aplicação Segura"
  VERSAO: "1.0.0"
  AMBIENTE: "produção"

jobs:
  validacao-seguranca:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Validar secrets
        run: |
          # Verificar se secrets necessários estão configurados
          if [ -z "${{ secrets.AWS_ACCESS_KEY_ID }}" ]; then
            echo "❌ AWS_ACCESS_KEY_ID não configurado"
            exit 1
          fi
          
          if [ -z "${{ secrets.AWS_SECRET_ACCESS_KEY }}" ]; then
            echo "❌ AWS_SECRET_ACCESS_KEY não configurado"
            exit 1
          fi
          
          if [ -z "${{ secrets.DATABASE_URL }}" ]; then
            echo "❌ DATABASE_URL não configurado"
            exit 1
          fi
          
          echo "✅ Todos os secrets estão configurados"

  deploy:
    needs: validacao-seguranca
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Configurar AWS
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
          
      - name: Deploy para AWS
        run: |
          echo "🚀 Iniciando deploy seguro..."
          echo "Aplicação: ${{ env.NOME_APLICACAO }}"
          echo "Versão: ${{ env.VERSAO }}"
          echo "Ambiente: ${{ env.AMBIENTE }}"
          
          # Usar secrets de forma segura
          echo "Database configurado: ${{ secrets.DATABASE_URL != '' }}"
          echo "API Key configurada: ${{ secrets.API_KEY != '' }}"
          
          # Comandos de deploy aqui
          aws s3 sync ./dist s3://${{ secrets.S3_BUCKET }}/
          
      - name: Notificar sucesso
        if: success()
        run: |
          echo "✅ Deploy concluído com sucesso!"
          echo "📧 Enviando notificação..."
          # Usar webhook ou email com secret
          curl -X POST ${{ secrets.WEBHOOK_URL }} \
            -H "Content-Type: application/json" \
            -d '{"text":"Deploy concluído com sucesso!"}'
```

---

## 🧪 Workflow de Testes

### Exemplo: Testes Automatizados

```yaml
name: Testes Automatizados

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  NOME_PROJETO: "Projeto de Testes"
  COBERTURA_MINIMA: "80"
  TIMEOUT_TESTES: "300"

jobs:
  testes-unitarios:
    runs-on: ubuntu-latest
    
    env:
      NODE_VERSION: "18"
      TEST_ENV: "unit"
      
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          
      - name: Instalar dependências
        run: npm ci
        
      - name: Executar testes unitários
        run: |
          echo "🧪 Executando testes unitários..."
          echo "Timeout: ${{ env.TIMEOUT_TESTES }}s"
          echo "Ambiente: ${{ env.TEST_ENV }}"
          
          npm run test:unit -- --timeout ${{ env.TIMEOUT_TESTES }}000
          
      - name: Verificar cobertura
        run: |
          COBERTURA=$(npm run test:coverage --silent | grep -o '[0-9]*%' | head -1 | sed 's/%//')
          echo "Cobertura atual: ${COBERTURA}%"
          echo "Cobertura mínima: ${{ env.COBERTURA_MINIMA }}%"
          
          if [ "$COBERTURA" -lt "${{ env.COBERTURA_MINIMA }}" ]; then
            echo "❌ Cobertura insuficiente!"
            exit 1
          fi
          echo "✅ Cobertura adequada!"

  testes-integracao:
    runs-on: ubuntu-latest
    
    env:
      TEST_ENV: "integration"
      DATABASE_URL: ${{ secrets.TEST_DATABASE_URL }}
      
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup ambiente de teste
        run: |
          echo "🔧 Configurando ambiente de integração..."
          echo "Ambiente: ${{ env.TEST_ENV }}"
          echo "Database configurado: ${{ secrets.TEST_DATABASE_URL != '' }}"
          
      - name: Executar testes de integração
        run: |
          echo "🧪 Executando testes de integração..."
          npm run test:integration
          
      - name: Gerar relatório
        run: |
          echo "📊 Gerando relatório de testes..."
          npm run test:report
          
      - name: Upload relatório
        uses: actions/upload-artifact@v3
        with:
          name: relatorio-testes
          path: ./reports/

  testes-e2e:
    runs-on: ubuntu-latest
    
    env:
      TEST_ENV: "e2e"
      BASE_URL: "https://staging.exemplo.com"
      
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Playwright
        uses: microsoft/playwright-github-action@v1
        
      - name: Executar testes E2E
        run: |
          echo "🧪 Executando testes E2E..."
          echo "URL Base: ${{ env.BASE_URL }}"
          echo "Ambiente: ${{ env.TEST_ENV }}"
          
          npx playwright test
          
      - name: Upload screenshots
        uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: screenshots-e2e
          path: playwright-report/
```

---

## 📊 Workflow de Relatórios

### Exemplo: Geração de Relatórios

```yaml
name: Relatórios Automatizados

on:
  schedule:
    - cron: '0 8 * * 1'  # Toda segunda-feira às 8h
  workflow_dispatch:

env:
  NOME_PROJETO: "Sistema de Relatórios"
  PERIODO: ${{ format('{0:yyyy-MM}', github.event.head_commit.timestamp) }}

jobs:
  gerar-relatorio:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Gerar relatório
        run: |
          echo "📊 Gerando relatório do período: ${{ env.PERIODO }}"
          echo "Projeto: ${{ env.NOME_PROJETO }}"
          echo "Data: $(date)"
          
          # Gerar relatório aqui
          python scripts/gerar_relatorio.py --periodo ${{ env.PERIODO }}
          
      - name: Enviar relatório
        run: |
          echo "📧 Enviando relatório..."
          # Enviar por email ou webhook
          curl -X POST ${{ secrets.REPORT_WEBHOOK }} \
            -H "Content-Type: application/json" \
            -d "{\"text\":\"Relatório ${{ env.PERIODO }} gerado com sucesso!\"}"
```

---

## 🎉 Conclusão

Estes exemplos demonstram como usar variáveis de forma eficiente no GitHub Actions para:

- ✅ Automatizar processos
- ✅ Manter configurações centralizadas
- ✅ Criar workflows dinâmicos
- ✅ Implementar segurança
- ✅ Gerar relatórios

Lembre-se de adaptar estes exemplos às necessidades específicas do seu projeto!
