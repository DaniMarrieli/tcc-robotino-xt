
# Relatório de Testes - Fase Gateway (Rede)

**Objetivo:** Estabelecer uma arquitetura híbrida onde o AMD Geode (UCP original) atuaria como Servidor de Baixo Nível e o Raspberry Pi 4 como Gateway/Cliente de Alto Nível.

## 1. Metodologia do Teste

A estratégia consistia em conectar o Raspberry Pi ao Robotino via cabo Ethernet em uma rede local (Sub-rede `172.26.1.x`). O Raspberry Pi deveria:

1. Iniciar uma conexão TCP/IP com o IP do Robotino na porta `10001` (API v2) ou `12080` (REST).
    
2. Solicitar dados de odometria e sensores.
    
3. Traduzir esses dados para mensagens ROS 2 (`/odom`) e receber comandos `/cmd_vel` para reenviar ao robô.
    

## 2. Diagnósticos e Falhas Identificadas

**A. Incompatibilidade de Arquitetura (O "Muro" dos 32-bits):**

- As bibliotecas nativas da Festo (`.so`) foram compiladas para a arquitetura **Intel 80386 (32-bit)** do AMD Geode.
    
- Ao tentar rodar scripts de integração no Raspberry Pi (Arquitetura **ARM64**), houve falha total de linkagem, pois não é possível executar binários x86 nativos em processadores ARM sem emulação pesada, o que inviabilizaria o tempo real.
    

**B. Corrupção de Dependências e APIs Legadas:**

- Tentativas de baixar versões específicas da **OpenRobotinoAPI v2** resultaram em erros `403 Forbidden` ou `404 Not Found` nos repositórios antigos da Festo/OpenRobotino.
    
- A dependência de bibliotecas obsoletas como o `Boost.Asio` em versões muito específicas impediu a compilação de novos wrappers de comunicação no ambiente moderno do Ubuntu 22.04.
    

**C. Análise de Latência (Gargalo de Rede):**

- Testes com `ping` e captura de pacotes via **Wireshark** mostraram que, embora a conexão TCP fosse estabelecida, a pilha de protocolos legada do Ubuntu 9.04 apresentava atrasos intermitentes.
    
- O Kernel RTAI no Robotino exige respostas em prazos de milissegundos; a latência introduzida pela tradução de pacotes no Gateway causava "time-outs" frequentes no controle dos motores.
    

## 3. Resultados com "Raw Protocol"

Foi realizada uma tentativa de bypassar as bibliotecas usando comunicação via **Sockets puros (Raw TCP/UDP)**:

- **Sucesso parcial:** Conexão estabelecida e recebimento de dados hexadecimais brutos (IDs 0x01 a 0x04).
    
- **Problema:** A complexidade de fazer o parse manual de todo o protocolo proprietário, somada à instabilidade do hardware original (AMD Geode apresentando lentidão), tornou o processo pouco confiável para navegação autônoma.
    

## 4. Conclusão da Fase

A fase de Gateway provou que a **obsolescência do software legado** era o maior impedimento. Manter o AMD Geode como "servidor" criava um ponto único de falha e limitava o robô a protocolos de rede instáveis.

**Decisão:** Abandono da comunicação via rede em favor da **comunicação serial direta (UART)** entre o Raspberry Pi e a IO Board, eliminando a necessidade do processador AMD Geode e do sistema operacional Ubuntu 9.04.

## 5. Impacto na Arquitetura do Projeto

A inviabilidade da arquitetura Gateway levou à decisão de substituir completamente a UCP original, estabelecendo comunicação serial direta entre o Raspberry Pi e a IO Board.