# Interface de Controle: IO Board

A IO Board é o componente que faz a ponte entre o processador (seja o AMD Geode original ou o Raspberry Pi 4 novo) e os atuadores físicos do robô.

## Protocolo de Comunicação
- **Interface:** Serial RS-232.
- **Baudrate:** O baudrate padrão da IO Board do Robotino v2 é **12340 baud**, um valor proprietário utilizado pela Festo para sincronização com o firmware 2.4.
- **Protocolo:** Envio de mensagens binárias curtas. Erros de comunicação costumam ser reportados como "EA09 message length" em caso de ruído elétrico ou falha de nível lógico.

## Integração com Raspberry Pi 4
A conexão exige o uso de um transceptor (MAX3232) para converter a saída serial da IO Board para os níveis lógicos TTL (3.3V) do Raspberry Pi.