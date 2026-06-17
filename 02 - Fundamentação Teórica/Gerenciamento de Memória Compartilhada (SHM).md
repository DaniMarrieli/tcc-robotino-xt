# Memória Compartilhada (Shared Memory - SHM)

## O Erro "Detached from Gyroscope SHM"
Este é um conceito técnico avançado que surgiu na prática. O driver `controld2` tenta acessar um espaço de memória na RAM onde o giroscópio deveria escrever seus dados.

## Mecanismo de Falha
Quando o sistema é reiniciado de forma abrupta ou o sensor não é encontrado, esse "espaço de memória" fica bloqueado ou órfão. 
- **Solução Técnica:** O uso do comando `ipcrm -a` limpa esses segmentos de memória compartilhada, permitindo que o driver reinicie sem tentar se conectar a um sensor inexistente, garantindo a estabilidade da inicialização do robô.