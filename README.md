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

      - Agora, vamos preparar um caminho para hospedarmos o site do Tailminds em nosso servidor. Com o comando abaixo vamos criar esse caminho.

             mkdir meu-site && cd meu-site

      - O próximo passo é baixar o script dinâmico do Tailmindcss, para isso, dentro da pasta meu-site vamos digitar o seguinte comando. E em seguida ja executar a aplicação.

             curl -o tailwind.js https://cdn.tailwindcss.com
             docker run --name meu-nginx-static -d -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx

      - Teremos a seguinte visualição quando acessarmos http://localhost:8080.

          ![Image](https://github.com/user-attachments/assets/7c04a32f-f0fc-4285-9289-595bfb736aa7)
        

     ## 3.2. Criando e rodando um container interativo.

      - Primeiro temos que rodar um contaneir no Ubuntu que seja interativo, para isso, utilizamos o comando:

            docker run -it --name meu-ubuntu ubuntu bash
        
      - Quando executar esse comando você entrará dentro dele, antes de inociarmos a interação com ele vamos rodar o comando abaixo para atualizar o sistema:

            apt update && apt install -y curl

      - Caso queria sair do container sem parar a sua execução é só utilizar o comando `exit` e para retornar usa-se o comando `docker start -ai meu-ubuntu`
   
      - Agora, vamos criar um script `.sh` para que esse container possa interagir. Para isso utilizamos o comando:
   
            nano logs.sh
        
      -  Dentro desse arquivo vamos fazer um programas simples para gerir logs fos sistemas, data e hora, uso de discos e memória.
   
             #!/bin/bash

             echo "=== Logs do Sistema ==="
             echo "Data e Hora: $(date)"
             echo "Uso de Disco:"
             df -h
             echo "Uso de Memória:"
             free -m

     - Após salver e sair, vamos começar o processo de automatizar esse script, no terminal terá que digitar o seguinte comando.

             chmod +x logs.sh

    - Agora para visualizar ele é só digitamos:
   
           ./logs.sh

    -  Ele nós dara as seguintes informações, conforme a imagem. 

        ![Image](https://github.com/user-attachments/assets/2e2671f2-baa6-461d-871c-4f161db81436)


  ## 3.3. Listando e removendo containers

  - Para listar os contaneirs que estão em execução, utilizamos o comando:

        docker ps

  - Agora para listar todos os contaneirs que existem, utiliza-se o comando
 
         docker ps -a

    **Exemplo**

    ![Image](https://github.com/user-attachments/assets/b3e12d97-9953-4723-ba49-3c93be20f9c2)

 - Para interromper a execução de um container utilizamos o comando:

       docker stop nome_do_container

- Para excluir utiliza-se o comando:

       docker rm nome_do_container

- Um comando que facilita caso todos containers parados não seja mais útils é `docker container prune`, ele irá remover todos os containers que não estão mais em execução.

## 3.4. Criando um Dockerfile para uma aplicação simples em Python.
   - Para iniciar a criação do *Dockerfile* utilizando uma aplicação *Flask*, primeiramente, é necessário criar um diretório que armazenará o conteúdo a ser exibido. Para isso iremos utilizar comando:

         mkdir flask-app && cd flask-app

  - Dentro da pasta flask-app iremos criar o arquivo com o comando `nano nome_do_arquivo`, dentro desse arquivo iremos agregar o seguinte script:

        echo "from flask import Flask
        app = Flask(__name__)
        @app.route('/')
        def home():
           return 'Olá, Docker!'
        if __name__ == '__main__':
           app.run(host='0.0.0.0', port=5000)" > app.py

    - Agora iremos criar um Dockerfile e nele colocaremos o seguinte conteúdo:

          echo "FROM python:3.9
          WORKDIR /app
          COPY app.py .
          RUN pip install flask
          CMD ['python', 'app.py']" > Dockerfile

    - Depois desses passos, temos que construir a imagem que será transmitida, para isso utilizaremos o comando `docker build -t flask-app` e por fim terá que rodar o contaneir com o comando `docker run -d -p 5000:5000 flask-app`.
   
    - Em http://localhost:5000, você irá ver a imagem que foi construida.
   
      **Exemplo**

        ![Image](https://github.com/user-attachments/assets/47365a47-2667-463f-8a2e-c5467c4cc236)


## 4. Persistência de dados, redes Docker e otimização de imagens.
   ## 4.1. Criando e utilizando volumes para persistência de dados.

- O passo inicial consiste na criação do volume por meio do comando:

       docker volume create mysql_data

  ![Image](https://github.com/user-attachments/assets/150fd3fa-dd5b-4588-a473-5e6a6d4bdb85)

- Esse procedimento garante a persistência dos dados do MySQL, permitindo sua reutilização independentemente do ciclo de vida dos containers.O próximo passo consiste na execução do container MySQL com persistência de dados. Para isso, utilizamos o seguinte comando:

      docker run -d --name meu-mysql -e MYSQL_ROOT_PASSWORD=root -v mysql_data:/var/lib/mysql mysql:latest

- Esse comando inicia um container MySQL em modo desacoplado (-d), atribui a ele o nome meu-mysql e define a senha do usuário root. Além disso, associa o volume mysql_data ao diretório **/var/lib/mysql**, garantindo a persistência das informações armazenadas no banco de dados.

- Por fim, para acessar o MySQL dentro do container, utilizamos o seguinte comando:

      docker exec -it meu-mysql mysql -u root -p
  
- Esse comando executa uma instância interativa (-it) do MySQL dentro do container meu-mysql, permitindo o acesso ao banco de dados com o usuário root. Assim, garantimos que o volume foi corretamente criado e está armazenando os dados de forma persistente.

  **Exemplo:**

![Image](https://github.com/user-attachments/assets/a2231766-3be7-41a5-b51c-3f85256bdb3a)

 ## 4.2. Criando e rodando um container multi-stage.

 - O primeiro comando que vamos utilizar é, para clonar o repósitorio do Go Fiber.

        git clone https://github.com/gofiber/recipes.git
        cd recipes/dockerls

- Em seguida deve-se criar um DockerFile utilizando o comando `nano`

      nano Dockerfile

- Dentro desse dockerfile iremos por as seguintes informações:

      FROM golang:1.23 AS builder #Usa a imagem oficial do Go (1.23) para ter todas as ferramentas necessárias.
      WORKDIR /app #Define o diretório /app dentro do container.
      COPY . . #Copia o código-fonte para o container.
      RUN go mod tidy && go build -o server . #Baixa as dependências e compila o código Go em um binário chamado server.

      FROM alpine:latest #Usa a imagem mínima Alpine Linux
      WORKDIR /root/ #Define um diretório /root/ para rodar a aplicação.
      COPY --from=builder /app/server . #Copia apenas o binário compilado da primeira fase
      EXPOSE 8080 #Expõe a porta 8080

      CMD ["./server"] #Define o comando que inicia o servidor

  - Execute o seguinte comando dentro da pasta onde está o Dockerfile, ele irá baixar a imagem do Go e compila o projeto.

        docker build -t go-fiber-app .

    - Para rodar o container:
   
          docker run -p 8080:8080 go-fiber-app
      **PRECISO REVER ESSE EXERCICIO**

   ##  4.3. Construindo uma rede Docker para comunicação entre containers.
  - Antes de iniciar os containers, crie uma rede personalizada para permitir a comunicação entre eles:

        docker network create minha-rede

  - Agora, suba um container MongoDB e conecte-o à rede criada:
 
        docker run -d \
        --name meu-mongo \
        --network minha-rede \
        -e MONGO_INITDB_ROOT_USERNAME=admin \
        -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
        -p 27017:27017 \
        mongo

    - Isso iniciará um container MongoDB com um usuário root. Clone o repositório MEAN Todos (ou crie um projeto básico Node.js):

          git clone https://github.com/meanjs/mean.git mean-todos
          cd mean-todos

    - Dentro da pasta do projeto, crie um arquivo Dockerfile:
   
           FROM node:16

          WORKDIR /app

          COPY package*.json ./

          RUN npm install
  
          COPY . .

          EXPOSE 3000

          CMD ["node", "server.js"]




    

   


  

   


        
      

      


    











