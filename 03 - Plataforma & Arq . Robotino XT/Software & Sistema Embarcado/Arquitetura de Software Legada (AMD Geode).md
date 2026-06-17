# Arquitetura de Software Legada

A inteligência original do Robotino XT reside em um ecossistema de software "congelado" no tempo, necessário para manter a compatibilidade com o hardware de 2013.

## 1. Sistema Operacional e Kernel
- **SO:** Ubuntu 9.04 (Jaunty Jackalope) 32-bit.
- **Kernel:** Versão 2.6.x. Esta versão é a última a oferecer suporte estável para os drivers de baixo nível e a interface de tempo real do Robotino.

## 2. Camada de Tempo Real (RTAI)
- **Função:** O RTAI (Real-Time Application Interface) é uma extensão que transforma o Linux em um sistema de tempo real rigoroso (*hard real-time*).
- **Importância:** Garante latência < 1ms para o controle de motores e leitura de odometria. Sem o RTAI, a precisão do controle é perdida.

## 3. Drivers e APIs Proprietárias
- **OpenRobotinoAPI:** Bibliotecas (como `librec_robotino_com.so`) compiladas estritamente para a arquitetura 32-bit (Intel 80386).
- **PCAN Drivers:** Pacote `robotino-pcan` que gerencia a comunicação via barramento CAN entre a placa-mãe e os módulos de motor.