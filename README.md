# Container apps
container-apps-stacks examples of different webapps that run as a docker containers or docker compose stacks

### PostgreSQL PG Admin with a volume to contain DB settings
#### Docs for [container app](https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html)
```bash
docker run  \
            --name pg_admin \
            -p 85:80 \
            -e "PGADMIN_DEFAULT_EMAIL=admin" \
            -e "PGADMIN_DEFAULT_PASSWORD=secret" \
            -v pgadmin-data \
            -d dpage/pgadmin4
```

#### PostgreSQL PG Admin
Run with port 85 in host network
``` bash
docker run  \
            --name pg_admin_app \
            -e "PGADMIN_DEFAULT_EMAIL=admin@domain.com" \
            -e "PGADMIN_DEFAULT_PASSWORD=MY-Secret!" \
            -e "PGADMIN_LISTEN_PORT=85" \
            -v pgadmin-data --network="host" \
            -d dpage/pgadmin4
```
### MYSQL php Admin 
Access MySQLDBs

```bash
DOMAIN=mysql-multi.500
docker run                         \
          --name mySQL-Admin       \
          -p 8080:80               \
          -e PMA_HOST="$DOMAIN" \
          -e PMA_PORT=3306      \
          --link testsql:db
          -d phpmyadmin/phpmyadmin
```

### Run Prometheus with bind volume
```bash
docker run  \
            --name prometheus_bindvol \
            --detach -p 127.0.0.1:9090:9090 \
            -v /tmp/prometheus.yml:/etc/prometheus.yml \
            prom/prometheus \
            --config.file=./data/prometheus.yml
```

### Run Prometheus Creating aditional volume
```bash
docker run  \
            --name prometheus_vol \
            --detach -p 127.0.0.1:9090:9090 \
            -v /prometheus-data \
            prom/prometheus:latest --config.file=/prometheus-data/prometheus.yml
```
#### Bind-mount your prometheus.yml from the host by running
```bash
docker run -d --net=host \
            -v /$HOME/prometheus.yml:/etc/prometheus/prometheus.yml \
            --name prometheus-server \
            prom/prometheus
```

### node exporter for local system at [NodeExporter](https://github.com/prometheus/)node_exporter
```bash
docker run -d \
            --name node_exporter \
            --net="host" \
            --pid="host" \
            -v "/:/host:ro,rslave" \
            quay.io/prometheus/node-exporter \
            --path.rootfs /host
```

###  Another way to run the Prometheus node_exporter
```bash
docker run -d \
    -v "/proc:/host/proc" \
    -v "/sys:/host/sys" \
    -v "/:/rootfs" \
    --net="host" \
    --name=prometheus \
    quay.io/prometheus/node-exporter:v0.13.0 \
        -collector.procfs /host/proc \
        -collector.sysfs /host/sys \
        -collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)"
```

###  Use pushGateway for short jobs
```bash
docker pull prom/pushgateway

docker run -d -p 9091:9091 --name push_gateway prom/pushgateway
```

###  Rancher Labs Kubernetes management tool
```bash
docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:latest
```

###  Runnning a local docker registry
```bash
docker run -d -p 5000:5000 --restart=always --name container_registry registry:latest
```

###  Run Atlasian FishEye
```bash
docker run -d -p 8080:8080 --name fish_eye mswinarski/atlassian-fisheye
```

###  kubernetes analyzer
```bash
docker run -d \
        --net="host" \
        --name 'kube-opex-analytics' \
        -v /home/ws/kube-opex-analytics:/data \
        -e KOA_DB_LOCATION=/data/db \
        -e KOA_K8S_API_ENDPOINT=http://127.0.0.1:8001 \
        rchakode/kube-opex-analytics
```

###  Spinnaker Docs runing in Ruby container
```bash
docker run -d --name 'spinnaker-oss-docs' \
       -p 4000:4000 \
       --volume /home/ws/deployments/Spinnaker/oss-contribute-docs/code:/code \
       aleon1220/spinnaker-dev-docs
```

### Jekyll containers
##### Build Jekyll site
```bash
docker run --rm -it --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" --env JEKYLL_ENV=production jekyll/jekyll:3.8 jekyll build
```

### Serve Jekyll site
```bash
docker run --rm --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" --env JEKYLL_ENV=development -p 4000:4000 jekyll/jekyll:3.8 jekyll serve
```
