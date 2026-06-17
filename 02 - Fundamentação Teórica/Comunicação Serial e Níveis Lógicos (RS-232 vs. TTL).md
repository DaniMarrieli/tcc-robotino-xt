# Comunicação Serial e Integração de Níveis Lógicos

## O Problema dos Níveis Elétricos
- **Raspberry Pi 4:** Opera com lógica **TTL 3.3V**.
- **IO Board (Robotino):** Opera com o padrão industrial **RS-232** (tensões que podem variar de -12V a +12V).

## O Transceptor (MAX232 / SP3232)
Para conectar os dois sem queimar o Raspberry Pi, é obrigatório o uso de um chip conversor. 
- **Função:** Ele inverte e ajusta as tensões para que a UART do RPi (`/dev/serial0`) possa "conversar" com a porta serial da placa de controle do robotino.
- **Configuração Crítica:** Baudrate de **12340 baud** (padrão proprietário da Festo para o Robotino v2).