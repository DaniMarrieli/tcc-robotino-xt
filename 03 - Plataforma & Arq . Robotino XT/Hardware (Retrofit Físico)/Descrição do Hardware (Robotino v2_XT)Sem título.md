# Descrição Técnica do Hardware Robotino XT

A plataforma Robotino XT é um sistema robótico móvel omnidirecional projetado para pesquisa e ensino. O modelo em questão (v2) possui uma base circular que abriga a inteligência de baixo nível e a tração.
![[ArqOriginal.jpeg|169]]
## 1. Sistema de Tração
- **Configuração:** 3 rodas independentes posicionadas a 120°.
- **Motores:** Motores DC com encoders de alta resolução (4000 pulsos por volta).
- **Cinemática:** Permite movimento omnidirecional (translação em qualquer direção e rotação simultânea).

## 2. Sensores Integrados (Legados)
- **Odometria:** Realizada via encoders acoplados aos motores.
- **Sensores de Distância:** 9 sensores infravermelhos (IR) dispostos ao redor da base para detecção de obstáculos próximos.
- **Bumper:** Detector de colisão mecânico 360°.
![[PlacaInfrasMotoresBumpCarregador.jpeg|218]]
## 3. Arquitetura Interna Original
- **UCP:** Processador AMD Geode com Chipset CS5536.
- **Interface de Controle:** Placa de E/S (IO Board) que gerencia os drivers de motor e a leitura de sensores analógicos/digitais.
![[RobotinoAMD.jpeg|261]]