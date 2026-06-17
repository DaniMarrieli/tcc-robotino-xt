# Testes e Diagnósticos - Robotino XT Retrofit

## Visão Geral

Este documento registra todas as etapas experimentais realizadas durante o retrofit do Robotino XT, com foco na identificação de falhas, validação de hipóteses e tomada de decisões para prosseguimento do projeto.

---

## 1. Fase de Revitalização (Abril - Maio/2025)

### Objetivo
Restaurar a capacidade de energização do robô.

### Testes Realizados

#### Teste de baterias
- Foi verificado que as baterias originais (Pb/NiMH) estavam com as celulas degradadas e não aceitavam carga, sendo inutilizaveis no projeto.
![[bateria.png|145]]
#### Compra de baterias
- Realizada a compra de novas baterias (nacionais), compativeis com o robô para prosseguimento dos testes.
![[baterianova.png|185]]
#### Diagnóstico do circuito de carregamento
- Regulador de tensão da IO Board queimado.
![[ReguladorTensão.jpeg|118]]
#### Carregamento de baterias por fonte externa
- Fonte externa utilizada:
	- 14,4V por bateria
	- 1,6A
- Solução paliativa para contornar a falha do hardware embarcado.

### Resultado
- Sistema voltou a energizar corretamente.
- Possibilitou avanço para testes de software

---

## 2. Fase de Validação do Sistema Legado (Junho - Agosto/2025)

### Objetivo
Entender os limites do sistema original e validar a integridade mecânica e funcional.

### Testes

#### Atualização de Firmware
- Ao testar as conexões do robotino com o software robotino view foi constatada falha de compatibilidade das versões de firmware reconhecidas, assim, surgindo a necessidade de atualização.
- Selecionado firmware 2.4, disponivel na robotino wiki com compatibilidade para Compact flash, como único estável para estabelecer a comunicação do robô com o software.
	![[versoesview.png|363]]
#### Atuadores
- Testes realizados com Robotino View 2.8.4:
  - motores omnidirecionais✅
  - sensores infravermelhos
	  - 2 sensores com defeito
	  - 1 foi trocado (priorizado o da frente do robô)✅
  - Garra pneumatica✅

#### Módulo pneumático
![[ArqOriginal.jpeg|225]]
- Funcionamento validado
- Identificado alto consumo energético para ativação dos compressores
 ![[Arq&BoardPneumatica.jpeg|185]]

#### Teste de autonomia
- Após a remoção do modulo pneumático foi identificada uma melhora na percepção e detecção do ambiente pelos sensores
- Como também foi validada uma maior duração da carga para uso constante do robô
![[RobotinoReconfig.jpeg|587]]
### Resultado
- Sistema mecânico funcional
- Módulo pneumático removido por decisão de otimização estrutural

---

## 3. Fase Gateway e Comunicação de Rede (Setembro - Outubro/2025)

### Objetivo
Manter o sistema legado e integrar com Raspberry Pi via rede.

### Testes

#### Acesso SSH
```bash
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 \
-oHostKeyAlgorithms=+ssh-rsa user@ip
```

#### APIs

- Tentativa de uso da OpenRobotinoAPI v2
- Falhas: erros 403/404

#### Comunicação TCP/UDP

- Porta 10001
- Recebimento de dados binários

#### Latência

- Testes com ping e Wireshark
- Latência incompatível com tempo real

### Resultado

- Arquitetura Gateway inviável

---

## 4. Fase de Retrofit e Integração Serial (Dez/2025 - Mar/2026)

### Arquitetura

Raspberry Pi  
↓ UART TTL  
↓ MAX3232  
↓ RS232  
↓ IO Board

### 4.1 Configuração da UART

```bash
enable_uart=1
dtoverlay=miniuart-bt

ls -l /dev/serial*
```
-  /dev/serial0 → ttyAMA0

---

### 4.2 Teste de Loopback

```bash
sudo minicom -D /dev/serial0 -b 115200
```
Procedimento:

- curto entre TX e RX

Resultado:

- caracteres retornam

Conclusão:

- UART funcional

---

### 4.3 Teste com Minicom (IO Board)

```bash
sudo minicom -b 12340 -D /dev/serial0
```
Comando:

```bash
version
```

Resultado:

- sem resposta

---

### 4.4 Testes do Transceptor (MAX3232)

Validações:

- alimentação
- pinagem
- substituição de módulo

Hipóteses:

- RX/TX invertidos
- ausência de GND comum
- defeito no módulo

---

### 4.5 Diagnóstico do controld2

```bash
systemctl status controld2  
sudo /opt/robotino/bin/controld2 -v
```
#### Erro SHM

```bash
Detached from Gyroscope SHM
```
Solução:
```bash
 ipcrm -a
```
#### Erro EA09

```bash
EA09 message length must be >= 0
```

Interpretação:

- erro de comunicação serial
### 4.6 Estado atual

- UART funcional
- controld2 executando
- IO Board não responde

---

## 5. Checklist de Diagnóstico

- [x]  UART habilitada
- [x]  Porta correta (/dev/serial0)
- [x]  Loopback funcional
- [x] Comunicação RS232 validada ✅ 2026-04-06
- [x] RX/TX confirmados ✅ 2026-04-06
- [x] GND comum validado ✅ 2026-04-06
- [x] Jumpers da IO Board verificados ✅ 2026-04-06

---

## Referências

- Comunicação serial RS232 Robotino
- Documentação Robotino View
- Testes práticos com minicom
- Logs do controld2