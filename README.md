# E-Commerce Microservice Saga & Outbox

This repository contains a Spring Boot microservice system that demonstrates the **Saga** and **Outbox** patterns for a simple e-commerce domain. The system uses **PostgreSQL** for data storage, **Kafka** for asynchronous messaging, and **Eureka** for service discovery, with an **API Gateway** for routing.

## Architecture Overview

**Core components**

- **Eureka Server**: Service discovery for all microservices.
- **API Gateway**: Single entry point that routes requests to services.
- **Kafka + Zookeeper**: Event streaming backbone for the Saga/Outbox flow.
- **PostgreSQL**: Shared database server with separate databases per service.

**Business services**

- **User Service** (`user-service`)
- **Product Service** (`product-service`)
- **Order Service** (`order-service`)
- **Payment Service** (`payment-service`)
- **Delivery Service** (`delivery-service`)
- **Saga Service** (`saga-service`)

## Prerequisites

- **Docker** + **Docker Compose** (recommended way to run the system)

## Quick Start (Docker Compose)

From the repository root:

```bash
docker compose up --build
```

The first build can take a few minutes while Gradle downloads dependencies and builds each service.

### Stop and clean up

```bash
docker compose down -v
```

## Service Ports

| Service | Port |
| --- | --- |
| API Gateway | `8080` |
| Eureka Server | `8761` |
| Product Service | `8090` |
| Order Service | `8082` |
| Payment Service | `8095` |
| Delivery Service | `8060` |
| User Service | `8087` |
| Saga Service | `8020` |
| PostgreSQL | `5432` |
| Kafka | `9092` |

## API Gateway Routes

All traffic should go through the API Gateway:

| Route Prefix | Service |
| --- | --- |
| `/product/**` | Product Service |
| `/order/**` | Order Service |
| `/payment/**` | Payment Service |
| `/delivery/**` | Delivery Service |
| `/user/**` | User Service |
| `/saga/**` | Saga Service |

Example:

```
http://localhost:8080/product/api/product
```

## Environment Variables

You can override defaults by creating a `.env` file or exporting variables before running Docker Compose:

| Variable | Default | Description |
| --- | --- | --- |
| `POSTGRES_USER` | `ecommerce` | Database username |
| `POSTGRES_PASSWORD` | `ecommerce` | Database password |
| `MY_SECRET_KEY` | `change-me` | JWT signing secret |
| `MY_EXPIRATION_TIME` | `3600000` | JWT expiration time (ms) |

## Database Initialization

The PostgreSQL container runs the init script in `docker/postgres/init/01-create-dbs.sql` to create:

`deliverydb`, `orderdb`, `paymentdb`, `productdb`, `sagadb`, `userdb`

## Local Development (Optional)

Each service is a standalone Spring Boot project. You can run one locally if you provide PostgreSQL, Kafka, and Eureka:

```bash
cd orderservice
./gradlew bootRun
```

Make sure the infrastructure in `docker-compose.yml` is running if you choose local runs.
