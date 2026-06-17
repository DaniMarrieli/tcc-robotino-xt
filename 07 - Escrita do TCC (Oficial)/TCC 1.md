\textbf{{\Large Revitalização e Adaptação de Plataforma Robótica Móvel: Um Estudo de Caso com o Robotino XT Visando Otimização, Integração e Desenvolvimento de Autonomia Mediante uma Arquitetura Híbrida com Raspberry Pi 4 e ROS2}} \\ %\vspace{0.3cm}
%\hrulefill \\
\vfill
\begin{flushleft}
\textbf{Discente}: Danielle Menezes Marrieli\hfill{}\\
\textbf{Orientador}: André Chaves Magalhães\hfill{}\\
\textbf{Coorientador}: Diogo Leonardo Ferreira Silva\hfill{}\\
\textbf{Curso}: Engenharia de Controle e Automação
\end{flushleft}

\vfill


% \vspace{\stretch{1}}
% \begin{center}
% \large{\bf{\today}}
% \end{center}
\end{titlepage}


\pagenumbering{arabic}

\begin{abstract}
O presente trabalho propõe a revitalização de um robô móvel, da linha Robotino XT, que se encontra obsoleto devido aos sistemas legados de hardware e software, mediante sua restauração sistêmica e retrofit de unidade central de processamento. O escopo do projeto abrange a reconfiguração de hardware, com a remoção do módulo pneumático, visando à otimização da plataforma e o desenvolvimento de uma arquitetura híbrida utilizando um Raspberry Pi 4 com ROS 2 como unidade primaria de processamento do Robotino XT, para aprimoramento das funcionalidades de programação e implementação de periféricos atuais a fim de promover o melhor desempenho em algoritmos de navegação autônoma. O objetivo final consiste no retrofit do robotino, contando com desenvolvimento e implementação de algoritmos de navegação autônoma e percepção, capacitando o robô para a execução de tarefas em ambientes controlados, por meio de uma arquitetura de controle híbrida através do ROS externo.

\end{abstract}


%---------------------------------------------------------------------------------------------------------
\section{Introdução}

A evolução acelerada da Indústria 4.0 tem impulsionado avanços significativos nas áreas de automação e controle, especialmente no desenvolvimento de manipuladores robóticos e robótica móvel. Essas inovações proporcionam maior eficiência, flexibilidade e integração de sistemas, ampliando as possibilidades de aplicação em ambientes industriais e acadêmicos. Contudo, essa rápida evolução também evidencia um desafio recorrente: a obsolescência tecnológica de equipamentos com sistemas de hardware e software legados, que perdem o suporte dos fabricantes e têm sua capacidade de integração severamente limitada, demonstrando dificuldades para acompanhar as novas demandas da indústria.

Essa realidade resulta na perda do investimento inicial em máquinas e equipamentos, atingindo não somente o setor industrial, mas também sistemas utilizados em ambientes de pesquisa e aprendizagem, como é o caso do Robotino XT, da Festo — objeto de estudo deste trabalho. O Robotino XT é uma plataforma robótica de pesquisa composta por uma base móvel omnidirecional e um manipulador biónico eletropneumático, como ilustrado na Figura \ref{fig:fig_1}, totalizando 12 graus de liberdade: 3 na base móvel e 9 no manipulador. Adquirido para fins didáticos e de pesquisa universitária, com o passar do tempo e a perda de suporte por parte da fabricante, as aplicações e atualizações da plataforma foram limitadas, principalmente pela dificuldade de integração com periféricos modernos. Entretanto, tais limitações tornaram possível o desenvolvimento e condução desta pesquisa, que aborda o retrofit de uma plataforma robótica legada.

\begin{figure}[H]
	\centering
	\includegraphics[scale=0.25]{Figuras/RoboFesto.jpg}
	\caption{Robotino XT}
	\label{fig:fig_1}
\end{figure}

A arquitetura original do Robotino XT baseia-se em um computador embarcado com o sistema operacional Ubuntu 9.04 e firmware versão 2.4, compatíveis com os modelos Robotino 1 e 2, datados de aproximadamente 2013. Essa combinação de hardware e software, embora robusta para a época, caracteriza o Robotino XT como uma plataforma robótica legada, cuja atualização e integração com sistemas modernos de controle são dificultadas pela ausência de suporte, uma vez que bibliotecas e protocolos de comunicação recentes não são compatíveis nem reconhecidos.

O problema da obsolescência de plataformas robóticas é um desafio reconhecido na indústria. Conforme apontado por \cite{alqoud2022}, diante da necessidade de integrar digitalização e conectividade no contexto da Indústria 4.0, as empresas enfrentam uma escolha estratégica: substituir seus ativos legados ou modernizá-los. Embora a substituição possa trazer resultados a curto prazo, essa opção tende a ser excessivamente cara e contrária aos princípios de produção sustentável, uma vez que descarta máquinas ainda funcionais. Em resposta a esse dilema, o retrofit digital tem se tornado uma alternativa cada vez mais atraente e custo-efetiva. O processo de modernização de equipamentos com hardware e software legados não apenas estende a vida útil da infraestrutura industrial, mas também melhora sua interoperabilidade e contribui para a sustentabilidade. Contudo, a implementação do retrofit ainda apresenta desafios para pequenas e médias empresas (PMEs), principalmente devido à complexidade dos sistemas existentes e à variedade de protocolos e sistemas operacionais.

Nesse contexto, pesquisas recentes têm explorado soluções orientadas ao desenvolvimento de sistemas robóticos que se afastam de arquiteturas monolíticas e autossuficientes, em direção a abordagens modulares e distribuídas. O uso de Single Board Computers (SBCs) como o Raspberry Pi, em conjunto com middlewares de robótica como o Robot Operating System (ROS) \cite{Quigley2009}, tem emergido como estado da arte para conceder inteligência e capacidade de percepção a robôs. Tais plataformas, por sua alta capacidade de processamento, baixo custo e amplo suporte de drivers, são utilizadas como “computadores de bordo” para processar dados de sensores avançados, como LIDAR e câmeras de alta resolução, e executar algoritmos de navegação complexos.

Além das limitações de integração, outro problema identificado no Robotino XT é uma redução consideravelmente rapida da carga das bateria devido ao consumo energético do circuito eletropneumático que controla o manipulador biónico, limitando também algumas aplicações práticas que poderiam ser implementadas na plataforma, como é abordado no trabalho do \cite{Rawashdeh2019}. 

Diante desse cenário, o presente trabalho propõe a revitalização e reconfiguração de hardware do Robotino XT por meio da remoção do módulo pneumático e do circuito eletropneumático, visando à otimização da plataforma e à ampliação de suas aplicações. O retrofit será realizado mediante a substituição da unidade de processamento central AMD Geode, componente principal do sistema embarcado em sua arquitetura original, por um Raspberry Pi 4 \cite{OpenRobotino_Raspberry}, o qual dará suporte a outras funcionalidades, como percepção avançada por câmera e controle de navegação via ROS2. Assim, a solução adotada não busca substituir o Robotino XT, mas complementar sua arquitetura original, aproveitando suas capacidades consolidadas no controle de motores e leitura de odometria, enquanto expande suas funcionalidades por meio de uma arquitetura híbrida.


%---------------------------------------------------------------------------------------------------------
\section{Justificativa}

A escolha do tema deste trabalho foi motivada pela necessidade crescente de modernização e revitalização de plataformas robóticas legadas, especialmente no contexto da Indústria 4.0, que demanda sistemas cada vez mais integrados, eficientes e autônomos. O Robotino XT, plataforma disponível no laboratório e com grande potencial didático e de pesquisa, apresenta limitações decorrentes de sua arquitetura tecnológica antiga, o que o torna um caso ideal para investigação e desenvolvimento de soluções que prolonguem sua vida útil e ampliem suas funcionalidades, como a utilização de ROS2 para desenvolvimento de algoritmos de navegação e controle \cite{Borse2024}.

A relevância deste estudo está em contribuir para a área de automação e robótica, propondo uma abordagem prática para a atualização de sistemas embarcados legados, com foco na melhoria da sua capacidade de funcionamento e na integração de arquiteturas híbridas de comando. Essa contribuição é importante para reduzir custos associados à substituição completa de equipamentos, além de promover a sustentabilidade tecnológica e o reaproveitamento de investimentos já realizados.

A viabilidade do projeto é garantida pela infraestrutura disponível, que inclui o próprio Robotino XT, equipamentos de suporte e conhecimento técnico adquirido nas disciplinas do curso, como sistemas embarcados, robótica móvel, automação e redes industriais. Essa integração entre teoria e prática fortalece o aprendizado e possibilita a aplicação dos conteúdos acadêmicos em um projeto real e desafiador, alinhando-se aos objetivos do curso e às necessidades do mercado.


%----------------------------------------------------------------------------------------------------------------------
\section{Objetivos}

\subsection{Objetivo Geral}

Revitalizar e realizar o retrofit da plataforma robótica móvel Robotino XT por meio da substituição da sua unidade central de processamento original pelo Raspberry Pi 4 embarcado com ROS 2, reconfigurando sua arquitetura de controle para integrar sensores e periféricos modernos e prepará-la para a execução de algoritmos de navegação autônoma em ambiente controlado. Essa revitalização visa prolongar a vida útil da plataforma legada, ampliar suas capacidades operacionais e garantir sua aplicabilidade em contextos acadêmicos e de pesquisa alinhados às tecnologias atuais.

\subsection{Objetivos Específicos}

Para alcançar o objetivo geral, os seguintes objetivos específicos serão investigados e desenvolvidos:

\begin{itemize} \item \textbf{Análise da arquitetura original, atualização e verificação do sistema:}
Analisar detalhadamente a arquitetura de hardware e software do Robotino XT, incluindo sistema embarcado, firmware, sistema operacional e periféricos, a fim de identificar limitações que dificultam sua integração com tecnologias modernas e, após validar a estabilidade do sistema por meio da atualização do firmware e testes de funcionalidade, estabelecer a base necessária para sua posterior reconfiguração.

\item \textbf{Reconfiguração de hardware:}  
Remover o manipulador e o circuito de controle eletropneumático do Robotino tornando a plataforma mais adequada para aplicações de mobilidade pura.

\item \textbf{Substituição da unidade central de processamento:}
Implementar uma nova arquitetura de controle para o Robotino XT por meio da substituição da unidade central de processamento original (AMD Geode) por um Raspberry Pi 4 executando ROS 2, estabelecendo uma comunicação bidirecional estável entre o sistema embarcado do robô e a nova unidade de processamento, de forma a possibilitar o envio de comandos de movimento e o recebimento de dados de odometria e sensores, preparando a plataforma para a posterior integração e execução de algoritmos de navegação autônoma.


\item \textbf{Desenvolvimento e implementação de algoritmos de navegação autônoma:}  
Desenvolver e integrar algoritmos de navegação autônoma utilizando o ROS 2, capazes de interpretar os dados de odometria do Robotino XT e as leituras de sensores e periféricos posteriormente adicionados, a fim de permitir que a plataforma execute comandos de locomoção e responda de forma autônoma às condições estabelecidas em um ambiente controlado definido.

\item \textbf{Documentação e análise crítica da revitalização:}  
Registrar de forma sistemática todas as etapas do processo de retrofit do Robotino XT, destacando os desafios encontrados, as soluções adotadas e as lições aprendidas. Além disso, avaliar a eficácia da arquitetura híbrida resultante em termos de desempenho, autonomia e capacidade de integração com sensores e periféricos, identificando limitações remanescentes e possíveis aprimoramentos e contribuições futuras para a comunidade acadêmica e para o desenvolvimento de plataformas robóticas legadas.

\end{itemize}


%---------------------------------------------------------------------------------------------------------
\section{Metodologia}

O desenvolvimento deste trabalho será conduzido com base em uma abordagem experimental aplicada à revitalização e ao retrofit da plataforma robótica Robotino XT. A metodologia busca validar a viabilidade de substituir sua unidade central de processamento original por um Raspberry Pi 4 embarcado com ROS 2 e implementar uma arquitetura de controle híbrida capaz de suportar navegação autônoma em ambiente controlado.

Para assegurar coerência, organização e progressão lógica das etapas, o projeto será estruturado em cinco fases de desenvolvimento, seguidas da fase final de fechamento e relatório e defesa do TCC 2, conforme distribuído ao longo dos meses de trabalho no cronograma apresentado através do Gráfico de Gantt Simplificado.

\subsection{Desenvolvimento}

\subsubsection{Diagnóstico, validação e reconfiguração de hardware}

\begin{itemize}
    \item Atualização do firmware do Robotino XT para a versão 2.4 no sistema operacional Ubuntu 9.04.

    \item Verificação da funcionalidade do sistema embarcado original por meio de testes com Robotino View 2.8.4 (conectividade IP, motores, sensores, módulo pneumático).

    \item Após validada a integridade funcional, remoção do manipulador e do circuito de controle eletropneumático para caracterização da plataforma como robô móvel puro.
\end{itemize}

\subsubsection{Retrofit e substituição da unidade central de processamento}
\begin{itemize}
    \item Remoção da unidade AMD Geode e embarcados interligados a ela e instalação do Raspberry Pi 4 como nova UCP.

    \item Instalação de SO compativel e drivers da placa de controle principal (I/O board).

    \item Configuração elétrica e lógica de comunicação entre o Raspberry Pi e a IO Board do Robotino XT.

\end{itemize}

\subsubsection{Configuração da arquitetura distribuída}
\begin{itemize}
    \item Integração do Robotino XT com o Raspberry Pi 4 e o computador externo através da arquitetura de controle distribuída.
    
    \item Configuração dos tópicos ROS 2 que publicarão odometria, sensores e comandos de movimento.
    
    \item Sincronização dos fluxos de dados para leitura de odometria e sensores no ROS 2.

    \item Configuração do envio de comandos de locomoção da camada de alto nível para o Robotino.

    \item Garantia de estabilidade e tempo de resposta adequado para aplicações de navegação.
\end{itemize}

\subsubsection{Implementação da Autonomia e Validação Experimental}
\begin{itemize}
    \item Desenvolvimento dos algoritmos de navegação no computador externo utilizando ROS 2.

    \item Integração dos sensores e fontes de odometria no pipeline de tomada de decisão.

    \item Testes práticos da navegação autônoma em ambiente controlado (Laboratório 2314 da UNIFEI), verificando respostas à diferentes configurações do ambiente.
\end{itemize}

\subsubsection{Validação experimental e análise de desempenho}
\begin{itemize}
    \item Avaliação da eficácia da autonomia e da navegação em ambiente controlado.

    \item Medição de estabilidade da arquitetura distribuída, precisão da odometria e autonomia.

    \item Identificação de limitações e potenciais melhorias para trabalhos futuros.
\end{itemize}

\subsubsection{Fechamento, relatório e defesa}
\begin{itemize}
    \item Consolidação e análise crítica dos resultados obtidos.

    \item Documentação completa do processo, desafios enfrentados e lições aprendidas.

    \item Redação final do TCC e preparação da apresentação para a defesa.
\end{itemize}

\section{Materiais e Métodos}

O desenvolvimento deste trabalho foi conduzido por meio da execução de um protótipo experimental, com o objetivo de validar a viabilidade da revitalização da plataforma robótica Robotino XT mediante retrofit e substituição da sua unidade central de processamento. Para garantir rastreabilidade, organização e alinhamento com os objetivos definidos, os materiais e procedimentos foram estruturados em tarefas sequenciais (tasks), descritas a seguir.

\subsection{Materiais e Equipamentos}

Os materiais e equipamentos utilizados ao longo do projeto são listados a seguir:

\begin{itemize}
    \item \textbf{Robotino XT:} Plataforma robótica móvel submetida ao processo de retrofit.
    \item \textbf{Raspberry Pi 4:} Nova unidade de processamento embarcada.
    \item \textbf{Computador externo:} Unidade de processamento de alto nível responsável pela execução dos algoritmos de navegação autônoma.
    \item \textbf{Cartão CompactFlash:} Utilizado para gravação e atualização do firmware do Robotino XT.
    \item \textbf{Cartão MicroSD:} Necessário para instalação do sistema operacional no Raspberry Pi 4.
    \item \textbf{Fonte de alimentação:} Responsável pelo carregamento das baterias do Robotino.
    \item \textbf{Transceptor serial MAX232CPE:} Conversor de níveis lógicos RS\textendash232/TTL destinado à comunicação segura entre a I/O Board do Robotino XT e o Raspberry Pi 4.
\end{itemize}

\subsection{Métodos (Tasks)}

As atividades de desenvolvimento foram organizadas em tasks, permitindo planejamento adequado, distribuição temporal e rastreabilidade dos resultados obtidos.

\begin{enumerate}[label=\textbf{T\arabic*}]
    \item \textbf{Validação do hardware legado:} Atualizar o firmware do Robotino XT para a versão 2.4 e verificar a operação do sistema embarcado original mediante testes via Robotino View, incluindo comunicação IP, motores, sensores e módulo pneumático.
    
    \item \textbf{Otimização física da plataforma:} Remover o manipulador eletropneumático e seu circuito de controle, visando redução do consumo energético, aumento da autonomia e preparação da plataforma para operação exclusivamente como robô móvel.
    
    \item \textbf{Instalação e preparação da nova unidade de processamento:} Instalar Ubuntu Server 22.04 e ROS2 no Raspberry Pi 4, configurar a rede e preparar o ambiente de execução do sistema de controle.
    
    \item \textbf{Conexão segura entre Raspberry Pi e Robotino XT:} Integrar eletricamente o Raspberry Pi 4 à I/O Board utilizando o transceptor MAX232CPE para conversão de nível lógico RS\textendash232/TTL e validar a comunicação serial entre os dispositivos.
    
    \item \textbf{Integração da arquitetura de controle distribuída:} Configurar a comunicação bidirecional Robotino~XT $\leftrightarrow$ Raspberry~Pi~4 $\leftrightarrow$ computador externo, disponibilizando dados de odometria e sensores no ROS~2 e permitindo o envio de comandos de locomoção ao robô.
    
    \item \textbf{Desenvolvimento e implementação de algoritmos de navegação autônoma:} Desenvolver algoritmos de navegação utilizando ROS~2, integrando dados de odometria e sensores para permitir resposta autônoma às condições do ambiente controlado definido como o Laboratório 2314 da UNIFEI.
    
    \item \textbf{Validação experimental e análise de desempenho:} Avaliar a eficácia da navegação autônoma e da arquitetura híbrida quanto à estabilidade da comunicação, precisão da odometria, autonomia energética e desempenho geral do sistema.
    
    \item \textbf{Documentação e análise crítica:} Registrar detalhadamente o processo de retrofit, os desafios enfrentados, as soluções implementadas e as lições aprendidas, gerando material de suporte para discussão dos resultados e defesa do TCC.
    
    \item \textbf{Relatório e defesa:} Consolidar os resultados, finalizar o texto da monografia e preparar os materiais necessários para defesa pública do TCC.
\end{enumerate}

%---------------------------------------------------------------------------------------------------------
\section{Resultados Esperados}
Espera-se que a realização deste trabalho resulte não apenas na documentação de um processo de modernização, mas na demonstração prática da viabilidade do retrofit de uma plataforma robótica obsoleta. Ao final do projeto, o Robotino XT deverá estar revitalizado por meio da substituição da sua unidade central de processamento original pelo Raspberry Pi 4 executando ROS 2, operando como um robô móvel funcional e capaz de executar navegação autônoma em ambiente controlado com funcionalidades de controle mais robustas. 

O resultado técnico central esperado consiste na implementação de uma arquitetura de controle híbrida e distribuída, em que o hardware e a placa IO da plataforma são preservados e complementados pela nova unidade de processamento central. Assim, o robô deverá ser capaz de interpretar dados de odometria e sensores, processá-los por meio dos algoritmos desenvolvidos em ambiente de programação robusto e atualizado e responder de maneira autônoma às condições do ambiente em que será inserido. 

Além da prova prática de funcionamento, espera-se que o trabalho produza um modelo de referência replicável para futuros retrofits de plataformas robóticas em situação de obsolescência, documentando desafios enfrentados, soluções implementadas e recomendações de boas práticas. Essa contribuição permitirá que outras instituições de ensino e pesquisa, bem como laboratórios com equipamentos antigos, disponham de uma metodologia clara e acessível para prolongar a vida útil de seus sistemas robóticos por meio da integração de tecnologias atuais. 

Finalmente, espera-se que os resultados forneçam evidências de que a digitalização de sistemas robóticos legados pode ser tecnicamente viável, economicamente vantajosa e alinhada a práticas de sustentabilidade. Ao demonstrar que é possível habilitar funcionalidades avançadas a partir da modernização de estruturas existentes, este trabalho busca abrir caminho para futuras pesquisas e iniciativas que valorizem a reintegração de equipamentos antes considerados obsoletos.
\newpage
% =========================================================
\section{Cronograma de Execução}

Com base nas Tasks definidas na seção de Materiais e Métodos, o cronograma de execução apresentado no Quadro 1 distribui as atividades do projeto ao longo de 15 meses a partir do início do desenvolvimento. Cada barra representa o período previsto para execução de cada Task, permitindo visualizar a sobreposição de etapas, a sequência lógica do desenvolvimento e os marcos de conclusão do projeto, tendo seu inicio em abril de 2025 e término previsto para junho de 2026.
