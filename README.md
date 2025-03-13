# Docker-na-Pratica
Repositório com exercícios práticos de Docker, desde containers básicos até configurações avançadas com Docker Compose e multi-stage builds.

## Índice

1. [Resumo e tecnologias](#Resumo)  

2. [Objetivo](#objetivo)  
   
3. [Rodando containers simples, interagindo com terminais e gerenciando containers.](#Rodando-containers-simples,-interagindo-com-terminais-e-gerenciando-containers.)  
   3.1. [Rodando um container básico](#Rodando-um-container-básico)
   3.2. [Criando e rodando um container interativo.](#Criando-e-rodando-um-container-interativo.)
   3.3. [Listando e removendo containers.](#Listando-e-removendo-containers.)
   3.4. [Criando um Dockerfile para uma aplicação simples em Python.](#Criando-um-Dockerfile-para-uma-aplicação-simples-em-Python.)
   
4. [Persistência de dados, redes Docker e otimização de imagens.](#Persistência-de-dados,-redes-Docker-e-otimização-de-imagens.)  
   4.1. [Criando e utilizando volumes para persistência de dados.](#Criando-e-utilizando-volumes-para-persistência-de-dados.)
    
   4.2. [Criando e rodando um container multi-stage.](#Criando-e-rodando-um-container-multi-stage.)

   4.3. [Construindo uma rede Docker para comunicação entre containers.](#Construindo-uma-rede-Docker-para-comunicação-entre-containers.)

   4.4. [Criando um compose file para rodar uma aplicação com banco de dados](#Criando-um-compose-file-para-rodar-uma-aplicação-com-banco-de-dados)

5. [Construção de imagens personalizadas e configurações avançadas.](#Construção-de-imagens-personalizadas-e-configurações-avançadas.)  
   5.1. [Criando uma imagem personalizada com um servidor web e arquivos estáticos.](#Criando-uma-imagem-personalizada-com-um-servidor-web-e-arquivos-estáticos.)

6. [Considerações](#Considerações)

7.  [Referências](#Referências)

# 1. Resumo e tecnologias:
Este projeto reúne uma série de exercícios práticos para aprender Docker, desde a execução de containers básicos até configurações avançadas com Docker Compose, volumes e redes. Cada exercício inclui um exemplo real, como rodar um servidor web, gerenciar bancos de dados e otimizar aplicações.

Tecnologias:
- Docker;
- Nginx/Apache;
- Ubuntu;
- Python (Flask);
- MySQL/PostgreSQL;
- Node.js + MongoDB;
- Go;
- Laravel/Django.

# 2. Objetivo:
O objetivo desses exercícios é ensinar, na prática, como usar Docker para criar, gerenciar e otimizar containers, passando por diferentes níveis de complexidade. Eles ajudam a entender desde o básico, como rodar e interagir com containers, até conceitos mais avançados, como persistência de dados, redes Docker e multi-stage builds. Além disso, cada exercício traz exemplos aplicáveis, permitindo que você veja o Docker sendo usado em cenários reais, como a execução de aplicações web, bancos de dados e APIs.

## 3. Rodando containers simples, interagindo com terminais e gerenciando containers.

- Antes de iniciarmos a criação do nosso container, é essencial verificar se o Docker está devidamente instalado em nossa máquina com o sistema operacional Ubuntu. No terminal, devemos digitar o seguinte comando:

          docker --version
  
  - Se o Docker estiver devidamente instalado, o comando retornará as seguintes informações contidas na imagem:
 
    ![Image](https://github.com/user-attachments/assets/2666d188-ad34-440c-bf7f-ab9590ba06f1)

  - Caso o Docker não esteja instalado, devemos executar os seguintes comandos para proceder com a instalação

        sudo apt update
        sudo apt install docker.io -y

  - Após a instalação, é recomendável verificar se o serviço do Docker está ativo com:

        sudo systemctl status docker
 
  - Para verificar se o Docker está funcionando corretamente, podemos executar a imagem de teste "hello-world" com o seguinte comando:

        docker run hello-world

  - Se tudo estiver configurado corretamente, o Docker fará o download da imagem e exibirá uma mensagem confirmando que o container foi executado com sucesso, assim como a imagem abaixo.

    ![Image](https://github.com/user-attachments/assets/aed0862f-9095-44f2-a3c8-e01802850b22)


    ## 3.1. Rodando um container básico
    - **Objetivo**: Executar um container usando a imagem do Nginx e acesse a página padrão no navegador.
    - Exemplo de aplicação: Use a landing page do TailwindCSS como site estático dentro do container.

    - Para executar um container utilizando a imagem do Nginx, basta rodar o seguinte comando:

          docker run -d -p 8080:80 --name meu-nginx nginx
      
    - Para verificar se o container está funcionando corretamente, basta abrir o navegador e acessar o link **http://localhost:8080**. A imagem abaixo ilustra como a página será exibida no navegador.
 
      ![Image](https://github.com/user-attachments/assets/d6cb0ebc-f320-4c6d-b429-ea715f30b134)
      
    - **Explicações** O comando `docker run -d -p 8080:80 --name meu-nginx nginx` inicia um container em segundo plano (`-d`), utilizando a imagem do **Nginx**. A opção `-p 8080:80` define que a porta **80** do container será acessível pela porta **8080** da máquina host, permitindo que o Nginx seja acessado pelo navegador. O parâmetro `--name meu-nginx` atribui um nome personalizado ao container, facilitando sua identificação.
      
      

      


    











