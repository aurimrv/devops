# 1.2 DevOps e Docker

Nesta seção faz-se uma introdução a DevOps e qual a sua relação com contêineres e, consequentemente, Docker.

Desenvolvimento e Operações \(DevOps\) corresponde a ideia de unir duas áreas que eram consideradas conflitantes.  A idéia do DevOps é permitir que desenvolvimento, garantia de qualidade \(QA\), operações de TI e segurança da informação \(Infosec\) passem a trabalhar juntas e de maneira bastante integrada \([Kim et al., 2018](http://www.altabooks.com.br/produto/manual-de-devops-como-obter-agilidade-confiabilidade-e-seguranca-em-organizacoes-tecnologicas/)\).

[Kim et al. \(2018\)](http://www.altabooks.com.br/produto/manual-de-devops-como-obter-agilidade-confiabilidade-e-seguranca-em-organizacoes-tecnologicas/) alerta que, em quase toda organização de TI há conflito entre a equipe de desenvolvimento e as operações de TI, necessárias para operacionalizar o desenvolvimento e distribuição do software. Tal conflito resulta em uma queda de desempenho e um tempo cada vez maior na entrega de melhorias nos produtos de software.

Organizações de TI, segundo [Kim et al. \(2018\)](http://www.altabooks.com.br/produto/manual-de-devops-como-obter-agilidade-confiabilidade-e-seguranca-em-organizacoes-tecnologicas/), possuem dois grande objetivos:

* Responder ao cenário competitivo e que muda frequentemente; e
* Fornecer serviço estável, confiável e seguro para o cliente.

Nesse sentido, com frequência, o Desenvolvimento assumirá a responsabilidade de responder ao mercado, corrigindo, implementando novas funcionalidades, e desenvolvendo novos produtos para atender o mercado o mais rápido possível. Por outro lado, as Operações de TI assumirá a responsabilidade de garantir a qualidade de serviço ao cliente, mantendo o ambiente estável e seguro, o que, acaba contrariando o objetivo do Desenvolvimento, em geral, por impossibilitar a introdução de mudanças, resultando no conflito de interesse entre as duas áreas \([Kim et al., 2018](http://www.altabooks.com.br/produto/manual-de-devops-como-obter-agilidade-confiabilidade-e-seguranca-em-organizacoes-tecnologicas/)\).

Basicamente, DevOps introduz uma cultura que permite esse ciclo ser quebrado e a cooperação entre Desenvolvimento e Operações de TI passe a ocorrer efetivamente, e todos saem ganhando.

Os pilares por trás do DevOps é decorrente dos métodos ágeis. Entretanto, juntar ambos nem sempre é uma tarefa trivial, conforme relatado por [Betteley e Skelton \(2017\)](https://www.infoq.com/br/articles/merging-devops-agile/). Eles relatam no artigo intitulado "[Unindo desenvolvimento ágil e DevOps](https://www.infoq.com/br/articles/merging-devops-agile/)" as diferenças entre ágil e DevOps e o que precisa ser feito para que ambos cooperem entre si para prover recursos de melhor qualidade no desenvolvimento de software. 

Se considerarmos as práticas do DevOps em que as equipes de desenvolvimento e operações cooperam entre si, observamos que juntas elas desenvolvem, testam, implementam e monitoram aplicativos com velocidade, qualidade e controle, seguindo o que é chamado de Fluxo \(_Pipiline_\) DevOps. A figura abaixo, extraída de [Torre \(2020\)](https://docs.microsoft.com/pt-br/dotnet/architecture/containerized-lifecycle/), ilustra as etapas previstas num fluxo DevOps e a presença dos contêineres Docker para a realização de determinadas etapas do fluxo.

![Fluxo de trabalho gen&#xE9;rico e para o ciclo de vida do aplicativo em cont&#xEA;ineres do Docker \(extra&#xED;da de Torre \(2020\)\)](https://docs.microsoft.com/pt-br/dotnet/architecture/containerized-lifecycle/docker-application-lifecycle/media/containers-foundation-for-devops-collaboration/generic-end-to-enddpcker-app-life-cycle.png)

Como pode ser observado, há contêineres Docker viabilizando o ambiente de trabalho do Dev, no loop interno de desenvolvimento \(1\); na parte de contrução, integração contínua e teste \(3\), na entrega contínua \(4\); e no ambiente de gerenciamento e execução da aplicação \(5\), referente ao ambiente de procução.

Dento do contexto deste curso, a intenção é que passemos por essas etapas, utilizando uma aplicação exemplo, e que permita realizarmos o fluxo completo de DevOps, ilustrado com contêineres Docker. 

Animados!? Hora de começar a por a mão na massa.











