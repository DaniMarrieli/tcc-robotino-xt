# Janeiro/2026 — Diagnóstico da Comunicação Serial

## Contexto
Após a instalação do Raspberry Pi como nova UCP, foi iniciada a etapa de integração direta com a IO Board do Robotino.

## Configuração
- UART do Raspberry Pi habilitada
- Conexão via transceptor MAX3232
- Porta utilizada: /dev/serial0

## Testes realizados
- Execução do daemon controld2
- Teste com minicom
- Loopback TTL para validação da UART

## Resultados
- UART do Raspberry Pi funcional
- controld2 inicializa, porém não estabelece comunicação estável com a IO Board
- Observado erro relacionado à memória compartilhada (Gyroscope SHM)

## Hipóteses investigadas
- Inversão RX/TX
- Falha de GND comum
- Configuração incorreta dos jumpers da IO Board
- Problema no transceptor RS232