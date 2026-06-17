A arquitetura interna do sistema embarcado do Robotino XT, que define sua natureza legada, é baseada em um conjunto específico de hardware e software de baixo nível.

A arquitetura pode ser descrita da seguinte forma:

- **Processador Principal:** O sistema utiliza um hardware baseado na família de processadores **[[AMD Geode]]**, que é suportado por um [[chipset **AMD CS5536** ]]e utiliza a interface IDE para o armazenamento (Compact Flash).
    
- **Sistema Operacional:** O sistema operacional instalado é o **Linux Ubuntu 9.04**, rodando sobre um **Kernel Linux 2.6.x**.
    
- **Controle de Tempo Real:** Para o controle preciso de motores e sensores, o kernel utiliza o módulo **[[RTAI]]**, que garante uma latência previsível, essencial para a estabilidade e a performance do robô.
    
- **Drivers e APIs:** O acesso ao hardware é feito através de drivers personalizados e da **OpenRobotinoAPI** (versões 1 e 2), que são bibliotecas (como `librec_robotino_com.so`) compiladas para a arquitetura 32-bit (Intel 80386) e dependem de bibliotecas auxiliares como o **Boost** (Boost.Asio) para comunicação de rede.
    
- **Comunicação Interna:** A comunicação interna de controle de baixo nível também é gerenciada por _drivers_ como os de barramento **[[CAN (PCAN Drivers)]]** .