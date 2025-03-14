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

  - Agora dentro da pasta criaremos a aplicação app.py, que é a aplicação que estará nosso script:

        from flask import Flask

        app = Flask(__name__)

        @app.route("/")
        def home():
        return {"message": "Hello, Docker with Flask!"}

        if __name__ == "__main__":
        app.run(host="0.0.0.0", port=5000)


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

- Esse comando inicia um container MySQL em modo desacoplado (-d), atribui a ele o nome meu-mysql e define a senha do usuário root. Além disso, associa o volume mysql_data ao diretório **/var/lib/mysql**, garantindo a persistência das informações armazenadas no banco de dados. Agora vamos executar o container MySQL associando o volume criado:

      docker run -d \
       --name mysql-container \
       -e MYSQL_ROOT_PASSWORD=rootpassword \
       -e MYSQL_DATABASE=laravel \
       -e MYSQL_USER=laravel_user \
       -e MYSQL_PASSWORD=laravel_password \
       -p 3306:3306 \
       -v mysql_data:/var/lib/mysql \
        mysql:latest


- Por fim, para acessar o MySQL dentro do container, utilizamos o seguinte comando e digite a senha que você gerou no comando:

      docker exec -it meu-mysql mysql -u root -p
  
- Esse comando executa uma instância interativa (-it) do MySQL dentro do container meu-mysql, permitindo o acesso ao banco de dados com o usuário root. Assim, garantimos que o volume foi corretamente criado e está armazenando os dados de forma persistente.

  **Exemplo:**

![Image](https://github.com/user-attachments/assets/a2231766-3be7-41a5-b51c-3f85256bdb3a)

 ## 4.2. Criando e rodando um container multi-stage.
- Primeiro passo para criar a aplicação é criar o arquivo main.go (`nano main.go`), e dentro dele colocar o seguinte contéudo:

      package main

      import (
      "github.com/gofiber/fiber/v2"
      )

      func main() {
        app := fiber.New()

      app.Get("/", func(c *fiber.Ctx) error {
           return c.SendString("Hello, Fiber!")
       })

      app.Listen(":3000")
      }

- Agora iremos criar um Dockerfile (`Dockerfile`), utilizando o multi-stage build. O primeiro estágio irá compilar o código Go, e o segundo estágio irá copiar os artefatos compilados para uma imagem mais enxuta.

      # Etapa 1: Build (usando a imagem do Go para compilar)
      FROM golang:1.20 AS builder

      # Definindo o diretório de trabalho
      WORKDIR /app

      # Copiar os arquivos do projeto para o diretório de trabalho
      COPY . .

      # Baixar as dependências do Go (vai usar go.mod e go.sum)
      RUN go mod tidy

      # Compilar o aplicativo Go
      RUN go build -o app .

      # Etapa 2: Imagem final (imagem menor para rodar a aplicação)
      FROM debian:bullseye-slim

      # Copiar o binário compilado da etapa anterior
      COPY --from=builder /app/app /app

      # Expor a porta em que a aplicação vai rodar
      EXPOSE 3000

      # Comando para rodar a aplicação
      CMD ["/app"]

  - Em seguida crie o arquivo go.mod(`nano main.go`), para gerenciar as dependências do Go, ele criará o go.mod e go.sum automaticamente.
 
        go mod init fiber-api
        go get github.com/gofiber/fiber/v2

  - O próximo passo é construir a imagem do Docker com o comando `docker build -t go-fiber-api .`, esse comando irá passar pela etapa de build, onde o Go será compilado na imagem maior e, em seguida, a aplicação compilada será copiada para uma imagem mais enxuta baseada no Debian Slim. E por fim para executar o container utilizaremos o comando `docker run -p 3000:3000 go-fiber-api`.

 ![Image](https://github.com/user-attachments/assets/1a4a2d62-8732-43ea-9290-e40208982c76)
    
 
    **TESTES:**

- No navegador digite http://localhost:3000, que ele levará a seguinte imagem.

  ![Image](https://github.com/user-attachments/assets/b8e6d5eb-94fc-40a5-9c27-ed51f972126b)



  

  



  

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

    - Agora, crie um docker-compose.yml para subir ambos os containers:
   
          version: '3'
          services:
          mongo:
            image: mongo
            container_name: meu-mongo
          networks:
             minha-rede
          environment:
            MONGO_INITDB_ROOT_USERNAME: admin
            MONGO_INITDB_ROOT_PASSWORD: admin123
          ports:
            "27017:27017"

          node:
           build: .
           container_name: meu-node
          networks:
          - minha-rede
          depends_on:
          - mongo
          ports:
          - "3000:3000"

           networks:
           minha-rede:
           driver: bridge

    - Agora, execute o docker-compose para iniciar os serviços:

          docker-compose up -d

    - Para verificar se o Node.js consegue se conectar ao MongoDB:
   
          docker logs meu-node

    - Para acessar o Node.js:
   
          curl http://localhost:3000

## 4.4. Criando um compose file para rodar uma aplicação com banco de dados
   - Se ainda não tem um projeto Django, crie um com os seguintes comandos:

          mkdir django-polls && cd django-polls
          django-admin startproject pollsapp .

     - Crie um arquivo chamado Dockerfile na raiz do projeto e adicione o seguinte conteúdo:
    
            # Usa a imagem oficial do Python
            FROM python:3.10

            # Define o diretório de trabalho no container
            WORKDIR /app

            # Copia os arquivos para dentro do container
            COPY requirements.txt .
            COPY . .

           # Instala as dependências
           RUN pip install --no-cache-dir -r requirements.txt

           # Expõe a porta 8000
           EXPOSE 8000

           # Comando para rodar a aplicação Django
           CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

  - Crie o arquivo requirements.txt com as dependências do projeto:

        Django
        psycopg2-binary

   - Agora, crie o arquivo docker-compose.yml para configurar os serviços Django e PostgreSQL:

    version: '3.8'

    services:
    db:
    image: postgres:latest
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_DB: django_db
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin123
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

     web:
    build: .
    container_name: django_app
    restart: always
    depends_on:
      - db
    ports:
      - "8000:8000"
    environment:
      - DATABASE_NAME=django_db
      - DATABASE_USER=admin
      - DATABASE_PASSWORD=admin123
      - DATABASE_HOST=db
      - DATABASE_PORT=5432
    volumes:
      - .:/app

    volumes:
     postgres_data:

   - No arquivo pollsapp/settings.py, altere a configuração do banco de dados para usar PostgreSQL:

         DATABASES = {
         'default': {
          'ENGINE': 'django.db.backends.postgresql',
          'NAME': 'django_db',
          'USER': 'admin',
          'PASSWORD': 'admin123',
          'HOST': 'db',
          'PORT': '5432',
          }
         }

- Execute os seguintes comandos para subir a aplicação:

      docker-compose up -d

- DDepois, aplique as migrações do Django:

       docker exec -it django_app python manage.py migrate

- E crie um superusuário para acessar o admin do Django:

      docker exec -it django_app python manage.py createsuperuser


  ## 5. Construção de imagens personalizadas e configurações avançadas.
  ##     5.1. Criando uma imagem personalizada com um servidor web e arquivos estáticos.
    - Crie uma pasta para o projeto e entre nela:

          mkdir nginx-static-site && cd nginx-static-site

    - Dentro da pasta, crie a seguinte estrutura:
 
          nginx-static-site/
          │── site/
          │   ├── index.html
          │   ├── styles.css
          │── Dockerfile
          │── nginx.conf
          │── docker-compose.yml

  - Dentro da pasta site/, crie um arquivo index.html com o seguinte conteúdo:
 
    
        <!DOCTYPE html>
        <html lang="pt">
        <head>
           <meta charset="UTF-8">
           <meta name="viewport" content="width=device-width, initial-scale=1.0">
           <title>Minha Página Estática</title>
           <link rel="stylesheet" href="styles.css">
        </head>
        <body>
           <h1>Bem-vindo ao meu site estático!</h1>
           <p>Este site está rodando dentro de um container Docker com Nginx.</p>
        </body>
        </html>

    - Agora, crie o arquivo styles.css:
   
          body {
          font-family: Arial, sans-serif;
          text-align: center;
          background-color: #f4f4f4;
          color: #333;
           }

      - Crie o arquivo nginx.conf na raiz do projeto:
     
            server {
            listen 80;
            server_name localhost;

            location / {
            root /usr/share/nginx/html;
            index index.html;
            }
            }

    - Crie o arquivo Dockerfile na raiz do projeto:

          # Usa a imagem oficial do Nginx
          FROM nginx:latest

          # Copia o arquivo de configuração do Nginx
          COPY nginx.conf /etc/nginx/conf.d/default.conf

          # Copia os arquivos do site para a pasta padrão do Nginx
          COPY site/ /usr/share/nginx/html/

          # Expõe a porta 80
          EXPOSE 80

          # Inicia o Nginx
          CMD ["nginx", "-g", "daemon off;"]

     - Crie o arquivo docker-compose.yml para facilitar a execução:

           version: '3.8'

           services:
            web:
              build: .
              container_name: nginx_static
              ports:
                - "8080:80"
              volumes:
                - ./site:/usr/share/nginx/html
              restart: always

    - Agora, execute os comandos abaixo para construir a imagem e iniciar o container:
   
          docker-compose up -d --build


      ## 6. Considerações
Trabalhar com **Docker, WSL e Ubuntu** para configurar aplicações completas envolve desafios e boas práticas que garantem a funcionalidade e a comunicação entre os containers.
Trabalhar com Docker Compose e gerenciar múltiplos containers exige atenção a detalhes como redes, volumes, compatibilidade de versões e ordem de inicialização dos serviços. Algumas boas práticas para evitar problemas incluem:  

- Verificar compatibilidade de versões das imagens antes de usá-las.  
- Utilizar volumes para persistência de dados em bancos de dados e arquivos estáticos.  
- Gerenciar a comunicação entre containers corretamente usando nomes de serviço ao invés de `localhost`.  
- Utilizar logs (`docker logs <container>`) para depurar erros e entender o que está acontecendo.  
- Reconstruir imagens sempre que fizer alterações importantes (`docker-compose up -d --build`).  
## 7. Referêmcias
-https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/?utm_source=chatgpt.com - acesso 13/03/2025

-https://github.com/nishanttotla/DockerStaticSite?utm_source=chatgpt.com - acesso 13/03/2025

-https://docs.docker.com/reference/samples/django/ - acesso 14/04/2025


      




  
     

     








    

   


  

   


        
      

      


    











