# Build

    docker build -t "sonarqube-glowroot:7.7-community" .

# Investigate

    docker exec -it sonarqube-glowroot:7.7-community /bin/bash
    docker network ls
