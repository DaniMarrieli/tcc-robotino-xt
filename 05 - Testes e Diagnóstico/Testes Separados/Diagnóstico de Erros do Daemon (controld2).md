
# Depuração do Serviço controld2

**Problema Comum:** O serviço entra em loop ou não consegue "falar" com os motores.

## Comandos de Diagnóstico
Para ver o que o robô está "sentindo" em tempo real:
```bash
sudo /opt/robotino/bin/controld2 -v
```

## Logs de Erro Identificados

- **"EA09 message length must be >= 0"**: Indica que o pacote recebido via serial está incompleto ou com ruído elétrico.
    
- **"State changed to 3 (Connected)"**: Indica sucesso na sincronização entre a nova UCP e a placa de potência.
    

## Ações de Correção

- Verificar se o Bluetooth está desativado (`dtoverlay=miniuart-bt`), pois ele interfere na UART do RPi 4.

## Exemplo de Log

EA09 message length must be >= 0

Interpretação:
pacote serial recebido com tamanho inválido.