### **Drivers e Barramento CAN (Controller Area Network)**

- **Barramento CAN:** O CAN é um protocolo de comunicação robusto, serial e de alta integridade, usado primariamente em ambientes industriais e automotivos para permitir que diferentes unidades de controle e módulos (como motores, sensores e a placa-mãe) **conversem entre si sem um computador central**.
    
- **Drivers PCAN:** A presença do pacote `robotino-pcan` no sistema do Robotino indica que o robô utiliza adaptadores ou interfaces de hardware da **Peak-Systems** para se comunicar através do barramento CAN.
    
- **Relação:** O _driver_ PCAN traduz as mensagens da camada RTAI do kernel para a interface CAN do hardware de controle do robô. Esta é mais uma peça de _software_ proprietário e dependente da versão antiga do kernel, essencial para a funcionalidade do Robotino.