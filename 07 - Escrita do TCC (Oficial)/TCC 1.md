# TCC I

# Revitalização e Adaptação de Plataforma Robótica Móvel: Um Estudo de Caso com o Robotino XT visando Otimização, Integração e Desenvolvimento de Autonomia mediante uma Arquitetura Híbrida com Raspberry Pi 4 e ROS 2

## Visão Geral

O TCC I definiu o planejamento e a proposta de revitalização do Robotino XT, uma plataforma robótica móvel da Festo que se encontrava limitada pela obsolescência de seu hardware e software.

O trabalho foi concebido como um projeto de retrofit da unidade central de processamento, visando transformar a plataforma em um robô móvel preparado para aplicações modernas utilizando Raspberry Pi 4 e ROS 2.

---

# Problema de Pesquisa

O Robotino XT apresentava:

- Hardware baseado em AMD Geode x86;
- Ubuntu 9.04 e firmware 2.4;
- Dependência de tecnologias descontinuadas;
- Baixa compatibilidade com sistemas modernos;
- Dificuldade de integração com sensores atuais;
- Restrições para utilização de ROS 2;
- Elevado consumo energético do módulo pneumático.

Diante desse cenário, surgiu a necessidade de investigar formas de prolongar a vida útil da plataforma através do retrofit e da integração com tecnologias modernas.

---

# Objetivo Geral

Revitalizar e realizar o retrofit do Robotino XT por meio da substituição da unidade central de processamento original pelo Raspberry Pi 4 executando ROS 2, preparando a plataforma para aplicações de navegação autônoma em ambiente controlado.

---

# Objetivos Específicos

## Analisar a arquitetura original

- Hardware embarcado;
- Firmware;
- Sistema operacional;
- Periféricos;
- Limitações de integração.

## Reconfigurar o hardware

- Remover o manipulador pneumático;
- Eliminar o circuito eletropneumático;
- Transformar a plataforma em um robô móvel puro.

## Substituir a unidade central de processamento

- Remover o AMD Geode;
- Integrar o Raspberry Pi 4;
- Estabelecer comunicação bidirecional com a IO Board.

## Desenvolver navegação autônoma

- Integrar ROS 2;
- Processar odometria e sensores;
- Executar algoritmos de navegação em ambiente controlado.

## Documentar o processo

- Registrar desafios;
- Avaliar desempenho;
- Produzir material para futuras pesquisas.

---

# Fundamentação

O trabalho foi fundamentado em:

- Obsolescência tecnológica em sistemas industriais;
- Retrofit digital na Indústria 4.0;
- Sustentabilidade e reaproveitamento de ativos;
- ROS (Robot Operating System);
- Arquiteturas distribuídas e sistemas embarcados modernos.

---

# Metodologia Proposta

O desenvolvimento foi organizado em cinco fases.

## Fase 1 – Diagnóstico e validação

- Atualização do firmware;
- Testes utilizando Robotino View 2.8.4;
- Verificação dos motores;
- Verificação dos sensores;
- Testes do sistema pneumático.

## Fase 2 – Reconfiguração física

- Remoção do manipulador pneumático;
- Redução do consumo energético;
- Preparação para aplicações de mobilidade.

## Fase 3 – Retrofit da unidade computacional

- Remoção do AMD Geode;
- Instalação do Raspberry Pi 4;
- Instalação do Ubuntu Server e ROS 2;
- Comunicação com a IO Board através do MAX232.

## Fase 4 – Arquitetura distribuída

Integração:

```
Robotino XT
     ↓
Raspberry Pi 4
     ↓
ROS 2
     ↓
Computador externo
```

Permitindo:

- Publicação de odometria;
- Leitura de sensores;
- Envio de comandos de movimentação.

## Fase 5 – Implementação da autonomia

- Desenvolvimento de algoritmos de navegação;
- Testes no Laboratório 2314 da UNIFEI;
- Avaliação da autonomia e desempenho.

---

# Materiais Utilizados

## Hardware

- Robotino XT;
- Raspberry Pi 4;
- Computador externo;
- Compact Flash;
- Cartão MicroSD;
- Fonte de alimentação;
- MAX232CPE.

## Software

- Ubuntu 9.04;
- Robotino View 2.8.4;
- Ubuntu Server 22.04;
- ROS 2;
- Drivers e ferramentas Linux.

---

# Tasks Definidas

T1 – Validação do hardware legado.

T2 – Otimização física da plataforma.

T3 – Instalação da nova unidade computacional.

T4 – Integração Raspberry Pi ↔ IO Board.

T5 – Implementação da arquitetura distribuída.

T6 – Desenvolvimento da navegação autônoma.

T7 – Validação experimental.

T8 – Documentação e análise crítica.

T9 – Relatório e defesa.

---

# Resultados Esperados

Esperava-se que ao final do projeto:

- O Robotino XT estivesse completamente revitalizado;
- O AMD Geode fosse substituído pelo Raspberry Pi 4;
- A IO Board permanecesse em operação;
- A comunicação fosse integrada ao ROS 2;
- O robô executasse navegação autônoma;
- Uma arquitetura híbrida fosse validada;
- A plataforma servisse como referência para retrofits futuros.

---

# Contribuições Esperadas

## Técnica

Modernização da plataforma Robotino XT.

## Acadêmica

Base para pesquisas futuras em robótica móvel.

## Econômica

Prolongamento da vida útil de equipamentos legados.

## Sustentabilidade

Reaproveitamento de ativos existentes.

## Científica

Proposição de uma arquitetura híbrida para modernização de plataformas robóticas.

---

# Cronograma

Período previsto:

Abril de 2025 → Junho de 2026

Organizado em nove tasks distribuídas ao longo de quinze meses.

---

# Síntese

O TCC I representou a fase de planejamento do projeto, estabelecendo a estratégia de revitalização do Robotino XT através do retrofit da unidade computacional original e da implementação de uma arquitetura híbrida baseada em Raspberry Pi 4 e ROS 2, com o objetivo final de obter uma plataforma capaz de executar navegação autônoma em ambiente controlado.