# Comunicação Serial: RPi4 ↔ Transceptor ↔ IO Board

Com a substituição da UCP, a comunicação passou a ser feita diretamente via hardware através do protocolo serial.

## 1. Camada Física e Níveis Lógicos
- **Raspberry Pi (UART):** Opera em TTL 3.3V (Pinos GPIO 14/TX e 15/RX).
- **Transceptor (MAX3232/SP3232):** Realiza a conversão de nível para o padrão RS-232 da IO Board.
- **Pinagem Crítica:** - TX do RPi -> RX do Módulo.
    - TX do Módulo (RS232) -> Pino 3 da IO Board.
    - RX do Módulo (RS232) -> Pino 5 da IO Board.

## 2. Parâmetros de Sincronização
- **Baudrate:** **12340 baud** (Configuração não padrão e obrigatória para o firmware 2.4).
- **Bit de Parada:** 1 stop bit.
- **Paridade:** None.

## 3. Ferramentas de Teste
- **Minicom:** Utilizado para enviar comandos de baixo nível (ex: `version`) diretamente para a placa.
- **Comando:** `sudo minicom -b 12340 -D /dev/serial0`.