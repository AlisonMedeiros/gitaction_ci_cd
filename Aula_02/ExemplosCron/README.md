# Gatilho: Schedule (Cron Job)
- [Schedule documentation](https://docs.github.com/pt/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#schedule)

## O que é o gatilho `schedule`?
O gatilho `schedule` permite que você execute um workflow em horários agendados, utilizando a sintaxe `cron`. É a ferramenta ideal para automações que precisam rodar em intervalos regulares, independentemente de interações com o código, como:
- Geração de relatórios noturnos.
- Verificações de saúde de serviços (health checks).
- Limpeza de recursos temporários.
- Sincronização de dados.
    
## Exemplos
- [1_on_cron.yml](1_on_cron.yml)

## Como testar o exemplo
Diferente de outros gatilhos, o `schedule` não é acionado por uma ação imediata como `push` ou `pull_request`. Para testá-lo:

1.  **Faça o commit e o push** do arquivo de workflow (ex: `1_on_cron.yml`) para a branch padrão do seu repositório (geralmente `main` ou `master`).
2.  **Aguarde o horário agendado**. O workflow será enfileirado para execução no horário UTC definido na expressão `cron`.
3.  **Verifique a aba "Actions"**. Após o horário agendado, navegue até a aba "Actions" do seu repositório. Você deverá ver a execução do seu workflow iniciada pelo evento "schedule".

## Entendendo a Sintaxe Cron
A expressão `cron` é uma string com 5 campos, que representam um agendamento.

┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6, onde 0 é Domingo)
│ │ │ │ │
* * * * *

## Observações Importantes
- **Fuso Horário UTC**: O GitHub Actions **sempre** interpreta a hora do `cron` em UTC. Se você está no Brasil (UTC-3), um job agendado para `15 3 * * *` (03:15 UTC) será executado às `00:15` no seu horário local.
- **Precisão do Agendamento**: A execução pode não ser exata no minuto agendado. O GitHub enfileira o job, e a execução pode sofrer um pequeno atraso dependendo da carga na plataforma.
- **Ferramenta Auxiliar**: Para montar e validar suas expressões `cron`, o site crontab.guru é um excelente recurso.
- **Frequência Mínima**: A frequência mínima para agendamentos é a cada 5 minutos.
