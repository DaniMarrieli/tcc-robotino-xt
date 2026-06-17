# Teste Gateway
## 📌 1. Objetivo Central

- **Integração de Sistemas:** Conectar o Robotino XT (Legado) ao Raspberry Pi 4 (Moderno) via rede Ethernet.
    
- **Arquitetura Servidor/Cliente:**
    
    - **Robotino XT (Servidor):** Responsável pelo controle de baixo nível e leitura de sensores via API nativa.
        
    - **Raspberry Pi 4 (Gateway/Cliente):** Tradutor de protocolos, transformando a API proprietária em mensagens ROS2.
        

---

## 🏗️ 2. Estrutura de Comunicação

- **Camada Física:** Cabo Ethernet RJ45 conectando os dois dispositivos em uma sub-rede local (ex: `172.26.1.x`).
    
- **Protocolos de Rede:**
    
    - **TCP (Porta 10001):** Utilizado para comandos de controle e fluxo de dados da API v2.
        
    - **REST API (Porta 12080):** Interface baseada em HTTP para leituras rápidas através do serviço `rpcd`.
        
    - **SSH:** Acesso remoto para monitoramento via comandos específicos de compatibilidade (`diffie-hellman-group1-sha1`).
        

---

## 🛠️ 3. Implementação Técnica

- **No Robotino XT:**
    
    - Execução de programas em C++ utilizando as bibliotecas `.so` (Intel 80386).
        
    - Dependência do Kernel 2.6.x e módulo RTAI para manter a precisão de tempo real (<1ms).
        
- **No Raspberry Pi 4:**
    
    - Hospedagem do Master ROS2 (Humble/Ubuntu 22.04).
        
    - Desenvolvimento de um "Client Raw" em Python para fazer o parse binário de pacotes UDP/TCP.
        

---

## ⚠️ 4. Pontos de Falha e Desafios (Motivação do Retrofit)

- **Obsolescência de Software:**
    
    - **APIs Corrompidas:** Downloads da API v2 da Festo apresentaram erros 403/404, inviabilizando o uso de bibliotecas oficiais.
        
    - **Gargalo de Arquitetura:** Incompatibilidade direta entre binários 32-bit (AMD Geode) e 64-bit (ARM64).
        
- **Performance:**
    
    - **Latência de Rede:** A ponte de rede adicionava atrasos de processamento que quebravam o sincronismo do RTAI.
        
    - **Instabilidade:** Erros recorrentes de integração levaram à conclusão de que manter o hardware AMD Geode era um risco ao projeto.
        

---

## 🏁 5. Resultado Final

- **Mudança de Escopo:** Abandono da função de Gateway.
    
- **Nova Estratégia:** Retrofit Total da UCP, instalando o Raspberry Pi 4 como processador central conectado diretamente à IO Board via Serial/UART.