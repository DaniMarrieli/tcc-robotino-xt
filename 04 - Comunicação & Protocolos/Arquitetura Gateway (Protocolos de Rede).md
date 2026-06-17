# Protocolos na Tentativa de Arquitetura Gateway

Durante a fase em que o Raspberry Pi 4 atuava como Gateway, foram testados protocolos de aplicação para a ponte de dados.

## 1. TCP/IP e UDP
- **TCP (Porta 10001):** Utilizado para o fluxo de dados que exige integridade (comandos críticos).
- **UDP:** Testado para o envio de odometria em alta frequência para reduzir a latência de rede.

## 2. REST API (rpcd)
- **Porta:** 12080.
- **Função:** Interface baseada em HTTP para comandos e leituras rápidas. No modo Gateway, o Raspberry Pi tentava consumir esses endpoints para traduzi-los para tópicos ROS2.
- **Insucesso:** A latência acumulada entre as chamadas REST e o processamento no ROS2 foi um dos fatores para a mudança para a comunicação serial direta.