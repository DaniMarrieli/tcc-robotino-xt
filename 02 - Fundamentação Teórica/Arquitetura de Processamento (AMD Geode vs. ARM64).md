# Arquitetura de Processamento: O Gargalo de 32-bits

## Processador AMD Geode (Legado)
- **Arquitetura:** x86 32-bit (família Intel 80386).
- **Limitação:** Por ser uma arquitetura antiga, não suporta sistemas operacionais modernos ou bibliotecas compiladas para 64-bit. Isso "prende" o hardware ao Ubuntu 9.04.
- **Chipset CS5536:** Gerencia o I/O de baixo nível, mas depende de drivers específicos que não foram portados para Kernels Linux atuais (acima do 2.6).

## Raspberry Pi 4 (Nova UCP)
- **Arquitetura:** ARM64 (Cortex-A72).
- **Vantagem:** Compatível com as versões mais recentes do Ubuntu Server e ROS2 Humble, permitindo o uso de processamento paralelo e bibliotecas de visão computacional modernas.