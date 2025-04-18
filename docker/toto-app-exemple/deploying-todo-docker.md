# this guid show how to deploy todo-app with docker

![alt text](image.png)

1. Clone the repo [todo-starter](https://github.com/Avshalom-Ts/todo-starter)

    Architectures used in the project

    - NodeJs version: 10.12.0
    - NPM version: 6.4.1

2. Create the network in docker

    ```bash
    docker network create todo-net
    ```

    Make shure the networks was created

    ```bash
    docker network ls
    ```

3. Deploy new postgresql container

    ```bash
    docker run --net todo-net --name todo-postgres -p 5432:5432 -e POSTGRES_USER=todo -e POSTGRES_PASSWORD=todo1234 -e POSTGRES_DB=todo -d postgres:11.2
    ```

    Check if container is running

    ```bash
    docker container ls
    ```

4. Deploy new redis container

    ```bash
    docker run --net todo-net --name todo-redis -p 6379:6379 -d redis:5.0.3
    ```

    Check if container is running

    ```bash
    docker container ls
    ```

5. Deploy new elastic container

    ```bash
    docker run --net todo-net --name todo-elastic -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.6.1
    ```

    Check if container is running

    ```bash
    docker container ls
    ```

    **Check the network config of todo-net to see all atached containers**

    ```bash
    docker network inspect todo-net
    ```

6. Running the api with npm

    ```bash
    cd todo-starter/todo-api/
    npm install
    npm start
    ```

    **Should print**

    ```text
    > todo-api@1.0.0 start
    > node index.js

    Todo API Server started!
    Redis client connected
    Postgres client connected
    Elasticsearch client connected
    Created a new Elastic index: todos { acknowledged: true, shards_acknowledged: true, index: 'todos' }
    ```

    Test the api from secont console

    ```bash
    curl http://localhost:3000/api/v1/todos
    ```

    Or in the web browser `http://<server-address>:3000/api/v1/todos`

    Put data in redis to get resolt in previes command

    ```bash
    curl --data "title=Get the kids from school" http://localhost:3000/api/v1/todos
    ```

7. Running the frontend with npm

