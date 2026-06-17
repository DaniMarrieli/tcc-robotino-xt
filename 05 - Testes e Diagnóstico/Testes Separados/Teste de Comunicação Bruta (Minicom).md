
# Diagnóstico de Comunicação Serial (Baixo Nível)

**Objetivo:** Validar se os dados enviados pelo Raspberry Pi 4 chegam à IO Board do Robotino sem corrupção.

## Procedimento de Teste
1. Conectar via SSH ao Raspberry Pi.
2. Abrir o terminal serial com o baudrate específico do firmware 2.4:
   bash
   sudo minicom -b 12340 -D /dev/serial0

3. Digitar o comando: `version`
    

## Resultados Esperados

- O Robotino deve responder com a versão do firmware (ex: `v2.4`).
    
- Se a tela ficar em branco ou caracteres ilegíveis aparecerem, indica erro de baudrate ou falha no transceptor MAX3232.

## Topologia de Comunicação

Raspberry Pi
↓ UART TTL
MAX3232
↓ RS232
IO Board

## Resultado Obtido

- ausência de resposta da IO Board
- nenhum retorno ao comando "version"

## Interpretação

Se o comando não retorna resposta, as causas possíveis são:

- inversão de RX/TX
- falha no transceptor
- GND não compartilhado
- baudrate incorreto
- jumpers da IO Board