# Build SonarQube with Glowroot APM

    docker build -t "sonarqube-glowroot:7.7-community" .

# Build and Composer

    docker-compose up -d

# Stop Composer

    docker-compose stop

# Investigatations

    docker ps -qf "name=glowroot-central-server"
    docker ps -qf "name=glowroot-central-db"
    docker ps -qf "name=sonarqube-server"
    docker ps -qf "name=sonarqube-db"
    docker exec -it $(docker ps -qf "name=glowroot-central-server") /bin/bash
    docker network ls
    docker-compose logs
