# Teste de Autonomia e Tensão Elétrica 

**Objetivo:** Monitorar o comportamento das novas baterias e o impacto da remoção do módulo pneumático. 
## Métricas de Bancada 
- **Tensão de Corte (Sleep):** 22.5V (configurado no `controld2.conf`). 
- **Protocolo de Carga:** Fonte externa em 28,8V (14,4V por bateria) a 1,6A. 
## Procedimento de Medição

Equipamento utilizado:
- Multímetro digital

Pontos de medição:
1. Saída do regulador da IO Board
2. Terminais das baterias
3. Entrada do circuito de carga

Resultados:
- saída do regulador: 0V
- tensão nas baterias: 24.3V
## Resultados de Diagnóstico 
1. **Regulador de Tensão:** Testes com multímetro confirmaram que o regulador original da IO Board está queimado (não envia carga às baterias). 
2. **Impacto do Retrofit:** A remoção do compressor eliminou picos de corrente.

