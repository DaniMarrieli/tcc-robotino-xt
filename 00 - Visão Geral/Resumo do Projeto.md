# Resumo do Projeto: Revitalização do Robotino XT

**Instituição:** UNIFEI - Campus Itabira  
**Curso:** Engenharia de Controle e Automação  
**Período de Desenvolvimento:** Abril/2025 – Junho/2026  

Este projeto documenta a revitalização e o retrofit sistêmico de uma plataforma robótica móvel Robotino XT (v2), focando na superação das limitações impostas por hardware e software legados.

### 🔧 Marcos da Implementação Prática

#### 1. Diagnóstico e Revitalização Inicial (Abril/2025)
- **Restauração Elétrica:** Substituição das baterias Pb/NiMH totalmente descarregadas por novas células para permitir o boot inicial do sistema.
- **Identificação da Arquitetura:** Mapeamento do hardware baseado no processador AMD Geode (arquitetura 32-bit x86) e chipset CS5536, operando com Ubuntu 9.04 e Kernel 2.6.x (RTAI).
- **Firmware:** Validação e fixação na versão 2.4 para garantir estabilidade mínima entre o Kernel legado e os drivers PCAN de controle dos motores.

#### 2. Reconfiguração de Hardware e Otimização
- **Remoção do Módulo Pneumático:** Extração do compressor e do manipulador biônico original para reduzir o alto consumo energético e eliminar limitações de movimento da base.
- **Manutenção de Carga:** Identificação de falha no regulador de tensão interno, estabelecendo o método de carga direta via fonte externa regulada em 14,4V/1,6A.

#### 3. Tentativa de Arquitetura Gateway (Abordagem Inicial)
- **Proposta:** Utilizar o Raspberry Pi 4 como uma ponte (Gateway) entre um PC externo (ROS 2) e a UCP AMD Geode original (Servidor).
- **Insucesso da Integração:** Esta etapa enfrentou falhas recorrentes. A OpenRobotinoAPI e as bibliotecas legadas (compiladas para Intel 80386) apresentaram incompatibilidade de rede e latências excessivas, impedindo o controle em tempo real via socket.
- **Decisão Estratégica:** Devido à instabilidade do software legado e à impossibilidade de atualizar o Kernel RTAI, optou-se pela substituição total da UCP pela arquitetura Raspberry Pi 4.

#### 4. Retrofit de UCP e Depuração de Baixo Nível
- **Integração Serial:** Estabelecimento de comunicação direta entre a UART do Raspberry Pi 4 e a IO Board via transceptor de nível lógico.
- **Mitigação de Dependências:** Identificação e isolamento do erro "Detached from Gyroscope SHM", forçando o driver `controld2` a operar de forma independente através da limpeza de segmentos de memória compartilhada (`ipcrm -a`).

### 📍 Local de Execução
Laboratório 2314 - UNIFEI Campus Itabira.