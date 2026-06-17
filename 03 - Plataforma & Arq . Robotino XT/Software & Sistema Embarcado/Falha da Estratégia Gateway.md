# Relatório de Falha: Arquitetura Gateway (Rede)

**Data da Análise:** Setembro/2025

## 1. Abordagem Proposta
Tentar manter o hardware original (AMD Geode) rodando um servidor que falaria com o Raspberry Pi (Cliente) via TCP/IP e uma conexão com uma maquina externa para controle e comandos em ROS 2.

## 2. Causas do Insucesso
- **Incompatibilidade Binária:** As bibliotecas `.so` do Robotino v2 são compiladas para 32-bit (i386) e não podem ser portadas ou linkadas diretamente no ambiente ARM64 do Raspberry Pi.
- **API Corrompida:** Erros de download (403/404) na API v2 da Festo impediram a construção do servidor de rede estável.
- **Latência:** A ponte de rede adicionava atrasos que o sistema RTAI legado não conseguia compensar, resultando em perda de pacotes de controle.

## 3. Resultado
A estratégia foi abandonada em favor da comunicação serial direta entre o RPi4 e a IO Board.