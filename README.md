# Сборка проекта

## Сборка контейнеров Docker

### MongoDB

Используемый образ `mongodb` из Docker Hub:

```
docker image pull mongodb/mongodb-community-server:8.0.1-ubuntu2204
```

### Docker-Compose со сборкой образов

#### SpringBoot Application Properties

##### Сервис Parser

*/src/main/resources/application.properties*:

```
spring.application.name=parser
server.port=8082
spring.data.mongodb.uri=mongodb://mongodb:27017/wb-products
processor.service.url=http://wb-processor:8082/process
```

При изменении `server.port` поменять порт в Dockerfile.

##### Сервис Processor

*/src/main/resources/application.properties*:

```
spring.application.name=processor
server.port=8082
spring.data.mongodb.uri=mongodb://mongodb:27017/wb-products
```

При изменении `server.port` поменять порт в Dockerfile и `application.properties` у сервиса Parser.

#### Сборка JAR исполнямых файлов

Собираем jar исходники с помощью `./mvnw`.

Пример. В папке сервиса `parser`:

```
./mvnw package
```

Итого: исполняемый файл в папке *target/*.

Также с остальными сервисами.

#### Сборка docker-compose

В папке с файлом docker-compose.yml:

```
docker-compose build
```

Генерируются Docker образы (images) для `parser` и `processor` (для `mongodb` взяли образ из Docker Hub).

Запуск контейнеров:

```
docker-compose up -d
```

***

### Docker-Compose без сборки образов

Образы на Docker Hub:

```
docker image pull walkername/wb-parser:latest
docker image pull walkername/wb-processor:latest
```

Запуск контейнеров. В папке с файлом docker-compose.yml:

```
docker-compose up -d
```

***

### API

API для парсинга:
```
http://wb-parser:8080/parse
```

Отправка JSON'a в виде:

```json
{
    "url": "[url]"
}
```

[url] - ссылка из заголовка запроса товаров (не с адресной строки).
