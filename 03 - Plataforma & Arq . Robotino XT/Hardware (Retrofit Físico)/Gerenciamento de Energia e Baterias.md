# Sistema de Energia e Manutenção

O Robotino v2 possui um sistema de alimentação crítico que exige monitoramento constante para evitar danos às células e garantir a estabilidade da nova UCP.

## 1. Especificações das Baterias
- **Tipo:** 2 baterias de 12V conectadas em série (Total 24V nominais).
- **Substituição:** Devido ao tempo de inatividade, foram adquiridas novas baterias Pb/NiMH em abril/2025 para possibilitar o boot do sistema.
- Bateria antiga (Alemã):
![[bateria.png|225]]

- Alternativa de bateria nova (nacional):
![[baterianova.png|285]]
## 2. Falha no Circuito de Carga
- **Diagnóstico:** Identificado que o regulador de tensão interno da IO Board está queimado.
- **Procedimento de Carga:** A carga deve ser feita externamente via fonte de alimentação ajustada em **14,4V e 1,6A** por bateria (ou 28,8V no conjunto total).
![[ReguladorTensão.jpeg|263]]
## 3. Limites de Segurança de Software
- **Configuração (`controld2.conf`):** O parâmetro `sleep_voltage` está definido em **22.5V**. 
- **Impacto:** Se a tensão cair abaixo deste limite, o robô entra em modo de proteção e encerra os serviços de controle de motor para preservar a bateria.