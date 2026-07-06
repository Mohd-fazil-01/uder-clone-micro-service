# 🚗 Uber Clone — Microservices Architecture

A scalable backend clone of Uber, built using **Node.js** and a **Microservices Architecture**. The system is split into independent services (**User**, **Captain**, and **Ride**) that communicate asynchronously through **RabbitMQ**, with a central **API Gateway** routing all incoming requests.

This project demonstrates real-world distributed system design patterns — service decoupling, inter-service messaging, centralized routing, and secure authentication — commonly used in production-grade ride-hailing platforms.

---

## 📌 Features

- 🧩 **Microservices Architecture** — independent User, Captain, and Ride services for better scalability and fault isolation
- 🌐 **API Gateway** — single entry point that routes requests to the correct microservice using `express-http-proxy`
- 📨 **Asynchronous Messaging** — RabbitMQ-based communication between services to decouple workflows
- 🔐 **Secure Authentication** — JWT + bcrypt based auth with cookie-based session handling
- 🗄️ **Independent Databases** — each service manages its own MongoDB collections via Mongoose
- 🚕 **Ride Management** — ride creation, ride requests, and ride handling flow between users and captains
- 🧑‍✈️ **Captain Management** — captain registration, profile, and availability handling
- 👤 **User Management** — user registration, login, and profile management

---

## 🏗️ Architecture Overview

```
                        ┌─────────────────┐
                        │   API Gateway    │
                        │ (Express + HTTP  │
                        │      Proxy)      │
                        └────────┬─────────┘
                                 │
        ┌────────────────┬──────┴──────┬────────────────┐
        ▼                ▼             ▼
 ┌─────────────┐  ┌──────────────┐ ┌─────────────┐
 │ User Service │  │Captain Service│ │ Ride Service │
 │ (Node/Express│  │(Node/Express) │ │(Node/Express)│
 │  + MongoDB)  │  │  + MongoDB)   │ │  + MongoDB)  │
 └──────┬───────┘  └───────┬──────┘ └──────┬───────┘
        │                  │               │
        └──────────────────┴───────────────┘
                            │
                     ┌─────────────┐
                     │   RabbitMQ   │
                     │(Message Broker)│
                     └─────────────┘
```

Each service is independently deployable and communicates with others either through direct REST calls (via the gateway) or asynchronously through RabbitMQ queues, keeping services loosely coupled.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB + Mongoose |
| Messaging | RabbitMQ |
| API Gateway | Express.js + express-http-proxy |
| Authentication | JWT, bcrypt, cookie-parser |
| Architecture Style | Microservices |

---

## 📂 Project Structure

```
uber-clone-microservices/
├── api-gateway/          # Central gateway routing requests to services
│   ├── src/
│   └── package.json
├── user-service/         # Handles user registration, login, profile
│   ├── src/
│   └── package.json
├── captain-service/       # Handles captain registration & management
│   ├── src/
│   └── package.json
├── ride-service/          # Handles ride creation & ride lifecycle
│   ├── src/
│   └── package.json
└── README.md
```

> Note: Adjust the folder names above to match your actual repository structure.

---

## ⚙️ Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v16+)
- [MongoDB](https://www.mongodb.com/)
- [RabbitMQ](https://www.rabbitmq.com/) (running locally or via Docker)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Mohd-fazil-01/uber-clone-microservices.git
   cd uber-clone-microservices
   ```

2. **Install dependencies for each service**
   ```bash
   cd api-gateway && npm install
   cd ../user-service && npm install
   cd ../captain-service && npm install
   cd ../ride-service && npm install
   ```

3. **Set up environment variables**

   Create a `.env` file inside each service folder:
   ```env
   PORT=4001
   MONGODB_URI=mongodb://localhost:27017/user-service
   JWT_SECRET=your_jwt_secret
   RABBITMQ_URL=amqp://localhost
   ```

4. **Start RabbitMQ** (if not already running)
   ```bash
   # Using Docker
   docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
   ```

5. **Run each service** (in separate terminals)
   ```bash
   cd api-gateway && npm start
   cd user-service && npm start
   cd captain-service && npm start
   cd ride-service && npm start
   ```

6. **Access the app**
   All requests go through the API Gateway:
   ```
   http://localhost:<gateway-port>
   ```

---

## 🔑 API Overview

| Service | Endpoint | Description |
|---|---|---|
| User | `POST /users/register` | Register a new user |
| User | `POST /users/login` | User login |
| Captain | `POST /captains/register` | Register a new captain |
| Captain | `POST /captains/login` | Captain login |
| Ride | `POST /rides/create` | Create a new ride request |
| Ride | `GET /rides/:id` | Get ride details |

> Update this table with your actual routes and request/response formats.

---

## 🧪 Testing

You can test the APIs using **Postman** or **Thunder Client** by hitting the API Gateway URL, which forwards requests to the appropriate microservice.

---

## 🚀 Future Improvements

- [ ] Add a real-time location tracking service using WebSockets
- [ ] Add Docker Compose for one-command multi-service startup
- [ ] Add API documentation with Swagger
- [ ] Add unit and integration tests
- [ ] Add a payment service

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to open an issue or submit a pull request.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 👤 Author

**Mohd Fazil**
- GitHub: [@Mohd-fazil-01](https://github.com/Mohd-fazil-01)
- LinkedIn: [mohd-fazil0011](https://www.linkedin.com/in/mohd-fazil0011/)
- Portfolio: [persnal-portfolio-01.netlify.app](https://persnal-portfolio-01.netlify.app/)
