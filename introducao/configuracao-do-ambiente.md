# 1.3 Configuração do Ambiente

Um dos grandes desafios no uso de tecnologias voltadas para o desenvolvimento de aplicações diz respeito à configurtação de ambientes para o desenvolvimento e execução do produto de software produzido. A cada novo colaborador que chega na organização, a preparação do ambiente de trabalho pode consumir um tempo significativo. Conforme visto na seção anterior, o uso de máquinas virtuais simplificou bastante esse processo mas, as máquinas virtuais ainda sofrem do problema da exigência de um SO completo em cada uma delas para prover os serviçõs necessários. A evolução das mesmas deu origem ao conceito de contêineres os quais possuem capacidades semelhantes as das máquinas virtuais com a vantagem de serem mais "leves" e demandarem menos recursos de hardware para sua execução.

A tecnologia de contêineres surgiu para facilitar essa atividade e simplificar não apenas a configuração do ambiente de desenvolvimento, como também o ambiente de execução da aplicação. Utiliza-se muito hoje o termo _dockerização de aplicação_ para fazer referência a uma aplicação que não era executada em contêineres e passa a ser após a _dockerização_.

O primeiro passa para dar início a esse processo é instalar as ferramentas do Docker que iremos fazer uso. Neste capítulo veremos como instalar e configurar o ambiente do Docker + Docker Compose que será utilizado nos capítulos a seguir. Apesar de apresentarmos a instalação do Docker no ambiente Windows, é altamente recomendável que você faça uso da plataforma no ambiente Linux por este ser mais estável e alinhado com a filosofia do Docker.

## Windows

* Faça o download do instalador do Docker Desktop [aqui](https://hub.docker.com/editions/community/docker-ce-desktop-windows/);
* Abra o instalador e siga as instruções **\(Importante: deixe a opção "Enable Hyper-V Windows Features" ativa!\)**;
* Quando o processo de instalação terminar, reinicie o computador;
* Ao reiniciar, abra o Docker Desktop para poder iniciar o daemon do docker;
* Tanto o Docker quanto o Docker Compose estarão instalados e ativos na sua máquina, e podem ser testados abrindo uma janela do Powershell e inserindo comandos como `docker run hello-world`.

  > Obs: No Windows, o Docker Desktop também vem com um pequeno tutorial para começar a utilizar a ferramenta, pode ser útil.

![Instala&#xE7;&#xE3;o no Windows](https://imgur.com/CWNhb64.jpg)

## Linux

* Ubuntu:
  * No terminal, instale os pacotes `docker` e `docker-compose` com `sudo apt install docker.io docker-compose`;
* Arch:
  * No terminal, instale os pacotes `docker` e `docker-compose` com `sudo pacman -S docker docker-compose`;
* Rode o comando `sudo usermod -aG docker $USER` para adicionar o usuário atual ao grupo do docker e então poder rodar o comando sem ser root;
* Feche o terminal e abra de novo \(talvez seja necessário reiniciar o computador para finalizar a instalação e ativar o daemon do docker\);
* Rode o comando `docker run hello-world` para confirmar que a instalação aconteceu corretamente.

![Instala&#xE7;&#xE3;o no Linux](https://i.imgur.com/OQR8ByS.jpg)

