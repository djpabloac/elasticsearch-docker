# [Elasticsearch](https://www.elastic.co/es/elasticsearch/ "Elasticsearch")

##### Before

To use the following project first make sure you have installed [Docker](https://docs.docker.com/get-docker/ "Docker").

##### Verify Installation

In a terminal type the following command `docker --version`, then enter. The result should be like the following example `Docker version 20.10.10, build b485636`.

Note: The result will be the same or different depending on the version you have installed.

##### Setting

To not lose the data every time the container is started **[Elasticsearch](https://www.elastic.co/es/elasticsearch/ "Elasticsearch")** it is recommended to configure the file `docker-compose.yml` as follows:

```
version: '2.2'
services:
  es01:
    image: elasticsearch:5.5
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02
      - cluster.initial_master_nodes=es01,es02
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - folder_path/data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic
  es02:
    image: elasticsearch:5.5
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01
      - cluster.initial_master_nodes=es01,es02
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - folder_path/data02:/usr/share/elasticsearch/data
    networks:
      - elastic

volumes:
  data01:
    driver: local
  data02:
    driver: local

networks:
  elastic:
    driver: bridge
```
Note:
- The value of  **"folder_path"** should be the path where the **[Elasticsearch](https://www.elastic.co/es/elasticsearch/ "Elasticsearch")** **"data"** will be stored.
- You can add more elasticsearch containers depending on the processing they require. Adding the following syntax:
	- Create volumen:
```
    volumes:
    	data0X:
			driver: local
```
	- Create services:
```
    es0X:
        image: elasticsearch:5.5
        container_name: es0X
        environment:
          - node.name=es0X
          - cluster.name=es-docker-cluster
          - discovery.seed_hosts=es01,es02
          - cluster.initial_master_nodes=es01,es02,es0X
          - bootstrap.memory_lock=true
          - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
        ulimits:
          memlock:
            soft: -1
            hard: -1
        volumes:
          - folder_path/data0X:/usr/share/elasticsearch/data
        networks:
          - elastic
```

	es0X = name services
	node.name =  Assign name services.
	discovery.seed_hosts = Assign name of the other created services to balance.
	cluster.initial_master_nodes = Assign name of all services created to balance. Modify in the other services created as well.

##### Execution commands
- To run the container:

`docker-compose up --build -d`

- To stop the container: 

`docker-compose down`

## Authors

- [@dj.pablo.ac](https://gitlab.com/dj.pablo.ac)

## Licencia

[MIT](https://choosealicense.com/licenses/mit/)