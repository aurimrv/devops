# 6.3 Personalizando Imagem do Servidor Web via GitLab CI/CD

Confome dito anteriormente, vamos a seguir, gerar a imagem do nosso Servidor Web com o código de nossa aplicação empacotado dentro dele, simulando como se estivessemos fazendo o deploy da aplicação num servidor web remoto.

No nosso caso, como estamos executando tudo via localhost, o que faremos é atualizar a imagem do Servidor Tomcat no Hub Docker de modo que, em seguida, podemos reiniciar as instâncias locais dos nossos servidores localmente e as alterações passem a ter efeito.

Inicialmente, a primeira coisa a ser feita é entender que, para que seja possível a personalização da imagem do Tomcat de dentro do CI/CD, os arquivos de configuração do nosso servidor Web foram adicionados no nosso projeto da loja virtual dentro do GitLab. Para isso, criou-se uma pasta denominada `docker-img` e, dentro dela, uma subpasta `web`. Assim sendo, dentro desse diretório `./docker-img/web` foram adicionados o conteúdo da pasta `devops-extra/cap-06/web`, conforme ilustrado abaixo.

```text
$ tree -a devops-extra/cap-06/web/
devops-extra/cap-06/web/
├── catalina.sh
├── context.xml
├── Dockerfile
├── .gitignore
├── .gitkeep
├── .keystore
├── LEIAME.txt
└── server.xml
```

Internamento no GitLab os arquivos estão localizados na estrutura de diretórios conforme descrito acima.

![Diret&#xF3;rio contento o arquivo Dockerfile do Servidor Web](../.gitbook/assets/gitlab-01%20%281%29.png)

Observe que esses arquivos precisam agora estar no servidor pois, quem irá construir a imagem do servidor Web e enviá-la para o Hub Docker não seremos nós, como fizemos com as imagens do Servidor de Bando de Dados e do Servidor de Monitoramento, mas sim nosso script do GitLab CI/CD.

### Alterando o Script CI/CD

O primeiro passo é incluir um estágio a mais no nosso pipeline chamado de deploy. Abaixo apresentamos o arquivo do pipeline completo e em seguida as explicações sobre as alterações que foram realizadas para viabilizar o deploy da aplicação dentro da imagem do servidor Web.

```text
image: mcr.microsoft.com/java/maven:8-zulu-debian9

variables:
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

cache:
  paths:
    - .m2/repository/
    - combined/target/
    - docker-img/web/ 

services:
  - docker:dind

stages:
  - build
  - test
  - package
  - deploy

maven-build:
  stage: build
  script: 
    - mvn clean compile test-compile

maven-test:
  stage: test
  script: 
    - mvn test
  artifacts:
    reports:
      junit:
        - core/target/surefire-reports/TEST-*.xml
        - core/target/failsafe-reports/TEST-*.xml
        - admin/target/surefire-reports/TEST-*.xml
        - admin/target/failsafe-reports/TEST-*.xml
        - site/target/surefire-reports/TEST-*.xml
        - site/target/failsafe-reports/TEST-*.xml
        - combined/target/surefire-reports/TEST-*.xml
        - combined/target/failsafe-reports/TEST-*.xml

maven-package:
  stage: package
  script: 
    - mvn package
    - cp combined/target/devopsnapratica.war docker-img/web/devopsnapratica.war
  artifacts:
    paths:
      - combined/target/devopsnapratica.war 
  only:
    - master
    
docker-deploy:
  stage: deploy
  image: docker:latest
  script:
  - docker build -t $DOCKER_ID/tomcat-server-img ./docker-img/web
  - echo $DOCKER_TOKEN | docker login --username $DOCKER_ID --password-stdin
  - docker push $DOCKER_ID/tomcat-server-img
  only:
    - master
```

Como dissemos, a alteração principal foi a incluão de um novo `stage`, `deploy`, na linha 19 e o mesmo está associado ao job que se inicia na linha 53 \(`docker-deploy`\).

Esse job faz uso de uma outra imagem, denominada `docker:latest`\(linha 55\), que contém os comandos docker que serão executados na cláusula `script` \(linhas 57 a 59\). Isso é necessário poia na nossa imagem original, que usamos para a execução do Maven, não existe o Docker instalado e, portanto, sem a inclusão dessa imagem específica para a execução desse job, os comandos não seriam reconhecidos e geraria-se uma falha nesse job. Se quiser testar e ver o que ocorre, basta remover essa linha 54 do script e avaliar o resultado após todo o processamento do pipelene.

Em seguida, os comandos presentes nas linhas 57 a 59 são exatamente os mesmos que utilizamos para a construção das imagens e envio ao Hub Docker descritos na Seção 6.2. Entretanto, aqui estão parametrizados, considerando as variáveis já devinidas anteriormente na Seções 5.4 e 5.5, com o `DOCKER_ID` e o `DOCKER_TOKEN` do usuário. Desse modo, não são expostas publicamente o ID e o Token de Acesso Pessoal do usuário.

Outra novidade nesse arquivo de configuração é a presença, nas linhas 50 e 60, da cláusula `only`. O GltLab CI/CD oferece diferentes formas de restingir a execução de job considerando determinadas condições. Nesse exemplo acima, `only master` indique que, os jobs `maven-package` e `docker-deploy` só serão executados caso a alteração seja realizada no ramos master do repositório. Do contrário, esses jobs não serão executados.

Finalmente, para permitir que haja uma troca de arquivos entre os diferentes jobs e também para acelerar a execução dos mesmos está sendo utilizado o recurso de cache. No exemplo acima, estamos fazendo cache dos arquivos de dependência do Maven, armazenados localmente em `.m2/repository/` e também dos diretórios `combined/target/` e `docker-img/web/`. Esses dois últimos são responsáveis pelo armazenamento do arquivo lovavirtualdevops.war e dos arquivo de configuração do Servidor Web, respectivamente. 

Observa-se também, na linha 3 o uso da cláusula `variables` que, nesse escopo, permite a definição de uma variável global, `MAVEN_OPTS`, utilizada para indicar a localização do repositório local de arquivos de dependência do Maven.

Outra novidade presente nesse script é o uso da cláusula `artifacts` em dois jobs. No primeiro,  `maven-test,` ela é usada para indicar que, como resultado desse job, podem ser gerados arquivos de relatório do JUnit, nos locais indicados. No segundo, `maven-package`, ela é usada para indicar o arquivo representando o pacote de distribuição da aplicação: `devopsnapratica.war`.

