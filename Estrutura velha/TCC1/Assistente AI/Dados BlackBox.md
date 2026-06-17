### Resumo Completo do Robotino XT e Integração com ROS2 via Raspberry Pi 4

Este resumo compila todas as informações coletadas ao longo da conversa sobre o **Robotino XT** (modelo legado da Festo, adquirido em ~2006), incluindo especificações técnicas, limitações e o projeto de integração com ROS2. O foco é fornecer uma visão abrangente para entender o hardware/software do Robotino e o objetivo do retrofit híbrido com o Raspberry Pi 4. As informações são baseadas em testes, documentação oficial (Festo/Rec Robotino), fóruns comunitários e análises de logs/saídas de comandos.

#### 1. Especificações Técnicas do Robotino XT
O Robotino XT é um robô móvel omnidirecional (3 rodas independentes) projetado para aplicações de pesquisa em ambientes humanos (ex: navegação social, manipulação). É um modelo "legacy" (descontinuado ~2018), com hardware proprietário que limita atualizações modernas. Abaixo, detalhes chave:

- **Sistema Operacional (SO)**:
  - **Versão**: Ubuntu 9.04 (Jaunty Jackalopepe, 32-bit, lançado em 2009).
  - **Kernel**: 2.6.32.11 (com patches RTAI 3.8.1 para real-time). RTAI (Real-Time Application Interface) é essencial para controle de motores/sensores com latência <1ms.
  - **Razão para legado**: O SO é "congelado" devido a drivers proprietários da Festo (não compatíveis com kernels 3.x+ ou SO modernos como Ubuntu 20.04+). Tentativas de atualização falham porque o kernel 2.6.x é o último a suportar RTAI e periféricos (ex: placa PCAN para motores).
  - **Estado atual**: SO intacto, sem internet (rede isolada para segurança), SSH habilitado (usuário "robotino", senha "robotino"). Pacotes base instalados (openrobotino1/2, rtai, pcan), mas API v2 parcialmente corrompida.

- **Firmware**:
  - **Versão**: 2.4 (de ~2010, última para XT).
  - **Funcionalidades**: Controle de motores omnidirecionais (velocidades vx/vy/omega), leitura de sensores (IR, bumpers, bateria, odometria), comunicação TCP/UDP para comandos/dados. Suporta API v1/v2 básica, mas sem features avançadas (ex: IMU, calibração auto).
  - **Protocolo de comunicação**: Binário little-endian via rede (porta 10001 TCP para subs/comandos, 20001 UDP para broadcasts de sensores). Exposto pelo daemon openrobotino2 (v1), que roda no SO legado.
  - **Limitações**: Firmware antigo não suporta ROS nativo; dados são "raw" (ex: IR como ADC 0-1023, bumpers como bitmask). Sem auto-calibração; sensores precisam de ajuste manual (ex: IR para distância).

- **Hardware (Placa e Componentes)**:
  - **Placa Principal**: PC104+ (formato compacto, ~10x10cm) com processador AMD Geode LX800 (32-bit, ~500MHz, arquitetura x86/i386). Inclui chipset AMD CS5536 para I/O.
  - **Motores e Atuadores**: 3 motores DC com encoders (para odometria), controlados via PCAN (Peak Systems CAN bus). Omnidirectional (movimento em qualquer direção sem rotação).
  - **Sensores**:
    - **IR (Infravermelho)**: 9 sensores analógicos (Sharp GP2D12-like, alcance ~0.1-1m, saída 0-5V ou ADC 0-1023). Usados para detecção de obstáculos.
    - **Bumpers**: 8 sensores digitais (bitmask 0-255, 1 = colisão detectada). Para contato físico.
    - **Bateria**: Monitor de estado (voltagem ~24V, corrente, temperatura). Sem % automático (calculado manualmente).
    - **Odometria**: Encoders nos motores (x/y/phi em metros/radianos, resolução ~1mm).
    - **Outros**: Rede Ethernet (IP fixo 172.26.1.1), USB (para debug), sem câmera/LIDAR padrão.
  - **Drivers e Dependências**: RTAI para real-time, PCAN para CAN bus, libusb1 para USB. Qt4 para interfaces (se usado). Hardware não suporta 64-bit ou ARM (Pi é ARM64, incompatível diretamente).
  - **Alimentação e Conectividade**: Bateria interna (~24V), Ethernet para rede local (172.26.1.x), WiFi hotspot opcional ("RobotinoAP1.110"). Consumo ~50W.
  - **Limitações de Hardware**: Não atualizável (drivers RTAI proprietários); overclock ou mods podem danificar. Rede isolada (sem roteamento para internet).

- **API Instalada e Estado Atual**:
  - **Versão Principal**: OpenRobotino API v1 (daemon openrobotino2 rodando, versão 1.8.27). Suporta comunicação básica (sensores/controle via TCP/UDP).
  - **Tentativa de API v2**: Pacote robotino-api2 0.8.5 (i386) parcialmente instalado (estado "pi" no dpkg, libs/headers ausentes devido a corrupção de .deb). Sem daemons funcionais (robotino-daemons falhou por dependências/Qt).
  - **Outros Pacotes**: openrobotino1 (1.8.54), robotino-rtai-3.8.1, robotino-pcan, libfreenect-robotino (para Kinect antigo), linux-image-2.6.32.11.robotino-rtai-3.8.1-gcc-4.3.
  - **Estado de Comunicação**: Rede interna OK (ping 172.26.1.1 responde). Porta 10001 TCP aberta via openrobotino2; UDP 20001 para broadcasts. Sem API v2 full, mas protocolo raw funciona (testado com telnet).

- **Outras Informações Necessárias**:
  - **Compatibilidade**: Funciona com firmware 2.4, mas não com versões mais novas (3.x, que requerem hardware atualizado). Suporte comunitário limitado (fóruns.robotino.com, GitHub forks).
  - **Segurança**: SO antigo sem patches (vulnerabilidades conhecidas); use em rede isolada. Root access limitado (usuário robotino).
  - **Manutenção**: Firmware atualizável via USB (se disponível), mas não recomendado sem backup. Logs via dmesg ou /var/log.
  - **Custos/Legal**: Produto legado; suporte via Festo (email support@festo.com), mas resposta lenta.

#### 2. Objetivo do Projeto: Integração com ROS2 via Raspberry Pi 4
O projeto visa **modernizar o Robotino XT para ROS2**, permitindo leitura de sensores e controle de movimento em tempo real, sem alterar o hardware/SO legado do XT (impossível devido a RTAI/drivers). A solução é uma **arquitetura híbrida distribuída**, onde o XT age como "servidor de baixo nível" (controla motores/sensores via firmware) e o Raspberry Pi 4 como "cérebro" (processa dados, roda ROS2, integra autonomia).

- **Por Que Essa Abordagem?**
  - **Limitações do XT**: SO/kernel antigo impede ROS nativo ou atualizações (RTAI/drivers proprietários quebram com kernels modernos). API v2 falhou por downloads corrompidos (403/404 em mirrors, rede isolada).
  - **Vantagens do Pi**: Ubuntu Server 22.04 (64-bit, kernel 5.15) suporta ROS2 Humble nativamente. Processamento alto nível (nav2 para navegação, SLAM), QoS para dados real-time, integração com câmeras/LIDAR extras. Rede local (172.26.1.x) conecta Pi ao XT.
  - **Benefícios Gerais**: Controle remoto via ROS2 topics (ex: `ros2 topic pub /cmd_vel ...`), autonomia (evitar obstáculos via IR/bumpers), sem mexer no XT (preserva estabilidade).

- **Como Funciona a Comunicação**:
  - **Protocolo Usado**: Raw TCP/UDP do firmware 2.4 (não API v2 full, pois falhou). Cliente Python no Pi conecta via TCP (porta 10001) para subscrever sensores/comandos, recebe dados via UDP (porta 20001).
  - **Dados Transmitidos**:
    - **Sensores (do XT para Pi)**: IR (9 floats/distancias), bumpers (bitmask UInt8), bateria (BatteryState), odometria (Twist x/y/phi).
    - **Controle (do Pi para XT)**: Velocidades omni (/cmd_vel Twist linear.x/y, angular.z).
  - **Frequência**: ~10-20Hz (ajustável), suficiente para real-time.
  - **ROS2 Topics Publicados no Pi**:
    - `/robotino/bumpers` (std_msgs/UInt8): Bitmask de colisões.
    - `/robotino/ir_0` a `/robotino/ir_8` (sensor_msgs/Range): Distâncias de obstáculos.
    - `/robotino/battery` (sensor_msgs/BatteryState): Estado da bateria.
    - `/robotino/odom` (geometry_msgs/Twist): Posição/orientação.
    - `/cmd_vel` (geometry_msgs/Twist): Comando de movimento (subscribed).
  - **Ferramentas no Pi**: Node ROS2 em Python (rclpy), sockets para TCP/UDP, Wireshark para debug de pacotes.

- **Passos de Implementação (Status Atual)**:
  1. **Pi Configurado**: Ubuntu Server 22.04 + ROS2 Humble instalado, rede 172.26.1.102 (ping XT OK).
  2. **XT Preparado**: SO legado limpo, openrobotino2 rodando (porta 10001 aberta), API v1 funcional.
  3. **Cliente Raw Implementado**: Script Python no Pi conecta TCP/UDP, parse binário (IDs 0x01-0x04 para sensores), publica topics ROS2.
  4. **Testes**: Conexão TCP OK ("Conectado ao XT"), subs enviadas. Dados parciais (bateria/odom funcionam; IR/bumpers ajustáveis via Wireshark). Controle /cmd_vel move XT devagar.
  5. **Integrações Futuras**: Nav2 para autonomia (IR como LaserScan), rviz2 para visualização, SLAM com LIDAR extra.

- **Desafios e Soluções**:
  - **API v2 Falhou**: Downloads corrompidos (403/404); resolvido com raw protocol (sem libs no XT).
  - **Protocolo Raw**: IDs ajustáveis (ex: 0x01 bumpers); debug com Wireshark (capture pacotes hex).
  - **Latência**: RTAI garante <1ms no XT; Pi processa em ~10ms.
  - **Segurança**: Rede local; Pi isola XT de internet.
  - **Alternativas**: Se raw falhar, serial USB (Pi conecta via USB-RS232 ao XT, baud 115200).

- **Resultado Esperado**: ROS2 controla o XT remotamente (ex: `ros2 run teleop_twist_keyboard teleop_twist_keyboard` para mover via teclado), lê sensores para navegação autônoma. O XT permanece "intocado", enquanto o Pi adiciona inteligência moderna.

Este resumo cobre tudo necessário para o Robotino XT e o projeto. Se precisar de detalhes específicos (ex: código completo ou logs), avise!