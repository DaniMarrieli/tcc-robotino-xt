# Kernel RTAI e a Necessidade de Tempo Real

## O que é RTAI?
O **Real-Time Application Interface** é uma extensão do Kernel Linux que o transforma em um sistema de tempo real rigoroso (*hard real-time*).

## Importância no Robotino
- **Controle de Motores:** Garante que os comandos de velocidade cheguem à IO Board com latência baixíssima (< 1ms).
- **Odometria:** A leitura dos encoders das 3 rodas omnidirecionais precisa ser precisa para o cálculo correto da posição.
- **Dependência:** O software original da Festo é "amarrado" ao RTAI. Sem ele, os motores não respondem de forma estável. No retrofit, o desafio é emular ou traduzir essa comunicação para o Raspberry Pi.