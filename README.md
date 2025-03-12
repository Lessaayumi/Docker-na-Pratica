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

## 3.1. Rodando um container básico
- Antes de iniciarmos a criação do nosso container, é essencial verificar se o Docker está devidamente instalado em nossa máquina com o sistema operacional Ubuntu. No terminal, devemos digitar o seguinte comando:

          docker --version
  
  - Se o Docker estiver devidamente instalado, o comando retornará as seguintes informações contidas na imagem:

    











