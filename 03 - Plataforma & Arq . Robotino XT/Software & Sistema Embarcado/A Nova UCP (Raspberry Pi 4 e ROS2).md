# Nova Unidade de Processamento: Raspberry Pi 4

O retrofit substitui a unidade AMD Geode por uma arquitetura ARM64 moderna para habilitar funcionalidades de alto nível.

## 1. Configuração do Sistema Operacional
- **SO:** Ubuntu Server 22.04 LTS.
- **Middleware:** ROS2 Humble instalado no Raspberry Pi 4.

## 2. Driver de Controle (controld2)
- **Papel do Daemon:** O `controld2` é o serviço responsável por traduzir os comandos de software em sinais para a IO Board.
- **Sincronização:** Exige configuração da UART no `/boot/config.txt` com `enable_uart=1` e `dtoverlay=miniuart-bt` para evitar interferências do Bluetooth.

## 3. Tópicos e Mensagens ROS2
- **Publicação:** Odometria e estado dos sensores (IDs binários 0x01 a 0x04).
- **Subscrição:** Comando de velocidade geométrica via tópico `/cmd_vel` (geometry_msgs/Twist).