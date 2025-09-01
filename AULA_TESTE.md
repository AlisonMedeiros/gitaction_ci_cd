# 🧪 Branch Aula Teste

## 📋 Informações da Branch

- **Nome da Branch**: `aula-teste`
- **Criada em**: $(date)
- **Propósito**: Testes e experimentos com GitHub Actions

## 🎯 Objetivos desta Branch

Esta branch foi criada para:

1. **Testar workflows** do GitHub Actions
2. **Experimentar variáveis** e configurações
3. **Praticar commits** e pushes
4. **Aprender Git** e GitHub Actions

## 🔧 Workflows de Teste

### Workflow Básico de Teste

```yaml
name: Teste Básico - Aula Teste

on:
  push:
    branches: [ aula-teste ]
  workflow_dispatch:

env:
  NOME_BRANCH: "aula-teste"
  AMBIENTE: "teste"
  VERSAO: "0.1.0"

jobs:
  teste-basico:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Exibir informações da branch
        run: |
          echo "🎯 Branch atual: ${{ github.ref_name }}"
          echo "📁 Repositório: ${{ github.repository }}"
          echo "👤 Usuário: ${{ github.actor }}"
          echo "🔧 Ambiente: ${{ env.AMBIENTE }}"
          echo "📦 Versão: ${{ env.VERSAO }}"
          
      - name: Criar arquivo de teste
        run: |
          echo "Arquivo criado na branch: ${{ github.ref_name }}" > teste.txt
          echo "Data: $(date)" >> teste.txt
          echo "Commit: ${{ github.sha }}" >> teste.txt
          cat teste.txt
```

## 📝 Exercícios Práticos

### Exercício 1: Variáveis de Ambiente
- Criar variáveis para nome, versão e ambiente
- Usar variáveis em diferentes steps
- Exibir informações do contexto GitHub

### Exercício 2: Workflow Condicional
- Criar workflow que roda apenas nesta branch
- Usar condições baseadas na branch
- Testar diferentes cenários

### Exercício 3: Secrets e Segurança
- Configurar secrets (se necessário)
- Usar secrets de forma segura
- Validar configurações

## 🚀 Como Usar

1. **Fazer mudanças** nesta branch
2. **Testar workflows** do GitHub Actions
3. **Fazer commits** e pushes
4. **Verificar execução** no GitHub

## 📊 Status da Branch

- ✅ Branch criada com sucesso
- ✅ Arquivo de documentação criado
- 🔄 Pronto para testes e experimentos

## 🎉 Próximos Passos

1. Criar workflows de teste
2. Experimentar com variáveis
3. Fazer commits e pushes
4. Aprender com os resultados

---

**Branch**: aula-teste  
**Criada por**: GitHub Actions Assistant  
**Data**: $(date)
