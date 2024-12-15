## 9th

- My docker setup for Scylla
   ```bash
   docker run --name test_scylla -d -p 122:22 -p 17000-17001:7000-7001 -p 19042:9042 -p 19160:9160 -p 19180:9180 -p 10000:10000 --volume "/home/gurustron/DataGripProjects/Main/scylla.yaml":/etc/scylla/scylla.yaml scylladb/scylla
   ```
- Docker setup from course
  ```
  docker run --name Node_X -d scylladb/scylla:5.2.0 --overprovisioned 1 --smp 1 --reactor-backend=epoll
  docker run --name Node_Y -d scylladb/scylla:5.2.0 --seeds="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' Node_X)" --overprovisioned 1 --smp 1 --reactor-backend=epoll
  docker run --name Node_Z -d scylladb/scylla:5.2.0 --seeds="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' Node_X)" --overprovisioned 1 --smp 1 --reactor-backend=epoll
  ```

## 10th

- Lab
  ```sql
  CREATE KEYSPACE mykeyspace WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'replication_factor' : 3};
  use mykeyspace;
  CREATE TABLE users ( user_id int, fname text, lname text, PRIMARY KEY((user_id)));
  insert into users(user_id, fname, lname) values (1, 'rick', 'sanchez'); 
  insert into users(user_id, fname, lname) values (4, 'rust', 'cohle');
  CONSISTENCY QUORUM;
  insert into users (user_id, fname, lname) values (7, 'eric', 'cartman');
   
  CONSISTENCY ALL;
  insert into users (user_id, fname, lname) values (8, 'lorne', 'malvo');
   
  -- docker stop Node_Y
   
  CONSISTENCY QUORUM ;
  insert into users (user_id, fname, lname) values (9, 'avon', 'barksdale');
   
  CONSISTENCY ALL ;
  insert into users (user_id, fname, lname) values (10, 'vm', 'varga');
  -- [2024-12-10 23:10:35] com.datastax.oss.driver.api.core.servererrors.UnavailableException:
  -- Not enough replicas available for query at consistency ALL (3 required but only 2 alive)
   
  -- docker stop Node_Z
   
  CONSISTENCY QUORUM;
  insert into users (user_id, fname, lname) values (11, 'morty', 'smith');
  -- [2024-12-10 23:11:49] com.datastax.oss.driver.api.core.AllNodesFailedException:
  -- All 1 node(s) tried for the query failed (showing first 1 nodes, use getAllErrors() for more):
  -- Node(endPoint=172.17.0.3/<unresolved>:9042, hostId=593391ef-4413-40e8-a455-860702629709, hashCode=e8f6b27): [
  -- com.datastax.oss.driver.api.core.servererrors.UnavailableException:
  -- Not enough replicas available for query at consistency QUORUM (2 required but only 1 alive)]
   
  CONSISTENCY ONE;
  insert into users (user_id, fname, lname) values (11, 'morty', 'smith');
  ```
  ![image](https://github.com/user-attachments/assets/7a6a5d5f-5900-425a-951e-be4f0bb91b9f)

  ## 14th

  - Magic command
     ```
     ./usr/lib/scylla/seastar-cpu-map.sh -n scylla
     ```
  


