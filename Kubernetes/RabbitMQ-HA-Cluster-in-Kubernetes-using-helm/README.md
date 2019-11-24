# Use helm to install RabbitMQ HA Cluster

`
helm install --name dev-rabbitmq-ha -f .\rabbitmq-config.yml stable/rabbitmq-ha --namespace mynamespace `
