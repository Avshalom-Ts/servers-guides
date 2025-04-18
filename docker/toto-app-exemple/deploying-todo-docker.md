# this guid show how to deploy todo-app with docker

![alt text](image.png)

1. Clone the repo [todo-starter](https://github.com/Avshalom-Ts/todo-starter)

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
    docker run --net todo-net --name todo-elastic -d -p 9200:9200 -p 9300:9300 -e "discovery.type=singlenode" elasticsearch:6.6.1
    ```

    Check if container is running

    ```bash
    docker container ls
    ```
