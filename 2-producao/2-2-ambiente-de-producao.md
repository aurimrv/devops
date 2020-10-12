# 2.2 Ambiente de Produção

Nessa seção, será apresentada uma forma de, utilizando contêineres Docker, nós instanciarmos cada um dos servidores necessários para o funcionamento da aplicação. Há inúmeras maneiras de fazermos isso com a tecnologia Docker. Vamos iniciar com uma mais simples e mais fácil de colocar em execução. Posteriormente, nos capítulos mais adiante, utilizaremos recursos mais avançados para o gerenciamento e instanciação dos diversos contêineres que compõem não apenas o ambiente de produção, mas todos os demais ambientes necessários para colocar o DevOps em prática.

Nesse ponto, assume-se que o Docker já está instalado e que você conseguiu executar com sucesso o Hello-World do Docker, apresentado no Capítulo 1, Seção 1.3.

O próximo passo aqui criarmos as imagens para os nossos Servidores Web e BD utilizando Docker. O primeiro passo para isso é pesquisar no [Docker Hub](https://hub.docker.com/) pelas imagens já disponíveis e ver se tem alguma que nos atenda. Nesse repositório há imagens de diferentes tipos e pode ser que uma delas seja adequada para a nossa realidade.

Uma forma de consultar por imagens de nosso interesse é utilizando o comando abaixo:

```text
docker search tomcat
```

Como resultado desse comando, o Docker nos mostra todas as imagens presentes no Docker Hub que contém o termo tomcat. Um exemplo de parte da saída resultante é mostrado abaixo:

```text
auri@amrv:~$ docker search tomcat
NAME                          DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
tomcat                        Apache Tomcat is an open source implementati…   2827                [OK]                
tomee                         Apache TomEE is an all-Apache Java EE certif…   83                  [OK]                
dordoka/tomcat                Ubuntu 14.04, Oracle JDK 8 and Tomcat 8 base…   55                                      [OK]
bitnami/tomcat                Bitnami Tomcat Docker Image                     36                                      [OK]
kubeguide/tomcat-app          Tomcat image for Chapter 1                      29                                      
...

```

Observe que o primeiro delas é uma máquina oficial do Apache Tomcat e, daremos preferência a ela. O mesmo pode ser feito para a consulta de uma imagem com o MySQL.

```text
auri@amrv:~$ docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   9965                [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3645                [OK]                
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   725                                     [OK]
percona                           Percona Server is a fork of the MySQL relati…   509                 [OK]                
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   83                                      
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   75                                      
centurylink/mysql                 Image containing mysql. Optimized to be link…   61                                      [OK]
...
```

Observem que há também uma imagem pronta oficial com o MySQL já instalado. Faremos uso dessa também para criar o ambiente de produção de nossa aplicação.

Além do comando docker search, é possível também consutlar imagens diretamente no Docker Hub. Por exemplo, nesse link [https://hub.docker.com/\_/tomcat](https://hub.docker.com/_/tomcat), você tem acesso à documentação da imagem do Tomcat e nesse [https://hub.docker.com/\_/mysql](https://hub.docker.com/_/mysql) do MySQL. Recomenda-se a leitura do documentação para compreender as diferentes formas de uso dos contêineres.

Na documentação de ambos os contêineres vemos que há várias versões dos mesmos disponíveis e, nesse caso, podemos escolher aquela que é mais adequada para a execução de nossa aplicação e mostraremos isso nas seções a seguir.

