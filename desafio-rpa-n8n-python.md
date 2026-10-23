# Desafio Criativo DIO — Criando um Processo de RPA com N8N e Python

## Prompt Final

Atue como um especialista em automação de processos (RPA) utilizando N8N e Python.

Crie uma automação para processar diariamente arquivos de medição enviados por parceiros, validar os dados, consolidar em um banco de dados e alertar a equipe de operações sobre anomalias.

Público:
Equipe de operações e analistas de dados.

Ferramentas envolvidas:
N8N (orquestrador), Python (processamento), Google Drive (origem dos arquivos), PostgreSQL (destino) e Slack (alertas).

Fluxo:
1. O N8N monitora uma pasta no Google Drive a cada hora em busca de novos arquivos `.csv` ou `.xlsx`.
2. Ao detectar um arquivo, o N8N o repassa para um nó de código Python (Code Node).
3. O script Python lê o arquivo com pandas, remove duplicatas, formata datas no padrão ISO, converte valores numéricos e verifica se há campos obrigatórios vazios.
4. O Python retorna um JSON limpo com os registros válidos e uma lista separada de erros encontrados.
5. O N8N insere os registros válidos no PostgreSQL em lote.
6. Se a lista de erros não estiver vazia, o N8N envia uma notificação no Slack com o resumo dos problemas.
7. Se alguma medição ultrapassar um limite pré-configurado, o N8N dispara um alerta adicional no canal de operações.

Regras:
- Arquivos com mais de 10 MB devem ser ignorados e reportados no Slack.
- Registros com `valor_medido` nulo ou negativo devem ser descartados e contabilizados como erro.
- O limite de alerta é: `valor_medido > 100.0` (configurável via variável de ambiente).
- O fluxo deve ser idempotente: se o mesmo arquivo for processado duas vezes, não deve duplicar registros no banco.
- Erros de conexão com o banco ou com o Slack devem acionar uma retentativa (máximo 3 tentativas) antes de falhar definitivamente.

Explique quais nós do N8N devem ser utilizados, como o código Python se encaixa no fluxo e qual a lógica de funcionamento do workflow como um todo.
