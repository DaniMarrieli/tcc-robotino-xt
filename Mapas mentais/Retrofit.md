# Retrofit da UCP

## 🔧 Objetivo da Arquitetura

- Substituir a CPU AMD Geode.
- Utilizar Raspberry Pi 4 como unidade central.
- Preservar a IO Board original.
- Integrar futuramente com ROS 2.

---

## 🏗️ Estrutura Proposta

### Raspberry Pi 4

- Ubuntu Server 22.04.
- ARM64.
- UART serial.

### Conversão de Níveis

- MAX3232.
- TTL ↔ RS232.

### IO Board

- Controle dos motores.
- Sensores.
- Encoders.

---

## 🛠️ Implementação

### Raspberry Pi

- Configuração da UART.
- Testes seriais.

### Comunicação

- RS232.
- Conversão TTL-RS232.

### Serviços

- daemon controld2.
- Ferramentas de diagnóstico.

---

## 📊 Resultados Obtidos

### UART Funcional

- Porta serial configurada.

### Conversão Elétrica Validada

- MAX3232 operando.

### Serviços Inicializados

- controld2 executado.

### Comunicação Parcial

- Respostas da IO Board observadas.

---

## ⚠️ Problemas Encontrados

### Sincronização Serial

- Falhas intermitentes.

### Comunicação Instável

- Perda de pacotes.

### Reinicializações

- Serviços interrompidos.

### Dependência Temporal

- Comportamento associado ao RTAI.

### Controle

- Motores não acionados.

### Odometria

- Dados inconsistentes.

---

## ❌ Resultado Final

Retrofit não validado.

Motivos:

- Falhas persistentes na comunicação serial.
- Impossibilidade de sincronização entre Raspberry Pi e IO Board.

---

## 📚 Contribuições

- Diagnóstico da arquitetura.
- Testes RS232.
- Documentação da IO Board.
- Base para engenharia reversa.
- Registro das limitações do sistema.

---

## 🚀 Trabalhos Futuros

- Engenharia reversa da IO Board.
- Estudo do protocolo serial.
- Novos drivers para ROS 2.
- Substituição da eletrônica proprietária.