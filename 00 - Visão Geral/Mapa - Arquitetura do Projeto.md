
# Robotino XT – Retrofit

## 1. Plataforma Robotino XT (Arquitetura Original)

### Estrutura Mecânica
- Base circular omnidirecional
	- 3 graus de liberdade
- Garra Pneumatica
	- 9 graus de liberdade
- 3 rodas omnidirecionais (120°)
- Encoders de alta resolução
- Bumper 360°
- 9 sensores infravermelhos
	- 1 com defeito

### Sistema de Energia
- 2 baterias 12V em série
- 24V nominal
- tensão de proteção
	- 22.5V (sleep_voltage)

### Hardware de Controle Original
- CPU: AMD Geode
- arquitetura: x86 32-bit
- chipset: CS5536

### Software Original
- Sistema operacional
  - Ubuntu 9.04
- Kernel
  - Linux 2.6
- extensão de tempo real
  - RTAI

### Drivers Proprietários
- OpenRobotinoAPI
- robotino-pcan
- bibliotecas .so compiladas para i386

---

## 2. IO Board (Interface de Controle)

### Função
- interface entre CPU e hardware do robô

### Atuadores controlados
- drivers dos motores
- controle PWM
- leitura de encoders

### Sensores
- sensores infravermelhos
- bumper
- odometria

### Interface de Comunicação
- protocolo serial
- padrão RS232

### Parâmetros de Comunicação
- baudrate: 12340
- paridade: none
- stop bits: 1

### Mensagens
- pacotes binários curtos
- IDs de mensagem

---

## 3. Problema de Obsolescência

### Limitações do Sistema Legado
- software antigo
- bibliotecas incompatíveis
- dependência do kernel RTAI
- incompatibilidade com ROS2

### Incompatibilidade de Arquitetura
- AMD Geode
  - x86
- Raspberry Pi
  - ARM64

### APIs Legadas
- OpenRobotinoAPI v2
- repositórios descontinuados

---

## 4. Tentativa de Arquitetura Gateway

### Conceito
- Raspberry Pi como gateway
- AMD Geode como servidor de baixo nível

### Comunicação via rede
- Ethernet
- TCP/IP
- REST API (rpcd)

### Fluxo de dados
Raspberry Pi  
↓  
Gateway de rede  
↓  
AMD Geode  
↓  
IO Board

### Problemas encontrados
- latência de rede
- incompatibilidade binária
- APIs corrompidas
- dependências obsoletas

### Resultado
- arquitetura descartada

---

## 5. Retrofit da UCP

### Nova Unidade de Processamento
- Raspberry Pi 4

### Sistema Operacional
- Ubuntu Server 22.04

### Middleware
- ROS2 Humble

### Objetivos do Retrofit
- modernizar arquitetura
- permitir integração com ROS2
- eliminar dependência do AMD Geode

---

## 6. Nova Arquitetura do Sistema

### Camada de Alto Nível
ROS2

- navegação
- planejamento de trajetória
- controle de movimento

### Tópicos ROS2

Publicação
- /odom
- sensores

Subscrição
- /cmd_vel

### Driver de Hardware
controld2

Função
- traduzir comandos de software
- enviar comandos para IO Board

---

## 7. Comunicação Serial (Camada Física)

### Arquitetura

Raspberry Pi  
↓ UART TTL (3.3V)  
↓  
Transceptor MAX3232  
↓ RS232  
↓  
IO Board

### Pinos Raspberry Pi

TX
- GPIO 14

RX
- GPIO 15

### Conversão de Nível

TTL
- 0 – 3.3V

RS232
- ±12V

### Função do Transceptor

- conversão de níveis lógicos
- adaptação elétrica entre dispositivos

---

## 8. Serviços de Controle

### Daemon principal
controld2

Funções
- comunicação serial
- leitura de sensores
- controle de motores

### Memória Compartilhada

SHM
- odometria
- giroscópio
- sensores

### Problemas encontrados

Detached from Gyroscope SHM

Causa
- segmentos de memória órfãos

Solução
- ipcrm -a

---

## 9. Ferramentas de Diagnóstico

### SSH
- acesso remoto
- configuração do sistema
- reinício de serviços

### Minicom
- terminal serial
- testes de comunicação

Comando

sudo minicom -b 12340 -D /dev/serial0

### systemctl

controle de serviços

- start controld2
- stop controld2
- status controld2

### journalctl

logs do sistema

---

## 10. Testes Realizados

### Diagnóstico de Energia
- teste de baterias
- regulador da IO Board queimado
- carga externa das baterias

### Testes de Comunicação

- UART habilitada
- loopback serial
- minicom

### Testes de Transceptor
- substituição do MAX232
- uso de MAX3232

### Testes do Driver
- execução do controld2
- análise de logs

---

## 11. Arquitetura Final do Sistema

Raspberry Pi 4  
↓  
ROS2  
↓  
controld2  
↓  
UART  
↓  
MAX3232  
↓  
RS232  
↓  
IO Board  
↓  
Drivers de Motor  
↓  
Motores Omnidirecionais