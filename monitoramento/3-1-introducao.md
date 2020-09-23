# 3.1 Introdução

No Capítulo 2 fizemos o deploy de nossa aplicação no ambiente de produção. Para isso foram criados dois contêineres, cada um com um servidor exigido pela aplicação. Um deles era um Servidor Web rodando Tomcat e o outro um Servidor de Banco de Dados utilizando o MySQL.

Com os servidores no ar, nosso cliente pode fazer uso da Loja Virtual que, em princípio, deveria ficar operacional o tempo todo, ou pelo manos na maior parte do tempo. Isso se refere à qualidade de serviço ou QoS e, em geral, é representada por uma notação do tipo 24x7, que significa que desejamos a aplicação operacional 24 horas por dia nos 7 dias da semana.

Nesse ponto é que entra em ação a equipe de Operação, na garantia da qualidade do serviço a ser prestado.

Para saber se há algo errado com nossa aplicação ou com os serviços e servidores que viabilizam sua execução é necessário um monitoramento constante dos mesmos para que, na ocorrência de algum evento que impossibilite a operação da loja, ações sejam tomadas para colocá-la no ar novamente.

Claramente, realizar esse monitoramento de forma manual é praticamente impossível e, desse modo, faz-se necessária a disponibilidade de ferramentas que realiza esse monitoramento de forma automatizada e, havendo ocorrências que mereçam a ação humana, ela comunicará a equipe de operações para intervir e corrigir o problema, restaurando o serviço e colocando a aplicação no ar novamente, no menor tempo possível.

Exitem várias ferramentas que podem ser utilizadas para essa finalidade. Uma delas é a [Nagios](https://www.nagios.org/). Tal ferramenta possui uma versão paga, com uma interface de configuração gráfica, e uma versão gratuita, denominada [Nagios Core](https://www.nagios.org/projects/nagios-core/), que pode ser configurada por arquivos de configuração. Trata-se de uma ferramenta que está a um bom tempo no mercado, é bastante robusta e, apesar de não ter uma interface muito amigável, faz o trabalho que se propõe.

A título de ilustração, no restante deste capítulo vamos instanciar o Nagios Core para oferecer o suporte de monitoramento ao nossos ambiente de produção e, posteriormente, aos demais ambientes conteinerizados que forem criados. A figura abaixo ilustra o ambiente de produção com a inclusão do Servidor de Monitoramento.

![Ambiente de Produ&#xE7;&#xE3;o Monitorado \(adaptado de Sato \(2018\)\)](../.gitbook/assets/figura-monitoramento-ambiente-producao%20%281%29.png)

Animados para mais esse desafio? Então vamos iniciar a configuração dessa ferramenta.



