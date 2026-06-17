# Diagnóstico de Memória Compartilhada (SHM)

Durante a fase de integração, a gestão de IPC (Inter-Process Communication) revelou-se um ponto crítico para a estabilidade do sistema.

## 1. O Erro de Inicialização
- **Mensagem:** "Detached from Gyroscope SHM".
- **Causa:** O daemon `controld2` tenta se conectar a um segmento de memória compartilhada para leitura do giroscópio. Se o serviço falha ou o sensor é inexistente, o processo entra em loop de erro.

## 2. Procedimento de Correção
- **Limpeza de Memória:** Uso do comando `ipcrm -a` para remover segmentos de memória órfãos.
- **Configuração de Daemon:** O estado ideal após a correção é o `controld2` em `active (running)`, reportando "Gyroscope SHM is NULL" sem encerrar o programa.