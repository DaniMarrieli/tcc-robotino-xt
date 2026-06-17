# Objetivo do Retrofit e Justificativa

## 🔍 O Problema (Obsolescência Tecnológica)
O Robotino XT original tornou-se uma plataforma limitada devido ao hardware de processamento defasado, impedindo o uso de sensores modernos (como LIDAR e câmeras de profundidade) e algoritmos de IA.

## ⚙️ A Solução (Arquitetura Híbrida)
A estratégia não visa descartar a estrutura da Festo, mas sim complementar sua robustez eletromecânica com a versatilidade do Raspberry Pi 4.
- **Nova UCP:** Raspberry Pi 4 (maior poder de processamento e baixo custo).
- **Middleware:** ROS (Robot Operating System) para modularização do sistema.
- **Conectividade:** Link serial seguro via UART mapeada para a porta física da IO Board.

## 💡 Impacto e Sustentabilidade
Demonstrar que o retrofit digital é uma alternativa técnica e economicamente viável à substituição completa de equipamentos, promovendo o reaproveitamento de investimentos acadêmicos em Itabira.