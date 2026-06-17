O RTAI (**Real-Time Application Interface**) é um conceito fundamental para entender por que o sistema operacional do Robotino XT não pode ser simplesmente atualizado e porque é necessário o projeto de retrofit.

Em essência, o RTAI é uma **extensão do kernel Linux** que permite que o sistema operacional lide com tarefas de **tempo real**.

### **Sua Importância**

1. **Natureza:** O Linux padrão é um SO de propósito geral, o que significa que ele prioriza a justiça no uso dos recursos (garantindo que todos os programas rodem eventualmente), mas não garante que uma tarefa será executada em um prazo exato.
    
2. **Tempo Real (Hard Real-Time):** Em robótica, o controle de motores exige o que é chamado de _hard real-time_ (tempo real rigoroso). Isso significa que, se um comando para o motor deve ser enviado em 1 milissegundo, o sistema **precisa** enviá-lo nesse tempo, ou a precisão do controle (e a estabilidade do robô) é perdida.
    
3. **Função do RTAI no Robotino:** O RTAI modifica o kernel Linux para garantir que tarefas críticas (como o controle de _hardware_ e a leitura de sensores de odometria) tenham a mais alta prioridade e latência previsível. Sem o RTAI, o sistema de controle de baixo nível do Robotino simplesmente não funcionaria de forma confiável.
    

### **Conexão com a Obsolescência**

Os _drivers_ de controle dos motores e sensores do Robotino são estritamente ligados a essa camada RTAI e ao **[[Kernel Linux antigo]]** (versão 2.6.x). Como a Festo e a comunidade não mantiveram a compatibilidade desses _drivers_ RTAI para kernels Linux modernos (versões 5.x ou posteriores), **não é possível** instalar um SO atual sem perder a funcionalidade de controle de tempo real do robô.

Por tanto, esse é o motivo dessa metodologia se concentrar em manter o núcleo RTAI do Robotino XT e usar o Raspberry Pi/ROS2 apenas para o planejamento de alto nível.