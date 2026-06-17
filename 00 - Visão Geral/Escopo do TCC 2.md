# Escopo do TCC 2 - Implementação e Autonomia

**Previsão de Conclusão:** Junho de 2026
**Status:** Em Execução (Fase de Integração e Testes) 🚀

## 🎯 Objetivo
Implementação de uma arquitetura de controle híbrida e distribuída, capacitando o robô para navegação autônoma.

## 🛠 Atividades de Implementação (Tasks 5 a 9)
- **T5: Arquitetura Distribuída:** Estabelecimento de comunicação bidirecional entre RPi4, IO Board e computador externo.
- **T6: Algoritmos de Navegação:** Desenvolvimento de lógica de movimento e tomada de decisão via ROS (Robot Operating System).
- **T7: Validação Experimental:** Testes práticos de trajetória e desvio de obstáculos no Laboratório 2314 da UNIFEI.
- **T8/T9:** Documentação final, análise crítica de resultados e defesa pública da monografia.

## 📈 Metas Técnicas Atuais
- Estabilização da comunicação serial através da mitigação de erros de Memória Compartilhada (SHM).
- Publicação de tópicos de odometria e subscrição ao tópico de comandos de velocidade (`/cmd_vel`).