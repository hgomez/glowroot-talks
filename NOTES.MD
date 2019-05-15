SQ 6.7.3 token : c74abe72303cfebb39b86018d30cdbbf3c8d2909

mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:19000 \
  -Dsonar.login=c74abe72303cfebb39b86018d30cdbbf3c8d2909


SQ 6.7.4 token : 4ed5741c1042ac98db64685c188c3bac4f0f75d8

mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:29000 \
  -Dsonar.login=4ed5741c1042ac98db64685c188c3bac4f0f75d8


SQ 7.2 token : 30e37da3d89c9bb64cf0010e6441949ce086d7e1

mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:39000 \
  -Dsonar.login=30e37da3d89c9bb64cf0010e6441949ce086d7e1

SQ 7.4 token : 161f3c9c8c8f41e0d6e55351dcd4e6f2d33c02c1

mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:49000 \
  -Dsonar.login=161f3c9c8c8f41e0d6e55351dcd4e6f2d33c02c1

SQ 7.7 token : 1bbfcd6240c4fff59d33d21fe9b9a35a426e0058

mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=1bbfcd6240c4fff59d33d21fe9b9a35a426e0058

SQ 7.5 

[
  {
    "className": "org.sonar.ce.cleaning.CeCleaningSchedulerImpl",
    "methodName": "cleanCeQueue",
    "methodParameterTypes": [],
    "captureKind": "transaction",
    "transactionType": "Housekeeping",
    "transactionNameTemplate": "CeCleaningSchedulerImpl",
    "traceEntryMessageTemplate": "{{0}}",
    "timerName": "CeCleaningSchedulerImplTimer"
  },
  {
    "className": "org.sonar.ce.taskprocessor.CeWorkerImpl",
    "methodName": "executeTask",
    "methodParameterTypes": [
      "org.sonar.ce.queue.CeTask"
    ],
    "captureKind": "transaction",
    "transactionType": "Background",
    "transactionNameTemplate": "CEExecuteTask",
    "traceEntryMessageTemplate": "{{0}}",
    "timerName": "CEExecuteTaskTimer"
  }
]


SQ 7.7 

[
  {
    "className": "org.sonar.ce.taskprocessor.CeWorkerImpl$ExecuteTask",
    "methodName": "executeTask",
    "methodParameterTypes": [
      ".."
    ],
    "captureKind": "transaction",
    "transactionType": "Background",
    "transactionNameTemplate": "CEExecuteTask",
    "alreadyInTransactionBehavior": "capture-trace-entry",
    "traceEntryMessageTemplate": "{{0}}",
    "traceEntryStackThresholdMillis": 10,
    "timerName": "CEExecuteTaskTimer"
  }
]


# Terminal #1

MYGLOWROOT_BASE_DIR=$HOME/myglowroot
mkdir -p ${MYGLOWROOT_BASE_DIR}
cd ${MYGLOWROOT_BASE_DIR}

# Download Cassandra
CASSANDRA_VERSION=3.11.2
curl -L -s https://archive.apache.org/dist/cassandra/${CASSANDRA_VERSION}/apache-cassandra-${CASSANDRA_VERSION}-bin.tar.gz -O
tar xvzf apache-cassandra-${CASSANDRA_VERSION}-bin.tar.gz
rm -f apache-cassandra-${CASSANDRA_VERSION}-bin.tar.gz
mv apache-cassandra-${CASSANDRA_VERSION} cassandra

# Download Glowroot Central
GLOWROOT_VERSION=0.10.12
curl -L -s https://github.com/glowroot/glowroot/releases/download/v${GLOWROOT_VERSION}/glowroot-central-${GLOWROOT_VERSION}-dist.zip -O
unzip glowroot-central-${GLOWROOT_VERSION}-dist.zip
rm -f glowroot-central-${GLOWROOT_VERSION}-dist.zip

# Prepare Glowroot Agent
curl -L -s https://github.com/glowroot/glowroot/releases/download/v${GLOWROOT_VERSION}/glowroot-${GLOWROOT_VERSION}-dist.zip -O
unzip glowroot-${GLOWROOT_VERSION}-dist.zip
rm -f glowroot-${GLOWROOT_VERSION}-dist.zip

# check you have 3 dirs: 
# cassandra  glowroot  glowroot-central
ls 

# Prepare Conf for scanner
mkdir glowroot-4-scanner
cd glowroot-4-scanner
# Edit config.json by using config.json attached and change instrumentations
# Adjust slowThresholdMillis 2000ms could be to high for you
vi config.json


# Terminal #2
cd ${MYGLOWROOT_BASE_DIR}/cassandra
./bin/cassandra -f

# Terminal #3
cd ${MYGLOWROOT_BASE_DIR}/glowroot-central
java -Dui.port=40000 -jar glowroot-central.jar

# Terminal #4

# Add this to scanner JVM
# replace ${MYGLOWROOT_BASE_DIR} by appropriate contents :)
#
# -javaagent:${MYGLOWROOT_BASE_DIR}/glowroot/glowroot.jar -Dglowroot.agent.id=glowroot-4-scanner -Dglowroot.agent.rollup.id=michael-scanner \
# -Dglowroot.conf.dir=${MYGLOWROOT_BASE_DIR}/glowroot -Dglowroot.log.dir=${MYGLOWROOT_BASE_DIR}/glowroot/logs -Dglowroot.tmp.dir=${MYGLOWROOT_BASE_DIR}/glowroot/tmp \
# -Dglowroot.collector.address=127.0.0.1:8181
 


