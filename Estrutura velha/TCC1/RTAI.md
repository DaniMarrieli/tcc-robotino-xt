- **Definição:** O RTAI é uma **extensão do kernel Linux** que transforma o sistema operacional (normalmente de propósito geral) em um sistema de **tempo real rigoroso (_hard real-time_)**.
    
- **Função:** Sua principal função é garantir que comandos críticos de controle de motores e leitura de sensores sejam executados em um **prazo de tempo exato e previsível** (baixa latência). Isso é vital para a estabilidade e a precisão em robótica.
    
- **Implicação:** Os _drivers_ do Robotino são estritamente ligados ao RTAI e ao Kernel 2.6.x. A ausência de suporte para estes _drivers_ em kernels modernos é o que impede a atualização do sistema operacional, justificando a adoção da sua arquitetura híbrida.