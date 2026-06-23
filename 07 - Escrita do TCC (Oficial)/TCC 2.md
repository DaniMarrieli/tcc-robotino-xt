# Revitalização e Adaptação de Sistema Embarcado Obsoleto em Plataforma Robótica Móvel: Um Estudo de Caso com o Robotino XT

## Visão Geral

Este trabalho apresentou um estudo de caso experimental sobre a revitalização e a modernização da plataforma robótica móvel Robotino XT.

Foram investigadas duas estratégias de adaptação tecnológica:

1. Arquitetura Gateway;
2. Retrofit da unidade computacional utilizando Raspberry Pi 4.

Além da implementação das soluções propostas, o trabalho teve como objetivo avaliar experimentalmente sua viabilidade e identificar limitações arquiteturais, de software e de comunicação que pudessem inviabilizar a modernização da plataforma.

---

# Problema de Pesquisa

O Robotino XT apresentava:

- Hardware AMD Geode LX800 x86;
- Ubuntu 9.04 e kernel RTAI;
- APIs e bibliotecas proprietárias;
- Forte dependência de tecnologias descontinuadas;
- Baixa compatibilidade com ferramentas modernas;
- Dificuldade de integração com ROS 2;
- Restrições impostas pela arquitetura original.

A questão investigada foi:

> É possível modernizar o Robotino XT utilizando Raspberry Pi 4 e tecnologias atuais preservando os subsistemas originais da plataforma?

---

# Objetivo Geral

Investigar a viabilidade de modernização do sistema embarcado do Robotino XT por meio da integração de um Raspberry Pi 4, preservando os subsistemas eletromecânicos originais.

---

# Fundamentação

O estudo foi baseado em:

- Sistemas embarcados legados;
- Retrofit tecnológico;
- Sistemas de tempo real e kernel RTAI;
- Comunicação serial RS232;
- Conversão TTL-RS232;
- ROS (Robot Operating System);
- Obsolescência tecnológica em sistemas robóticos.

---

# Metodologia

O trabalho foi desenvolvido como um estudo de caso experimental.

## Etapa 1 – Revitalização da plataforma

- Recuperação estrutural e elétrica;
- Substituição das baterias;
- Atualização do firmware 2.4;
- Restauração do Ubuntu 9.04;
- Inspeção dos módulos eletrônicos.

---

## Etapa 2 – Validação do sistema legado

Utilizando o Robotino View 2.8.4:

- Motores omnidirecionais;
- Sensores infravelhos;
- Módulo pneumático;
- Comunicação TCP/IP.

Foi constatada a necessidade de substituição de dois sensores IR.

---

## Etapa 3 – Reconfiguração mecânica

Foi realizada:

- Remoção do módulo pneumático;
- Remoção da placa de controle pneumático;
- Redução do consumo energético;
- Liberação de espaço interno para futuras adaptações.

---

## Etapa 4 – Estudo da arquitetura Gateway

Arquitetura proposta:

```
Robotino XT (x86)
        ↓ TCP/IP
Raspberry Pi 4 (ARM)
        ↓
ROS 2
```

Objetivo:

- Manter a CPU original;
- Utilizar o Raspberry Pi como camada de alto nível.

Foram desenvolvidos:

- Servidor TCP baseado na OpenRobotino API;
- Cliente TCP integrado ao ROS 2;
- Comunicação entre os dispositivos.

---

## Etapa 5 – Estudo do Retrofit da UCP

Arquitetura proposta:

```
Raspberry Pi 4
      ↓
MAX3232
      ↓
IO Board
      ↓
Motores e sensores
```

Foram realizados:

- Configuração da UART;
- Conversão RS232 ↔ TTL;
- Testes do daemon controld2;
- Diagnósticos seriais;
- Monitoramento dos serviços Linux.

---

# Resultados

## Revitalização

### Validada

Foram recuperados:

- Inicialização do sistema;
- Firmware original;
- Comunicação em rede;
- Motores;
- Sensores;
- Ambiente legado Robotino View.

A plataforma voltou a operar em sua arquitetura original.

---

## Arquitetura Gateway

### Não validada

Resultados positivos:

- Comunicação TCP/IP estabelecida;
- Servidor e cliente funcionando;
- Troca de dados entre os sistemas.

Limitações encontradas:

- API proprietária disponível apenas para x86;
- Incompatibilidade com arquitetura ARM;
- Dependência do kernel RTAI;
- Ausência de suporte oficial da Festo.

Conclusão:

A arquitetura Gateway foi descartada.

---

## Retrofit da UCP

### Não validado

Resultados positivos:

- UART do Raspberry Pi funcional;
- Conversão TTL ↔ RS232 validada;
- Inicialização do daemon controld2;
- Comunicação parcial com a IO Board.

Problemas encontrados:

- Instabilidade na sincronização serial;
- Reinicializações do serviço;
- Perda de comunicação;
- Falhas de sincronização entre Raspberry Pi e IO Board.

Conclusão:

A substituição da CPU original não pôde ser validada.

---

# Comparação das arquiteturas

## Arquitetura original

Status:

✅ Validada

Características:

- AMD Geode x86;
- Ubuntu 9.04;
- RS232 nativo;
- Forte dependência do RTAI.

---

## Arquitetura Gateway

Status:

❌ Descartada

Limitação principal:

- Incompatibilidade das bibliotecas da OpenRobotino API com ARM.

---

## Retrofit da UCP

Status:

❌ Não validado

Limitação principal:

- Falhas persistentes de sincronização serial.

---

# Repositório do Projeto

Durante todo o desenvolvimento foi mantido um repositório contendo:

- Fotografias;
- Relatórios;
- Códigos-fonte;
- Testes realizados;
- Mapas mentais;
- Diagramas;
- Documentação técnica;
- Registros do processo de desmontagem e revitalização.

Repositório:

https://github.com/DaniMarrieli/tcc-robotino-xt

---

# Contribuições

## Técnica

Recuperação funcional do Robotino XT.

## Científica

Demonstração experimental das limitações do retrofit em plataformas proprietárias.

## Acadêmica

Documentação detalhada do Robotino XT e de sua arquitetura.

## Metodológica

Avaliação de resultados positivos e negativos como parte do estudo de caso.

## Engenharia

Identificação dos gargalos associados a:

- Kernel RTAI;
- APIs proprietárias;
- Dependência da arquitetura x86;
- Comunicação serial;
- Compatibilidade ARM.

---

# Trabalhos Futuros

- Engenharia reversa da IO Board;
- Estudo aprofundado do protocolo serial proprietário;
- Desenvolvimento de novas interfaces de comunicação;
- Substituição parcial ou completa da eletrônica de controle;
- Redução da dependência da infraestrutura proprietária;
- Integração com ROS 2 em arquiteturas independentes.

---

# Síntese

A revitalização da arquitetura original do Robotino XT foi concluída com sucesso.

As duas estratégias de modernização investigadas — Gateway e Retrofit da unidade computacional — apresentaram resultados parciais, porém não puderam ser validadas para aplicação prática devido às limitações impostas pelas bibliotecas proprietárias, pela dependência do kernel RTAI e pelas falhas de sincronização serial.

Dessa forma, o principal resultado deste trabalho não foi a modernização efetiva da plataforma, mas a caracterização experimental dos obstáculos que impedem o retrofit de sistemas robóticos legados baseados em arquiteturas proprietárias, produzindo documentação técnica e conhecimento que podem servir de base para futuras pesquisas.