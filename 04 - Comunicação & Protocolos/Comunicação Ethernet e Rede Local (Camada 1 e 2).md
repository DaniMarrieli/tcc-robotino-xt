# Comunicação Ethernet e Configuração de IP

No início do projeto, a comunicação primária para diagnóstico e revitalização foi estabelecida via interface física Ethernet (RJ45).

## 1. Conexão Física
- **Cabo Ethernet:** Utilizado para conectar o Robotino XT diretamente ao notebook de desenvolvimento no Laboratório 2314.
- **Interface no Robotino:** `eth0`.

## 2. Configuração de IP e Robotino View
- **IP Padrão (Exemplo):** `172.26.1.1` (conforme sub-rede padrão da Festo).
- **Robotino View:** Software utilizado para validar a conexão. O IP deve ser inserido no campo de conexão do software para que as portas de telemetria e controle sejam abertas.
- **Ping Test:** Ferramenta essencial para validar a latência da rede local antes de iniciar qualquer serviço de controle.