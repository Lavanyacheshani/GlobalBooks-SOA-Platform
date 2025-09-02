# GlobalBooks SOA Platform

This project demonstrates a **Service-Oriented Architecture (SOA)** implementation for a book ordering system, combining SOAP and REST services with enterprise messaging.

## System Overview

The GlobalBooks platform provides an integrated solution for managing book catalogs, orders, payments, and shipping.

### Core Services

1. **Catalog Service (SOAP)** – Port 8080  
   - Built with Java Spring Boot and JAX-WS  
   - Uses SOAP protocol, supporting WS-Security  
   - Handles book catalog operations, pricing, and availability checks  

2. **Orders Service (REST)** – Port 8081  
   - Java Spring Boot REST API  
   - Secured with OAuth2  
   - Manages order creation, tracking, and orchestration  

3. **Payments Service (REST)** – Port 8082  
   - Java Spring Boot microservice  
   - Handles payment initiation and processing  

4. **Shipping Service (REST)** – Port 8083  
   - Java Spring Boot microservice  
   - Manages shipment creation, processing, and tracking  

### Integration Components

- **RabbitMQ** – Asynchronous messaging between services  
- **BPEL Orchestration** – Coordinates order fulfillment workflows  
- **Security Layers** – WS-Security for SOAP, OAuth2 for REST  

## Prerequisites

- Java 11 or higher  
- Maven 3.6+  
- Docker and Docker Compose  

## Getting Started

1. **Clone the repository**  

```bash
git clone <repository-url>
cd SOA
````

2. **Start all services using Docker**

```bash
cd 10-deployment/docker
docker-compose up -d --build
```

3. **Check if services are running**

* Catalog Service (SOAP): `http://localhost:8080/soap/catalog?wsdl`
* Orders Service: `http://localhost:8081/api/v1/orders/health`
* Payments Service: `http://localhost:8082/api/v1/payments/health`
* Shipping Service: `http://localhost:8083/api/v1/shippings/health`
* RabbitMQ Admin: `http://localhost:15672` (admin/password123)

---

## API Endpoints

### Orders Service (REST)

```
POST   /api/v1/orders          # Create order
GET    /api/v1/orders          # Retrieve all orders
GET    /api/v1/orders/{id}     # Get order by ID
PUT    /api/v1/orders/{id}/status  # Update order status
DELETE /api/v1/orders/{id}     # Cancel order
GET    /api/v1/orders/health   # Service health check
```

### Payments Service (REST)

```
POST   /api/v1/payments/initiate
POST   /api/v1/payments/{id}/process
GET    /api/v1/payments/{id}
GET    /api/v1/payments/order/{orderId}
GET    /api/v1/payments/health
```

### Shipping Service (REST)

```
POST   /api/v1/shippings
POST   /api/v1/shippings/{id}/process
GET    /api/v1/shippings/{id}
GET    /api/v1/shippings/tracking/{num}
GET    /api/v1/shippings/health
```

### Catalog Service (SOAP)

* WSDL: `http://localhost:8080/soap/catalog?wsdl`
* Operations:

  * `searchBooks`
  * `getBookById`
  * `getBookPrice`
  * `checkAvailability`

---

## Workflow

1. Orders are created through the Orders REST API
2. Events are sent to RabbitMQ
3. Payments Service consumes payment events
4. Shipping Service handles shipment events
5. BPEL orchestrates the entire order workflow

---

## Security Overview

**Current Implementation:**

* SOAP: Basic authentication
* REST: OAuth2 with JWT
* Identity management via Keycloak

**Future Enhancements:**

* WS-Security UsernameToken for SOAP
* Full OAuth2 support for REST
* Mutual TLS between services

---

## Testing

* **Postman Collections**: Located in `09-testing/postman/`
* **SOAP UI Test Suite**: `09-testing/soap-ui/CatalogService-TestSuite.xml`

---

## Development

### Build Services Individually

```bash
# Catalog
cd 02-catalog-service
mvn clean package

# Orders
cd 03-orders-service
mvn clean package

# Payments
cd 04-payments-service
mvn clean package

# Shipping
cd 05-shipping-service
mvn clean package
```

### Run Services Locally

```bash
java -jar target/*.jar
```

---

## Monitoring

* Health check endpoints on all services
* RabbitMQ web interface for message monitoring
* Spring Boot Actuator metrics

---

## Deployment Options

### Docker

```bash
cd 10-deployment/docker
docker-compose up -d --build
```

### Kubernetes

* Manifests available in `10-deployment/kubernetes/`

---

## Architecture Patterns

* Service-Oriented Architecture (SOA)
* Event-Driven Architecture
* Microservices
* Enterprise Service Bus (ESB)
* API Gateway
* Circuit Breaker
* Saga pattern via BPEL

---

## Technology Stack

* **Java 11**
* **Spring Boot & Spring Web Services**
* **RabbitMQ**
* **Docker & Kubernetes**
* **OAuth2 & WS-Security**
* **BPEL orchestration**
* **H2 in-memory database**

---

## Project Structure

```
├── 01-design-artifacts/
├── 02-catalog-service/
├── 03-orders-service/
├── 04-payments-service/
├── 05-shipping-service/
├── 06-bpel-orchestration/
├── 07-integration/
├── 08-security/
├── 09-testing/
├── 10-deployment/
└── 11-documentation/
```

---

## Contributing

1. Fork the repository
2. Create a new branch
3. Make changes and add tests
4. Submit a pull request

---

## License

This project uses the MIT License – see LICENSE file.

```

---

If you want, I can **also add a “Step-by-step guide to test using Postman and SOAP UI”** section at the bottom so your README looks fully detailed and professional.  

Do you want me to add that?
```
