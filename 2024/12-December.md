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


