# 5.2 Controle de Versão

O primeiro passo para se trabalhar com a Integração Contínua diz respeito ao uso e um sistema de controle de versão \(_Control Version System_ - CVS\), responsável por gerenciar as mudanças realizadas no código fonte do produto ou da infra estrutura. 

O uso do CVS facilita imensamente o gerenciamento e recuperação de versões específicas do nosso produto, além da manutenção do histórico das alterações realizadas ao longo do tempo.

Toda mudança realizada no código passa a valor após passarem por um _commit_ \(confirmação\). O _commit_ agrupa o conjunto de arquivos alterados e permite a inclusão de uma mensagem referente as alterações realizadas e/ou o que demandou tais alterações.

O CVS permite depois que se navegue pelos diferentes _commits_ realizados, sabendo-se o que foi alterado, quem fez a alteração, e em que momento a alteração foi feita, dentre outras informações. Além disso, alterações concorrentes num mesmo arquivo podem gerar algum conflito que o CVS é capaz de detectar e tentar resolver de forma automática ou solicitando a intervenção humana para tal.

Desse modo, fazer uso de um CVS é obrigatório nos dias atuais e existem inúmeras opções disponíveis sendo uma das mais conhecidas o Git \([https://git-scm.com/](https://git-scm.com/)\). O Git é um CVS distribuído e que não exige a existência de um único repositório central para todos os _commits_ de um projeto. Cada desenvolvedor pode ter sua instância local do Git e diferentes Gits podem compartilhar os históricos de mudanças locais para outra instância de Git executando localmente em outra máquina. Desse modo, a opção por um servidor central é uma mera conveniência mas é possível utilizar o Git de forma distribuída sem a existência de um repositório central.

Práticas de DevOps como a Integração Contínua e a Entrega Contínua demandam extrema cooperação e disciplina dos envolvidos e o CVS facilita imensamente o controle dessa cooperação. Além disso, o que dispara a integração ou a entrega contínuas são os _commits_. Qualquer alteração que precise chegar até o ambiente de produção irá sempre iniciar com o _commit_ das alterações demandadas.

Hoje existem diferentes ambientes que utilizam-se do Git para fornecer ferramentas de cooperação para o desenvolvimento de software e o processo de Integração Contínua \(CI\) e Entrega Contínua \(CD\). Dois exemplos bastante conhecidos são o GitHub \([https://github.com/](https://github.com/)\) e o GitLab \([https://gitlab.com](https://gitlab.com)\). Ambos permitem a criação de contas gratuitas, a criação de projetos, e a definição de pipelines de CI/CD. 

Por exemplo, no caso do GitHub, ele foi utilizado até o presente momento para gerenciar a configuração do projeto exemplo adotado neste livro, disponível em [https://github.com/aurimrv/loja-virtual-devops](https://github.com/aurimrv/loja-virtual-devops),  que é um fork atualizado do projeto original disponível em [https://github.com/dtsato/loja-virtual-devops](https://github.com/dtsato/loja-virtual-devops).

O GitHub oferece ainda inúmeras ferramentas para a cooperação, relato de defeitos, solicitação de novas funcionalidades, e mais recentemente o chamado [GitHub Actions](https://github.com/features/actions), que viabiliza a criação de fluxos DevOps dentro do GitHub permitindo processos de CI/CD, dentre outras.

Outro sistema de colaboração baseado em Git bastante popular é o GitLab. Basicamente ele oferece as mesmas funcionalidades do GitHub e pode até mesmo ser integrado a este. É possível criar projetos no GitLab importando projeto do GitHub e, posteriormente, manter a sincronia entre os mesmos. Além disso, o [GitLab oferece também uma ferramenta para a criação de fluxos CI/CD](https://docs.gitlab.com/ee/ci/) que já é mais madura que o [GitHub Actions](https://github.com/features/actions) e é a que utilizaremos nas seções a seguir para exemplificar a criação de um fluxo DevOps de Integração Contínua e Entrega Contínua.

Como em ambos os casos, o CVS que executa por baixo do GitHub ou do GitLab é o Git, a execução do Git via linha de comando é idêntica. Por exemplo, para verificar o log de _commits_ no clone local do repositório acima em minha máquina, basta executar o comando `git log` dentro do repositório, conforme demonstrado a seguir.

```text
cd ~/temp/loja-virtual-devops
$ git log
commit 4a64972460fdb478f9219c9ca0f188c5c004746d (HEAD -> master, origin/master, origin/HEAD)
Author: Auri Vincenzi <aurimrv@yahoo.com.br>
Date:   Fri Oct 23 02:48:26 2020 +0000

    Update .gitlab-ci.yml

commit b76f569ae5ac30953b4aa26938f8051e2b5f8c48
Author: Auri Vincenzi <aurimrv@yahoo.com.br>
Date:   Thu Oct 22 18:26:17 2020 +0000

    Update .gitlab-ci.yml

...
```

Como pode ser observado acima, a cada alteração confirmada no repositório, o Git registra as informações e guarda o seu histórico. Sendo um repositório distribuído, para identificar unicamente cada uma das confirmações recebidas, ele calcula um código hash, ou SHA. No exemplo acima observam-se dois _commits_, um iniciando por `b76f569a` e o outro, mais recente, iniciando por `4a649724`. Caso seja necessário reverter o código para algum desses, a chave hash funciona como um identificador único \(chave primária\) para referenciar o respectivo _commit_.

Entretanto, essa não é a única forma de referenciarmos alterações em nossos códigos Git, é possível também atribuir tags a determinadas configurações de nosso repositório e utilizar essas tags como referências para se obter ou restaurar o código a determinado momento do tempo. Para mais informações sobre Git recomenda-se a leitura da documentação oficial disponível em  [https://git-scm.com/doc](https://git-scm.com/doc), ou busca na Internet pois existem inúmeros documentos sobre o seu uso.

Na sequência, veremos como utilizar um contêiner Docker para realizar o processo de build de nosso projeto e, em seguida, vamos o GitLab integrado com o GitHub para darmos início ao processo de desenvolvimento do pipeline DevOps.

