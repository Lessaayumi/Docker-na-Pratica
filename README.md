# Docker na Prática
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
      FROM ubuntu:22.04

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
    
 
   TESTES:

- No navegador digite http://localhost:3000, que ele levará a seguinte imagem.

  ![Image](https://github.com/user-attachments/assets/b8e6d5eb-94fc-40a5-9c27-ed51f972126b)


   ##  4.3. Construindo uma rede Docker para comunicação entre containers. **DIFICULDADE AQUI!**
  - Primeiro vamos criar um arquivo Dockerfile (`Dockerfile`), para o container Node.js que será responsável pela aplicação backend (API).

        # Usar imagem oficial do Node.js
        FROM node:16

        # Definir o diretório de trabalho dentro do container
        WORKDIR /app

        # Copiar o package.json e o package-lock.json para o diretório de trabalho
        COPY package*.json ./

        # Instalar as dependências da aplicação
        RUN npm install

        # Copiar o restante dos arquivos da aplicação
        COPY . .

        # Expor a porta onde o app vai rodar
        EXPOSE 3000

        # Rodar a aplicação quando o container iniciar
        CMD ["npm", "start"]

    - O próximo passo é criar é criar um docker-compose.yml ( `nano docker-compose.yml`), ele irá definir a rede personalizada, o container do Node.js e o container do MongoDB.
   
          version: '3'

          services:
            nodejs:
            build
             context: .
                dockerfile: Dockerfile
          ports:
             - "3000:3000"
          networks:
            - todos-network
          environment:
            - MONGO_URI=mongodb://mongo:27017/todos
          depends_on:
           - mongo
          restart: always

          mongo:
           image: mongo:latest
           ports:
            - "27017:27017"
          networks:
           - todos-network
          restart: always
 
          networks:
          todos-network:
          driver: bridge

    - Agora, crie um arquivo app.js (`nano app.js`) para a API do Node.js que irá se conectar ao MongoDB e criar endpoints para as tarefas.
   
          const express = require('express');
          const mongoose = require('mongoose');
          const cors = require('cors');

          // Inicializando o Express
          const app = express();

          // Conexão com o MongoDB
          mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
          .then(() => console.log('Conectado ao MongoDB'))
          .catch(err => console.log('Erro de conexão com o MongoDB', err));

          // Middleware
          app.use(cors());
          app.use(express.json());

          // Definindo o modelo de Tarefa (todos)
          const Task = mongoose.model('Task', new mongoose.Schema({
          task: String,
          completed: { type: Boolean, default: false }
          }));

          // Rotas
          app.get('/tasks', async (req, res) => {
          const tasks = await Task.find();
          res.json(tasks);
          });

          app.post('/tasks', async (req, res) => {
          const task = new Task({ task: req.body.task });
          await task.save();
          res.status(201).json(task);
          });

          app.put('/tasks/:id', async (req, res) => {
          const task = await Task.findByIdAndUpdate(req.params.id, req.body, { new: true });
          res.json(task);
          });

          app.delete('/tasks/:id', async (req, res) => {
          await Task.findByIdAndDelete(req.params.id);
          res.status(204).end();
          });

          // Iniciar o servidor
          app.listen(3000, () => {
          console.log('Server rodando na porta 3000');
          });

    - Agora crie o arquivo package.json (`nano packge.json`) para a aplicação Node.js com as dependências necessárias.
   
           {
             "name": "mean-todos",
             "version": "1.0.0",
             "description": "App de tarefas com Node.js e MongoDB",
             "main": "app.js",
             "scripts": {
             "start": "node app.js"
          },
            "dependencies": {
            "cors": "^2.8.5",
            "express": "^4.17.1",
            "mongoose": "^5.9.25"
          },
           "author": "",
           "license": "ISC"
          }

      - Agora, no diretório onde estão o docker-compose.yml, Dockerfile, app.js, e package.json, execute os seguintes comandos para construir e rodar os containers:
     
            docker-compose up --build

        ![Image](https://github.com/user-attachments/assets/47e9492e-f82d-435e-9764-9856f2bcc70f)

        
## 4.4. Criando um compose file para rodar uma aplicação com banco de dados
   - Primeiro, crie um diretório para o seu projeto, caso ainda não tenha um. Em seguida, entre na pasta e inicialize o projeto Django:

         mkdir django-polls-app
         cd django-polls-app
         python3 -m venv venv
         source venv/bin/activate
         pip install django
         django-admin startproject polls_project .

- Crie o app "polls": Dentro do projeto Django, crie o app chamado polls:
    
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
              DATABASE_URL: postgres://postgres:postgres@db:5432/pollsdb
             
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

- Se acessarmos localhost:8000, veremos que a aplicação está rodando.

  ![Image](https://github.com/user-attachments/assets/6bac85ef-b9c1-4ca5-ba27-4209db6c549e)


  ## 5. Construção de imagens personalizadas e configurações avançadas.
  ##     5.1. Criando uma imagem personalizada com um servidor web e arquivos estáticos.
    - Crie uma pasta para o projeto e entre nela:
      
          mkdir nginx-static-site
          cd nginx-static-site
          mkdir site
   
         
- Dentro da pasta, crie a seguinte estrutura:
 
          nginx-static-site/
          │── site/
          │   ├── index.html
          │   ├── styles.css
          │── Dockerfile
          │── nginx.conf
          │── docker-compose.yml

  - Dentro da pasta site/, crie um arquivo index.html com o seguinte conteúdo:
 
        <a href="sobre.html">Sobre Nós</a>


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
   
          # Remove a configuração padrão do Nginx
          RUN rm -rf /usr/share/nginx/html/*

          # Copia o arquivo de configuração do Nginx
          COPY nginx.conf /etc/nginx/conf.d/default.conf

          # Copia os arquivos do site para a pasta padrão do Nginx
          COPY ./site /usr/share/nginx/html/

          # Expõe a porta 80
          EXPOSE 80

          # Inicia o Nginx
          CMD ["nginx", "-g", "daemon off;"]

     - Crie o arquivo em html `nano nome.html`:

           <!DOCTYPE html>
            <html lang="pt">
            <head>
               <meta charset="UTF-8">
               <meta name="viewport" content="width=device-width, initial-scale=1.0">
               <title>Sobre Nós</title>
               <link rel="stylesheet" href="style.css">
           </head>
           <body>
               <h1>Sobre Nós</h1>
               <p>Esta é a página sobre o nosso projeto!</p>
               <a href="index.html">Voltar para Home</a>
                </body>
           </html>

   - Agora temos que construir a imaagem utilizando o comando:

         docker build -t meu-site-nginx .


   - Agora, execute os comandos abaixo para  iniciar o container:

         docker run -d -p 8080:80 --name site-container meu-site-nginx

 - Acesse http://localhost:8080/sobre.html que ele executará a pagina html criada:

   ![Image](https://github.com/user-attachments/assets/269e3c1f-c6f3-4cd2-9c84-ffe540bff6b8)

   ![Image](https://github.com/user-attachments/assets/e075e734-a3d6-4a8f-8a4f-2a775f08b957)   

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


      




  
     

     








    

   


  

   


        
      

      


    











