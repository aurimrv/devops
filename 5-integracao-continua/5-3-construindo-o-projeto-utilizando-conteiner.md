# 5.3 Construindo o Projeto Utilizando Contêiner

A construção de um projeto de software, em geral, demanda uma série de atividades até que o código em baixo nível esteja disponível para execução.

Quando se inicia no mundo da programação, nas matérias introdutórias, não nos preocupamos muito com isso pois desenvolvemos projetos simples que podem ser compilados facilmente dentro de um ambiente de desenvolvimento por meio de um clique. Entretanto, quando pensamos em grandes projetos que precisam ser construídos de forma automatizada passa a ser de fundamental importância as chamadas ferramentas de construção \(_build_\) de software.

Existem inúmeras delas disponíveis para as mais diferentes linguagens de programação. Por exemplo, no caso de [C](http://csapp.cs.cmu.edu/3e/docs/chistory.html)/[C++](https://en.wikipedia.org/wiki/C%2B%2B) temos o famoso [Make](https://www.gnu.org/software/make/), para [Ruby](https://www.ruby-lang.org/pt/) tem-se o [Rake](https://ruby.github.io/rake/), para [JavaScript](https://www.javascript.com/) tem-se o [Grunt](https://gruntjs.com/), para [.Net](https://dotnet.microsoft.com/languages) tem-se o [NAnt](http://nant.sourceforge.net/), e para [Java](https://docs.oracle.com/en/java/) têm-se o [Ant](https://ant.apache.org/), o [Maven](https://maven.apache.org/), o [Gradle](https://gradle.org/), dentre outros.

Todos eles apresentam funções bem similares e têm a missão de, a partir de um determinado conjunto de código fonte e demais recursos, resolver as dependências, ou seja, encontrar todas as bibliotecas externas que esse código fonte referencia, e construir o código de baixo nível da aplicação. Durante esse processo, é possível também executar os testes no código para se ter certeza de que tudo está operacional, gerar relatórios sobre o produto, gerar a documentação, dentre outras tarefas, de modo que, se todas forem completadas com sucesso, conseguiremos chegar ao resultado final que é a aplicação em um nível de abstração que possa ser executada.

No caso do nosso projeto exemplo, ele utiliza o [Maven](https://maven.apache.org/) como ferramenta de build. O processo de construção da aplicação está configurado no arquivo denominado `pom.xml`, localizado na raiz do repositório do projeto. É nesse arquivo que se determina quais são, por exemplo, as dependências que a loja precisa pra ser compilada, testada, e empacotada.

Um cenário típico de construção da aplicação até o seu empacotamento, passa por por uma série de objetivos \(_goals_\) na terminologia do Maven. Alguns objetivos comuns são:

* `mvn compile`: tem o objetivo de compilar todo código fonte da aplicação;
* `mvn test-compile`: tem o objetivo de compilar todo o código de teste da aplicação;
* `mvn test`: tem o objetivo de executar os testes na aplicação;
* `mvn integration-test`: tem o objetivo de executar os teste de integração na aplicação;
* `mvn verify`: tem o objetivo de executar tanto os testes unitários quanto de integração na aplicação. Equivalente a um `mvn test integration-test`;
* `mvn package`: tem o objetivo de empacotar a aplicação, produzindo ou o arquivo .jar ou .war, dependendo do tipo de aplicação. No nosso caso, será gerado o arquivo `combined/target/devopsnapratica.war`
* `mvn clean`: limpa todo código gerado durante o processo de build acima, mantendo apenas os fontes originais.

Esses são alguns dos objetivos disponíveis para uso com o Maven, que pode ser utilizado na linha de comando pela invocação do comando `mvn`, conforme ilustrado abaixo:

```text
$ cd ~/temp/loja-virtual-devops
$ mvn clean compile
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO] 
[INFO] loja-virtual-devops                                                [pom]
[INFO] core                                                               [jar]
[INFO] admin                                                              [war]
[INFO] site                                                               [war]
[INFO] combined                                                           [war]
[INFO] 
[INFO] -------------< br.com.devopsnapratica:lojavirtual-website >-------------
[INFO] Building loja-virtual-devops 1.0                                   [1/5]
[INFO] --------------------------------[ pom ]---------------------------------
...
...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for loja-virtual-devops 1.0:
[INFO] 
[INFO] loja-virtual-devops ................................ SUCCESS [  0.167 s]
[INFO] core ............................................... SUCCESS [  0.879 s]
[INFO] admin .............................................. SUCCESS [  1.733 s]
[INFO] site ............................................... SUCCESS [  1.569 s]
[INFO] combined ........................................... SUCCESS [  0.589 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.095 s
[INFO] Finished at: 2020-10-24T21:18:13-03:00
[INFO] ------------------------------------------------------------------------
```

No exemplo acima, o comando mvn foi executado com dois objetivos distintos, `clean`, para limpar todos os arquivos gerados e `compile`, para construir o código objeto da aplicação.

Um dos aspectos fundamentais para a garantia da qualidade dos produtos de software e ambém para alertar o desenvolvedor sobre possíveis problemas em relação às mudanças realizadas é o teste. Em DevOps é comum que os produtos contenham não apenas o código da aplicação em si, mas também código para o teste unitário, de integração, e outros tipos de testes automatizados, que visam a garantir a qualidade do produto e podem ser também utilizados para garantir o sucesso do build após um determinado _commit_ ser realizado.

No caso do nosso código exemplo, ele não possui muito código de teste associado, mas existem alguns poucos que podem ser compilados e executados com o comando `mvn test-compile test`, conforme ilustrado a seguir.

```text
$ mvn test-compile test
[INFO] Scanning for projects...
...
...
[INFO] --- maven-surefire-plugin:2.10:test (default-test) @ admin ---
[INFO] Surefire report directory: /home/auri/temp/loja-virtual-devops/admin/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running br.com.devopsnapratica.admin.controller.AdminLoginControllerTest
Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.335 sec

Results :

Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
...
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  8.814 s
[INFO] Finished at: 2020-10-24T21:50:06-03:00
[INFO] ------------------------------------------------------------------------
```

No exemplo, foram localizados dois testes na classe `br.com.devopsnapratica.admin.controller.AdminLoginControllerTest`, que tem por objetivo verificar problemas no pacote `admin`. Se algum teste falar, o resultado final não seria o de `BUILD SUCCESS`, mas sim de `BUILD FAILURE`, de modo que isso poderia impedir a entrega do produto caso alguma falha fosse detectada.

Os comandos acima foram possíveis de serem executados pois tenho o Maven instalado na minha máquina local, na qual fiz o clone do projeto disponível em [https://gitlab.com/aurimrv/loja-virtual-devops](https://gitlab.com/aurimrv/loja-virtual-devops). Entretanto, para implementarmos o _pipeline_ DevOps teremos a necessidade de encontrar um contêiner que permita a construção desse projeto na nuvem. 

### Contêiner Docker com Maven e JDK 1.8

Analisando a estrutura de nosso projeto, sabemos que ele demanda uma versão específica do Java para ser compilado, Java 1.8 da Oracle e o Maven. Em minha máquina a tentativa de compilação do projeto com o OpenJDK 1.8 foi frustrada e resultou em `BUILD FAILURE`.

Pesquisando no Hub Docker, localizei um repositório no Hub Docker com imagens ideais para quem trabalha com JDK e Maven: [https://registry.hub.docker.com/\_/microsoft-java-maven](https://registry.hub.docker.com/_/microsoft-java-maven). Nesse repositório existem inúmeras imagens com diferentes versões do Maven e do JDK disponíveis. No nosso caso, optei pela imagem nomeada `mcr.microsoft.com/java/maven:8-zulu-debian9` .

Para testar a imagem basta executar os comandos abaixo:

```text
$ cd ~/temp
$ docker run --rm -it mcr.microsoft.com/java/maven:8-zulu-debian9
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.099 s
[INFO] Finished at: 2020-10-25T02:44:12Z
[INFO] ------------------------------------------------------------------------
[ERROR] No goals have been specified for this build. You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-clean, clean, post-clean, pre-site, site, post-site, site-deploy. -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/NoGoalSpecifiedException
```

Observem que o resultado da criação do contêiner a partir desta imagem foi a execução simples do comando `mvn` sem qualquer objetivo definido. Desse modo, para utilizá-la para o nosso propósito necessitamos realizar duas ações:

1. Compartilhar o código de nossa aplicação com o contêiner
2. Executar o comando `mvn` de forma personalizada, indicando a localização do `pom.xml` de nossa aplicação e os objetivos desejados para a construção da aplicação.

Para realizar as ações acima, basta alterar ligeiramente o comando `docker run` incluindo os seguintes parâmetros, considerando a configuração de minha máquina. No caso de vocês, façam as alterações de diretórios correspondentes.

```text
$ cd ~/temp/devops-extra/cap-05/maven-jdk
$ git clone https://gitlab.com/aurimrv/loja-virtual-devops
$ docker run --rm -it -v /home/auri/temp/devops-extra/cap-05/maven-jdk/loja-virtual-devops:/home/loja-virtual-devops mcr.microsoft.com/java/maven:8-zulu-debian9 mvn -f /home/loja-virtual-devops/pom.xml clean package
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO] 
[INFO] loja-virtual-devops                                                [pom]
[INFO] core                                                               [jar]
[INFO] admin                                                              [war]
[INFO] site                                                               [war]
[INFO] combined                                                           [war]
[INFO] 
[INFO] -------------< br.com.devopsnapratica:lojavirtual-website >-------------
[INFO] Building loja-virtual-devops 1.0                                   [1/5]
[INFO] --------------------------------[ pom ]---------------------------------
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plugin/2.5/maven-clean-plugin-2.5.pom
...
...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for loja-virtual-devops 1.0:
[INFO] 
[INFO] loja-virtual-devops ................................ SUCCESS [  8.113 s]
[INFO] core ............................................... SUCCESS [07:03 min]
[INFO] admin .............................................. SUCCESS [05:09 min]
[INFO] site ............................................... SUCCESS [01:42 min]
[INFO] combined ........................................... SUCCESS [ 23.731 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  14:26 min
[INFO] Finished at: 2020-10-25T03:07:05Z
[INFO] ------------------------------------------------------------------------
```

A primeira alteração foi a inclusão do parâmetro `-v /home/auri/temp/devops-extra/cap-05/maven-jdk/loja-virtual-devops:/home/loja-virtual-devops`, que basicamente compartilha o diretório com a cópia local do repositório da loja virtual com o contêiner. Desse modo, o diretório `/home/loja-virtual-devops` dentro do contêiner está refletido no diretório local `/home/auri/temp/devops-extra/cap-05/maven-jdk/loja-virtual-devops`. 

Em seguida, após o nome da imagem, foi incluída a personalização do comando `mvn` que desejamos executar. Nesse caso, `mvn -f /home/loja-virtual-devops/pom.xml clean package`. Ou seja, peço ao Maven que utilize o arquivo `pom.xml` da aplicação de dentro do contêiner e execute os objetivos `clean package`. Desse modo, se tudo correr bem, será gerado o arquivo no diretório `combined/target/devopsnapratica.war`.

A execução do comando acima levará algum tempo mas, como será observado, permitiu a construção da aplicação exemplo sem utilizar o Maven e o compilador locais. Tudo foi feito dentro do contêiner escolhido. Ao término da execução do comando `mvn -f /home/loja-virtual-devops/pom.xml clean package`, o contêiner encerra sua execução e é possível verificar que o arquivo .war está também presente no diretório local, compartilhado com o contêiner.

```text
$ cd ~/temp/devops-extra/cap-05/maven-jdk
$ ls -l loja-virtual-devops/combined/target/
total 128436
drwxr-xr-x 10 root root      4096 out 25 00:06 devopsnapratica
-rw-r--r--  1 root root 131495230 out 25 00:07 devopsnapratica.war
drwxr-xr-x  2 root root      4096 out 25 00:06 maven-archiver
drwxr-xr-x  2 root root      4096 out 25 00:07 surefire
drwxr-xr-x  3 root root      4096 out 25 00:06 war
```

Importante observar acima que o proprietário e o grupo associados aos arquivos acima são root:root pois foram gerados dentro do contêiner e copiados para a pasta local pelo processo do `docker`. Caso deseje escrever nesse diretório ou realizar alguma alteração qualquer é necessário, primeiro, restaurar ao mesmo a posse para o usuário local que pode ser feito com o comando abaixo:

```text
$ cd ~/temp/devops-extra/cap-05/maven-jdk
$ sudo chown -R $USER:$USER loja-virtual-devops/
[sudo] senha para auri: 

$ ls -l loja-virtual-devops/combined/target/
total 128436
drwxr-xr-x 10 auri auri      4096 out 25 00:06 devopsnapratica
-rw-r--r--  1 auri auri 131495230 out 25 00:07 devopsnapratica.war
drwxr-xr-x  2 auri auri      4096 out 25 00:06 maven-archiver
drwxr-xr-x  2 auri auri      4096 out 25 00:07 surefire
drwxr-xr-x  3 auri auri      4096 out 25 00:06 war
```

Chegando até aqui com sucesso estamos aptos a iniciar a configuração de nosso servidor de intetração contínua.

