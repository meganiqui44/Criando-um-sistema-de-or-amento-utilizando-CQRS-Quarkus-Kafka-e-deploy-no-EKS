About CQRS - Command Query Responsibility Segregation

According with Martin Folwer

    At its heart is the notion that you can use a different model to update information than the model you use to read information. For some situations, this separation can be valuable, but beware that for most systems CQRS adds risky complexity.

The application

Simulates a bank account scenario where an end user adds a income or expense transaction, and it is processed in a ascyncronous event sourcing and CQRS architecture to recalculate the user's bank account balance. The user can also request the balance of it's account. Down here you can see the design:

Design
Deploying the external services

docker-compose up -d --build

It will deploy four docker containers on your environment with MongoDB, PostgreSQL, Kafka and Zookepper (required by Kafka)

After deploying Kafka, you'll need to create the topic on the Kafka cluster. For example:

docker exec -it bankaccount-kafka \
  ./bin/kafka-topics.sh --create \
  --topic transactions \
  --zookeeper bankaccount-zookeeper:2181 \
  --replication-factor 1 \
  --partitions 1

Testing the application
Running a CURL request to create a income transaction

curl -X POST -H "Content-Type: application/json" -d @income-transaction.json http://localhost:8080/transactions
