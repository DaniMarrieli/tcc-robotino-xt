# Objetivo

Desenvolver uma **ponte de comunicação de software** entre um sistema robótico legado **Robotino XT (firmware 2.4, Ubuntu 9.04, arquitetura x86)** e um **Raspberry Pi 4 rodando Ubuntu 22.04 (ARM64)** com **ROS2**, de modo a permitir leitura de dados e envio de comandos de movimento.

# Contexto Técnico

O **Robotino XT** possui uma **API nativa em C++** compilada em bibliotecas `.so` de 32 bits para arquitetura Intel 80386, incompatível com ARM.  
O **Raspberry Pi 4** será responsável pela integração com o **ROS2**, publicando e assinando tópicos padrão (como `/odom` e `/cmd_vel`).  
Assim, a solução consiste em desenvolver uma **ponte de comunicação em rede (TCP/IP)**, onde:

- **No Robotino (Servidor)**:
    
    - Executa um programa usando a API nativa para:
        
        - Ler dados de sensores e odometria.
            
        - Receber comandos de velocidade.
            
    - Mantém uma conexão TCP/IP e atua como **servidor**, traduzindo dados entre a API legada e o cliente de rede.
        
- **No Raspberry Pi (Cliente ROS2)**:
    
    - Executa um **nó ROS2** que:
        
        - Se conecta ao servidor Robotino via socket TCP/IP.
            
        - Recebe dados e publica em tópicos ROS2 (`nav_msgs/Odometry`, etc.).
            
        - Escuta comandos de `/cmd_vel` e os envia ao servidor.
            

# Tarefa

Crie um **plano detalhado de desenvolvimento e implementação**, incluindo:

1. **Arquitetura geral do sistema** (esquemática e descrição funcional).
    
2. **Estrutura de software** em ambos os lados (Robotino e Raspberry Pi), incluindo linguagens e bibliotecas sugeridas.
    
3. **Exemplo de protocolo de comunicação TCP/IP** (formato das mensagens, ciclo de envio/recebimento, exemplo de handshake).
    
4. **Exemplos de código**:
    
    - Programa servidor no Robotino (C/C++).
        
    - Nó cliente no ROS2 (Python ou C++).
        
5. **Passos para teste e validação da comunicação.**
    
6. **Sugestões para otimização**, como controle de latência e sincronização de tempo.
    

# Instruções

- Forneça o plano de forma **organizada e técnica**, com seções e subtítulos.
    
- Use linguagem clara, objetiva e profissional.
    
- Sempre que possível, inclua **exemplos de código comentados**.
    
- Use pseudocódigo se o código completo for extenso.
    
- Mantenha compatibilidade com **Ubuntu 9.04** e **Ubuntu 22.04**, destacando as limitações de cada lado.
    
- Explique como essa arquitetura pode ser expandida futuramente (ex: suporte a mais sensores ou controle remoto via ROS2).