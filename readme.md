# EstateFlow

EstateFlow is an event‐driven real estate web application built with a microservices architecture. It allows house checkers and potential clients to browse a collection of houses, marking them as liked or performing other actions. This repository contains the code for both the front‐end and the back‐end services (Config, Houses, and Core), all containerized with Docker and communicating via RabbitMQ as the message broker/event bus.

---

## Table of Contents

1. [Overview](#overview)  
2. [Architecture](#architecture)  
3. [Features](#features)  
4. [Tech Stack](#tech-stack)  
5. [Services](#services)  
6. [Getting Started](#getting-started)  
7. [Environment Variables](#environment-variables)  
8. [Docker Usage](#docker-usage)  
9. [Contributing](#contributing)  
10. [License](#license)

---

## Overview

EstateFlow is a microservices application designed to manage and display real estate listings. The system includes:

- A **front‐end** built in React (TypeScript), HTML, CSS, and Bootstrap.
- A **back‐end** composed of three services:
  - **Config** (Django) – Configures installed apps, URLs, middleware, and databases.
  - **Houses** (Django) – Manages house creation, listing, updating, and deletion.
  - **Core** (Flask) – Handles liking, checking, and other business logic actions for each house.
- A **message broker** (RabbitMQ) for event‐driven communication between microservices.
- Each service runs in its own **Docker** container and has its own dedicated **MySQL** database.

---

## Architecture

``` lua
+------------------------------+          +---------------------------------+
|       Front-end Service      |          |         Core App (Flask)        |
|   (React TS, HTML, CSS, ...) |          |    Docker + MySQL Database      |
|   Docker Container           |          |                                 |
+--------------+---------------+          +----------------+-----------------+
               |                                    |
               | Internal API Requests              |  
               |                                    |
+--------------v---------------+          +----------v------------+
|   Back-end Service           |          |      RabbitMQ         |
|   Docker Container           |          | (Message Broker/Event |
|  +---------+  +-----------+  |  <-----> |         Bus)          |
|  | Config  |  |  Houses   |  |          +----------^------------+
|  | (Django)|  | (Django)  |  |
+--------------+-------------+
```


1. **Front‐end** calls the back‐end services via internal API endpoints.  
2. **Config** app sets up middleware, installed apps, routes, and DB connections for the back‐end.  
3. **Houses** manages CRUD operations for house listings.  
4. **Core** performs house likes, checks, and other domain logic.  
5. **RabbitMQ** routes events/messages between the microservices.

---

## Features

1. **House Listing** – Create, list, update, and delete house entries.  
2. **House Liking** – Users can like houses via the Core service.  
3. **Event‐Driven Communication** – RabbitMQ handles asynchronous events and triggers.  
4. **Scalable Architecture** – Each microservice can be independently scaled as needed.  
5. **Containerized Deployment** – All services run in Docker containers.

---

## Tech Stack

- **Front‐end**: 
  - [React (TypeScript)](https://react.dev/)
  - [HTML5](https://developer.mozilla.org/en-US/docs/Web/HTML)
  - [CSS3](https://developer.mozilla.org/en-US/docs/Web/CSS)
  - [Bootstrap](https://getbootstrap.com/)

- **Back‐end**:
  - [Django](https://www.djangoproject.com/) (Config, Houses)
  - [Flask](https://flask.palletsprojects.com/) (Core)
  - [MySQL](https://www.mysql.com/) for databases
  - [RabbitMQ](https://www.rabbitmq.com/) as message broker

- **Deployment & Containerization**:
  - [Docker](https://www.docker.com/)
  - [Docker Compose](https://docs.docker.com/compose/)

---

## Services

1. **Config (Django)**  
   - Configures and includes all installed apps (such as the Houses app).  
   - Defines middleware, URLs/endpoints, and database connections.  

2. **Houses (Django)**  
   - Handles creation, listing, editing, and deletion of houses.  
   - Connects to its own MySQL database.  

3. **Core (Flask)**  
   - Manages house liking, checking, and other domain‐specific actions.  
   - Connects to its own MySQL database.  
   - Makes internal API requests to Config and Houses for cross‐service functionality.  

4. **Front‐end**  
   - React TypeScript application that interacts with the back‐end services via RESTful APIs.  
   - Displays the listings and enables user interactions (like and check).

5. **RabbitMQ**  
   - Serves as the message broker and event bus among the microservices.  
   - Facilitates async communication for tasks such as user notifications, database updates, etc.

---