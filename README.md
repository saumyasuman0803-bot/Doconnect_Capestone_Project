# Doconnect_Capestone_Project
Microservice-based Q&amp;A platform built with Spring Boot – separate user &amp; admin portals, JWT auth, and email notifications via a dedicated notification service.
# DoConnect – Q&A Microservices Application
DoConnect is a microservice-based Question & Answer platform built with Spring Boot.  
It provides separate portals for **Users** and **Admins**, with features like asking and answering questions, comments, chat, and email notifications.

---

## Table of Contents

- [Architecture](#architecture)
- [Features](#features)
  - [User Features](#user-features)
  - [Admin Features](#admin-features)
  - [Technical Features](#technical-features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Database Setup](#database-setup)
  - [Email / Notification Setup](#email--notification-setup)
  - [Running the Services](#running-the-services)
- [Usage](#usage)
  - [User Portal](#user-portal)
  - [Admin Portal](#admin-portal)
- [Future Improvements](#future-improvements)
- [Screenshots](#screenshots)
- [License](#license)

---

## Architecture

DoConnect is split into **four Spring Boot microservices**:

1. **Eureka Server** (`eureka-server`)
   - Service discovery for all other microservices.
   - Default port: **8761**

2. **User Service** (`user-service`)
   - Handles user registration, authentication (JWT), questions, answers, comments, and chat.
   - Exposes the main User UI (JSP pages).
   - Default port: **8081**

3. **Owner / Admin Service** (`owner-service`)
   - Admin portal for managing users and questions.
   - Shows statistics dashboard (users, questions, pending, approved).
   - Default port: **8082**

4. **Notification Service** (`notification-service`)
   - Independent microservice to send email notifications (for new questions and answers).
   - Default port: **8083**

All services register themselves with Eureka and communicate via **REST** using **Spring Cloud OpenFeign**.

---

## Features

### User Features

- User registration and login (JWT-based).
- Personal dashboard showing:
  - Total questions asked
  - Total answers posted
- Ask new questions (with title, content, tags, etc.).
- Browse and search questions.
- View question details (with all answers and comments).
- Post answers to open questions.
- Comment on answers.
- Simple chat between users.

### Admin Features

- Admin (Owner) registration and login.
- Admin dashboard showing:
  - Number of users
  - Number of questions
  - Pending questions
  - Approved questions
- Manage Users:
  - View users
  - (Optionally) deactivate users instead of deleting.
- Manage Questions:
  - View all questions
  - Approve / reject questions
  - Close / mark questions as resolved
  - Delete inappropriate questions
- Manage Answers **from question detail page**:
  - Approve / reject answers
  - Delete inappropriate answers
- Receive email notifications on new questions and answers (via `notification-service`).

### Technical Features

- Microservice architecture using **Spring Cloud Netflix Eureka** for service discovery.
- Inter-service communication using **Spring Cloud OpenFeign**.
- **Spring Security + JWT** for authentication in user service.
- **JPA/Hibernate** for database access.
- Layered architecture:
  - Controller → Service → Repository → Entity
- JSP + Bootstrap UI for both user and admin portals.

---

## Tech Stack

- **Backend**
  - Java 17+
  - Spring Boot 3.x
  - Spring Web
  - Spring Data JPA
  - Spring Security + JWT
  - Spring Cloud Netflix Eureka
  - Spring Cloud OpenFeign
- **Database**
  - MySQL (for user-service and owner-service)
- **View Layer**
  - JSP, JSTL
  - HTML5, CSS3, Bootstrap
- **Email**
  - Spring Mail (JavaMailSender) via Gmail SMTP

---

## Project Structure

High-level structure (root folder):

```text
DoConnect/
├── eureka-server/
│   └── src/main/java/... (EurekaServerApplication)
├── user-service/
│   ├── src/main/java/com/doconnect/userservice/
│   │   ├── controller/
│   │   ├── entity/
│   │   ├── repository/
│   │   ├── service/
│   │   ├── security/
│   │   └── client/
│   └── src/main/webapp/WEB-INF/views/ (JSPs for user portal)
├── owner-service/
│   ├── src/main/java/com/doconnect/ownerservice/
│   │   ├── controller/
│   │   ├── entity/
│   │   ├── repository/
│   │   ├── service/
│   │   └── client/
│   └── src/main/webapp/WEB-INF/views/ (JSPs for admin portal)
└── notification-service/
    └── src/main/java/com/doconnect/notificationservice/
        ├── controller/
        └── service/
