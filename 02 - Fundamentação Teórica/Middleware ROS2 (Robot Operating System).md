# Middleware ROS2 Humble

## Função no Projeto
O ROS2 atua como a camada de abstração que permite ao robô "pensar". Ele organiza a comunicação entre diferentes partes do software (nós) através de tópicos.

## Tópicos Principais
- `/cmd_vel`: Recebe comandos de velocidade geométrica (x, y, angular) para mover a base.
- `/odom`: Publica a posição estimada do robô no espaço.
- `/scan`: (Futuro) Para integração com sensores de distância/Lidar.

## Vantagem da Arquitetura Híbrida
O ROS2 permite que o processamento pesado (mapeamento e navegação) seja feito no Raspberry Pi, enviando apenas os comandos finais de movimento para a IO Board legada.