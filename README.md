# CST8915 Full-stack Cloud-native Development - Lab 2

- Name: Jane Akinyi Odhiambo
- Student ID: 041268593

## Demo Video

[YouTube Link](https://www.youtube.com/watch?v=hNrIiffn4KA)

## Azure VM Deployment

| Component | VM | Port |
| --- | --- | --- |
| Store Front | store-front | 8080 |
| Order Service | order-service-vm | 3000 |
| Product Service | product-service-vm | 3030 |
| RabbitMQ | rabbitmq-vm | 5672 |

## Service Repositories

- [Order Service](https://github.com/odhi0003/order-service)
- [Product Service](https://github.com/odhi0003/product-service)
- [Store Front](https://github.com/odhi0003/store-front)

## 12-Factor App Implementation

### Factor 1 - Codebase

Each microservice has its own GitHub repository:

- order-service
- product-service
- store-front

### Factor 2 - Dependencies

Dependencies are declared and managed by each service.

Node.js dependencies are declared in package.json. Rust dependencies are declared in Cargo.toml. The Store Front uses local Vue CLI dependencies and npm ci.

### Factor 3 - Configuration

Configuration was moved from hard-coded values into environment variables.

The Order Service uses RABBITMQ\_CONNECTION\_STRING and PORT.

The Product Service uses PORT.

The Store Front uses VUE\_APP\_ORDER\_SERVICE\_URL and VUE\_APP\_PRODUCT\_SERVICE\_URL.


### Factor 4 - Backing Services

RabbitMQ was deployed on its own dedicated Azure VM.

The Order Service connects to RabbitMQ through the RabbitMQ VM's public IP.

Orders are placed into the durable order\_queue.

RabbitMQ was verified using:

sudo rabbitmqctl list\_queues name durable messages

Successful orders increase the message count in the queue.

## Reflection Questions

### 1. What changes did you make to the order-service and product-service to comply with the Configurations and Backing Services factors?

I moved configuration values such as ports and the RabbitMQ connection string from hard-coded values into environment variables. The Order Service now connects to RabbitMQ using a connection string, allowing RabbitMQ to run as an external backing service on its own Azure VM. The Product Service also reads its port from an environment variable.

### 2. Why is it important to use environment variables instead of hard-coding configurations?

Environment variables separate application configuration from application code. This makes the same application easier to deploy in different environments without changing the source code. They also help protect sensitive information such as RabbitMQ credentials because secrets do not need to be committed to Git.

### 3. Why is it important to have separate repositories for each microservice? How does this help maintain independence and scalability?

Separate repositories allow each microservice to have its own codebase, dependencies, tests, and development lifecycle. A service can be updated, tested, and deployed independently without requiring changes to the other services. This supports microservice independence and makes individual services easier to maintain and scale.