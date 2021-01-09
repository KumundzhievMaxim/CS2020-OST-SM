# Kafka
Streaming Fraud Detection System based on Kafka and Python interface.

# Description
We will generate a stream of transactions and process those to detect which ones are potential fraud.
- Docker images taken from Confluent Platform — Confluent being a major actor in the Apache Kafka community — and was inspired by their Kafka single node example.


# Notions
```
Apache Kafka is a piece of software which, as all pieces of software, runs on actual computers.
First, a bit of terminology. Kafka being a distributed system, it runs in a cluster, i.e. multiple computers (a.k.a. nodes) that communicate with one another. In Kafka jargon, nodes are called brokers.
```

```
What does Kafka need in order to run? Actually, it only needs two things — a Kafka broker and a Zookeeper instance.
Zookeeper - it is a coordination software, distributed as well, used by Apache Kafka to keep track of the cluster state and members. It is an essential component of any Kafka cluster,
 ```

# Deployment
Before launching the kafka microservice, navigate to `docker.env` and setup desired parameters:
```
Example:
    DATASET=CICIDS2017
    LEVEL=top
    MODEL=svm
    DATABASE=mongodb

Explanation:
    DATASET=<NAME OF DATASET TO PROCESS> # TEST SET NOT TRAIN
    LEVEL=<LEVEL OF DATASET TO PROCESS> # FINE, MID, TOP -- EACH DATASET HAS IT"S OWN PARTICULAR TYPES 
    MODEL=<TYPE OF PREDICTING MODEL> # [SVM, DECISION TREE, RANDOM FOREST] 
    DATABASE=<DATABASE TO WRITE RESULTS> # [MONGODB, CASSANDRADB]
``` 
 

```bash
# mandatory [Terminal 1]
spin up the kafka && zookeeper clusters 
$ docker-compose -f docker-compose.kafka.yml up

# mandatory [Terminal 2]
spin up the generator && detector   
$ docker-compose --env-file docker.env up

# addition [Terminal 3]
to know when kafka cluster finished initialising 
$ docker-compose -f docker-compose.kafka.yml logs broker # specific case

# addition [Terminal 3]
to observe filtered by detector legit transactions (--topic streaming.transactions.legit)   
$ docker-compose -f docker-compose.kafka.yml exec broker kafka-console-consumer --bootstrap-server localhost:9092 --topic streaming.transactions.legit

# addition [Terminal 3]
to observe filtered by detector fraud transactions (--topic streaming.transactions.fraud)
$ docker-compose -f docker-compose.kafka.yml exec broker kafka-console-consumer --bootstrap-server localhost:9092 --topic streaming.transactions.fraud
```
