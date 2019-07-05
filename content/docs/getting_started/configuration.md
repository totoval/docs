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
    
## Environment File Configuration
Here's an example of `.env.json`:
```json
{
  "APP_NAME": "Totoval",
  "APP_ENV": "develop",
  "APP_DEBUG": true,
  "APP_PORT": 8080,
  "APP_LOCALE": "en",
  "APP_KEY": "YOUR-APP-KEY",

  "DB_CONNECTION": "mysql",
  "DB_HOST": "127.0.0.1",
  "DB_PORT": "3306",
  "DB_DATABASE": "YOUR-DATABASE-NAME",
  "DB_USERNAME": "YOUR-DATABASE-USER",
  "DB_PASSWORD": "YOUR-DATABASE-USER-PASSWORD",
  "DB_PREFIX": "prefix_",

  "CACHE_DRIVER": "memory",
  "REDIS_HOST": "127.0.0.1",
  "REDIS_PORT": "6379",
  "REDIS_PASSWORD": "",
  "REDIS_DB": 0,
  "REDIS_CACHE_DB": 1,

  "AUTH_SIGN_KEY": "YOUR-AUTH-SIGN-KEY",

  "QUEUE_CONNECTION": "nsq",
  "QUEUE_NSQD_TCP_HOST": "127.0.0.1",
  "QUEUE_NSQD_TCP_PORT": "4150",
  "QUEUE_NSQLOOKUPD_HTTP_HOST": "http://127.0.0.1",
  "QUEUE_NSQLOOKUPD_HTTP_PORT": "4161",
  "QUEUE_MAX_IN_FLIGHT": "50"
}
```

## Environment Configuration
Set the environment variable named as the key of `.env.json` defined will overwrite the data in `.env.json`.

## Configuration Priority
`Environment Variables` **>** `Environment Files`

## Accessing Configuration Values
**Totoval use `dot` to separate the level of config key for getting the data.**  
  
Here's an example of getting the boolean value of a key named `debug` in `config/app.go`.
```go
import "github.com/totoval/framework/config"
...
...
config.GetBool("app.debug")
...
...
```