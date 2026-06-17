# Dezembro/2025: Instalação do Raspberry Pi como UCP

**Status:** Em Execução 🚀

## Configuração do Ambiente
- **Hardware:** Substituição do processador AMD Geode pelo Raspberry Pi 4.
- **Software:** Instalação ?? no cartão MicroSD.
- **Conectividade:** Acesso configurado via ?

## Integração de Baixo Nível
- **Protocolo Serial:** Validação da comunicação UART entre RPi e IO Board.
- **Baudrate:** Configuração de baud para conversar com o hardware da Festo.
- **Desafio SHM:** Enfrentado o erro "Detached from Gyroscope SHM" ao rodar o `controld2` manualmente, exigindo limpeza de memória compartilhada.