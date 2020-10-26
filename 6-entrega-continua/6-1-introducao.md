# 6.1 Introdução

No capítulo anterior tratamos da parte da Integração Contínua que permite, a cada commit, confirmando mudanças realizadas, contruir toda a aplicação se for necessário, demonstrando que o código não quebrou. 

Nessa seção, iremos incluir um passo a mais no nosso arquivo de configuração do pipeline de modo que possa ser feito o deploy da aplicação e a mesma possa ser utilizada em um ambiente de produção, conforme já realizanos nos campítulos iniciais.

Na verdade, como estamos trabalhando com um servidor Tomcat local, faremos aqui uma simplificação do processo e ainda demandaremos um passo manual para a subida da versão atualizada da aplicação no Servidor Tomcat mas, caso estivessemos com um servidor Tomcat na nuvem, e com acesso por meio de um IP fixou ou DNS, o scropt de Entrega Contínua poderia facilmente atualizar o arquivo .war nesse servidor remoto e disparar a sua reinicialização pra que as alterações tivessem efeito de forma totalmente automatizada \(trataremos disso em uma nova edição deste livro\).

Nesta versão, o que desejamos realizar é, uma vez que a aplicação seja construída com sucesso e o arquivo `lojavirtualdevops.war` gerado, o mesmo será empacotado dentro da imagem do servidor Tomcat e poderá ser utilizado pelo nosso arquivo do `docker-compose` para subrir uma nova versão da aplicação.

Um passo importante pra realizarmos essa tarefa é a personalização/construção de uma imagem Docker e seu posterior carregamento dentro de um servidor de imagens que, no caso, utilizaremos o Hub Docker. A seção a seguir detalha esse processo antes de darmos início a alteração no pipeline CI/CD.

### Personalizando Imagens do Docker e Disponibilizando no Hub Docker



