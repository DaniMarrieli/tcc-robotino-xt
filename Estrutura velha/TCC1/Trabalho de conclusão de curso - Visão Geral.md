# Objetivo

O objetivo do trabalho é **[[revitalizar]]** um robô, do modelo **[robotino XT](https://www.festo.com/br/pt/e/sobre-a-festo/pesquisa-e-desenvolvimento/bionic-learning-network/garras-bionicas-e-robos-flexiveis/robotino-r-xt-id_33731/)** da festo, afim de fazé-lo voltar ao seu pleno funcionamento, após isso, **[[reconfigurar]]** o hardware do robô removendo o seu modulo eletropneumático, para assim, posteriormente **[[integrar]]** um **[raspberry pi](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)** a ele, funcionando como um **[[gateway]]** de comunicação entre um computador externo rodando **[ROS2](https://docs.ros.org/en/humble/index.html)** e o robotino XT.

# Processo

## 1. Revitalização da Plataforma

### 1.1 Limitações de hardware

- Para conseguir ligar o robô foi identificado que pelo tempo em que esteve parado suas [baterias](bateria.png) tinham perdido a carga total, não sendo possível carregá-las, então foi necessario comprar [novas baterias](baterianova.png) para testar as demais funções do robotino.

- Após ligar o robô o desafio foi identificar a [[versão do firmware]] instalada nele e se tinha como atualizar essa versão para uma mais recente, porém a [[arquitetura interna do sistema embarcado]] não permitia atualizações mais recentes, parando assim na versão 2.4 disponivel na [robotino wiki](https://www.openrobotino.org/Main_Page.html) compativel com o [CF](https://www.openrobotino.org/CF_card.html), cartão usado para gravação de firmware e SO do robô.

- Foi removido o CF do robotino, levado a um computador com windows 7 com leitor de CF e feita a atualização do firmware 2.4 de acordo com a versão disponível, em seguida testado e reconhecido pelo restante da plataforma.

### 1.2 Limitações de Software

- O primeiro teste do robô foi feito quando as baterias carregadas foram colocadas nele, o robotino ligou e tinha algumas programações demo já gravadas em sua memória, tornando possível a sua movimentação, porém sem aplicação de comandos especificos.

- Em seguida veio o gargalo, a comunicação do robô (ainda sem o firmware atualizado), com a plataforma oficial de programação da festo, o [robotino view](https://ip.festo-didactic.com/Infoportal/robotino/Software/Programming/EN/RobotinoView.html). As tentativas de comunicação foram realizadas em todas as [versões disponiveis](versoesview.png) do software até o momento, porém não houve sucesso no estabelecimento dessa comunicação, que é realizada através do IP do robotino, o quão não estava sendo reconhecido pelo robotino view, sendo assim, foi identificada a necessidade de atualização do firmware para estabelecer essa conexão.

- Depois da atualização a comunicação com o software foi testada novamente, e a versão compativel com o robotino XT foi a **RobotinoView 2.8.4** . Comunicação esta que é feita através do IP gerado pelo robotino, no caso deste modelo é o **ip 172.26.1.100**.

### 1.3 Testes e Identificação de Falhas

- Com a comunicação entre o robotino XT e o robotino view estabelecida foram realizados alguns testes para identificar se todos os motores, sensores, valvulas e o manipulador estavam funcionando corretamente.

- No teste dos motores os três funcionaram normalmente, seus encoders também estavam em pleno funcionamento realizando a leitura da odometria referênte a cada um.

- Partindo para o teste dos 9 sensores infravermelhos,  foram identificados 2 infravermelho com defeito, sendo eles o sensor 3 e o sensor 9. Houve a troca do sensor 3 e a volta de sua utilização na plataforma, já o sensor 9 mostrou um erro além do funcionamento do sensor, provavelmente no soquete de conexão entre ele e a placa, não conseguindo integra-lo novamente ao funcionamento geral da plataforma. 
  
- Partindo para o teste do manipulador e o circuito de comando eletropneumático os resultados foram positivos, o cilindro, compressor, valvulas, e placa funcionaram perfeitamente, fazendo com que o manipulador movimentasse em todos os seus graus de liberdade.
  
_**- O detector de colisão**_ (falta testar)

- Quanto a aspectos gerais do robotino, o seu circuito de carregamento não está funcionando, foi testado e constatado que o **[[regulador de tensão]]** na placa está queimado, não permitindo a regulação e envio de carga as baterias. Sendo assim, as baterias seguem sendo carregadas diretamente através de uma fonte de alimentação externa, com regulação 14,4V e 1,6A.


## 2. Reconfiguração de Hardware

### 2.1 Motivação

- O objetivo principal dessa reconfiguração é aumentar a versatilidade do robotino em relação ao seu tempo de carga e diminuir sua limitação de movimento (devido a garra que se estendia para fora da base móvel). Por conta do alto consumo de energia pelo compressor para atuar o manipulador, o robotino chegava a durar cerca de 30/40 minutos em pleno funcionamento, limitando suas possibilidades de utilização, tendo em vista que seu tempo de carga é em torno de 2h a 3h, a depender das configurações da fonte de alimentação.

### 2.2 Remoção do Manipulador e do Módulo de Controle Eletropneumático

- Inicialmente [a carcaça externa](carca.jpeg) do robotino foi removida para melhor visualização e identificação dos pontos de conexão do manipulador.