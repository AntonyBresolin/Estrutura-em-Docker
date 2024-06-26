# Estrutura em Docker

# Tutorial de Aplicação: Docker Swarm, Grafana, MySQL, WordPress & Prometheus

## 1. Preparação Inicial

1. Assegure-se de que o Docker, Git e WSL (caso utilize Windows) estão instalados.
2. Verifique se as portas necessárias estão livres.

## 2. Criação do Diretório

1. Crie um diretório e acesse-o:
    ```bash
    mkdir seu-diretorio 
    cd seu-diretorio
    ```
2. Clone o repositório:
    ```bash
    git clone https://github.com/AntonyBresolin/Estrutura-em-Docker.git
    cd Trabalho-Docker
    ```

## 3. Execução dos Comandos

1. Inicialize o Docker Swarm:
    ```bash
    docker swarm init
    ```
2. Crie e atualize um stack (conjunto de serviços) a partir de um arquivo Compose no Swarm:
    ```bash
    docker stack deploy -c docker-compose.yml wordpress_stack
    ```
3. Verifique os nós do Docker Swarm:
    ```bash
    docker node ls
    ```
4. Verifique os contêineres em execução:
    ```bash
    docker ps
    ```
5. Acesse o contêiner do WordPress:
    ```bash
    docker exec -it ID_Do_Container_Wordpress bash
    ```
6. No contêiner do WordPress, atualize os pacotes e instale o nano:
    ```bash
    apt update
    apt install nano
    ```
7. Edite o arquivo `wp-config.php`:
    ```bash
    nano wp-config.php
    ```
    Adicione as seguintes linhas:
    ```php
    if ($configExtra = getenv_docker('WORDPRESS_CONFIG_EXTRA', '')) {
        eval($configExtra);
    }

    /* Adicione as linhas abaixo */
    define('WP_CACHE', true);
    define('WP_REDIS_HOST', 'redis');
    define('WP_REDIS_PORT', 6379);
    
    ```
    Salve e saia do editor (CTRL+X, Y e ENTER).

## 4. Acessar o WordPress

1. No navegador, acesse `localhost:8080/wp-admin`. Você verá a página de instalação e configuração do WordPress.
2. Acesse o painel de administração em `localhost:8080/wp-admin`.
    - Vá em "Tools", "Site health" e "info", e procure por "database" para ver as configurações do banco de dados.
3. Vá em "Plugins" -> "Add New Plugin", procure e instale o plugin "Redis".
    - Ative o plugin.

## 5. Acessar o Prometheus

1. Acesse `http://localhost:9090`.
2. Na aba "Expressions", digite "up" e clique em "Execute".
3. Observe as instâncias que estão em execução.

## 6. Acessar o Grafana

1. Acesse `http://localhost:3000`.
2. Faça login com as credenciais "Admin" e "Admin".
3. Vá para "Data sources":
    1. Clique em "Add new data source".
    2. Selecione "Prometheus".
    3. Na seção de conexão, insira o endereço `http://localhost:9090`.
    4. Clique em "Save" e teste a conexão.
4. Vá para "Dashboards", clique em "New" e selecione "Import".
    - Importe um código, como por exemplo '3662', e clique em "Import".

## 7. Verificação Final

1. Verifique se as aplicações estão em execução com sucesso acessando `http://localhost:9090/Targets`.
2. O site wordpress fica disponível em `http://localhost:8080/`



## Fim
