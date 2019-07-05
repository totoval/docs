# Configuration

## Minimal Configuration
1. Use default env file

    ```sh
    mv .env.example.json .env.json
    ```
    
2. Startup your **mysql** instance if you don't have  
Here's a docker example:

    ```sh
    docker run --name mysql --rm -it \ 
    -p \"3306:3306\" \ 
    -v \"${HOME}/{YOUR-MYSQL-PERSISTENCE-DATA-PATH}/mysql:/var/lib/mysql\" \ 
    -e \"MYSQL_ROOT_PASSWORD={YOUR-MYSQL-ROOT-PASSWORD}\" \ 
    mysql:latest
    ```
    
3. Let's start your truck!

    ```sh 
    go run main.go
    ```
    
## Environment Configuration

## Environment File Configuration

## Accessing Configuration Values