# Senior Experts Platform

Welcome to the **Senior Experts** repository! This platform is specifically designed to connect **Senior Citizens (SE)** with **Small and Medium Enterprises (SMEs)**. SMEs can post jobs and Senior Citizens can apply for those jobs. Additionally, Senior Experts allows individuals to find mentors among senior citizens and book mentorship slots with them.

## Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
    - [Web App](#web-app)
    - [Mobile App](#mobile-app)
    - [Backend Services](#backend-services)
3. [Services Breakdown](#services-breakdown)
    - [Auth Service](#auth-service)
    - [Product Setup Service](#product-setup-service)
    - [Mentorship Service](#mentorship-service)
    - [Chat Service](#chat-service)
    - [Notification / Communication Service](#notification--communication-service)
4. [Technologies Used](#technologies-used)
5. [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation & Setup](#installation--setup)
    - [Running the Services](#running-the-services)
6. [Environment Variables](#environment-variables)
7. [Contributing](#contributing)
8. [License](#license)
9. [Contact](#contact)

---

## Overview

**Senior Experts** is a platform aiming to:
- Allow SMEs to post job opportunities targeted at senior citizens.
- Enable senior citizens to apply for these job postings.
- Provide mentorship sessions through a dedicated mentorship system, allowing scheduling and booking of time slots with senior mentors.

The solution is composed of multiple components for web, mobile, and backend services, leveraging microservices and message queues for efficient communication.

---

## Architecture

The platform is separated into three primary parts:
1. **Web App (Next.js)**
2. **Mobile App (React Native)**
3. **Backend (NestJS microservices + MySQL)**

Below is a high-level diagram of the architecture (conceptually):

```
          [Web App - Next.js :3000]    [Mobile App - React Native]
                         |                     |
                         v                     v
                  [Backend Gateway / Microservices]
             (Auth, Product Setup, Mentorship, Chat, Notifications)
                         |                     ^
                         |                     |
                         v                     |
                      [MySQL Database] <--> [RabbitMQ]
```

---

### Web App

- **Framework**: [Next.js](https://nextjs.org/)
- **Port**: `3000`
- **Purpose**:  
  - Provide a responsive interface for SMEs to post jobs.
  - Enable Senior Citizens to browse and apply for jobs.
  - Offer mentorship-related features (finding mentors, booking slots).

---

### Mobile App

- **Framework**: [React Native](https://reactnative.dev/)
- **Platform**: iOS and Android
- **Purpose**:  
  - Provide an easy-to-use mobile interface for both SMEs and Senior Citizens.
  - Supports real-time notifications, job browsing, and mentoring booking functionalities.

---

### Backend Services

The backend is composed of **five** [NestJS](https://nestjs.com/) microservices, each responsible for a specific domain. All services communicate using HTTP (for direct requests) and **RabbitMQ** for inter-service messaging. A shared **MySQL** database is used for persistent data storage.

1. [Auth Service](#auth-service) - Port `4000`  
2. [Product Setup Service](#product-setup-service) - Port `4001`  
3. [Mentorship Service](#mentorship-service) - Port `4002`  
4. [Chat Service](#chat-service) - Port `4003` (example)  
5. [Notification / Communication Service](#notification--communication-service) - Port `4004` (example)

> **Note:** The exact port for each service can be customized in your environment files. The ports listed here are for reference.

---

## Services Breakdown

### Auth Service

- **Port**: `4000`
- **Responsibilities**:
  - User authentication and authorization (JWT or other).
  - Profile management (managing user details, roles, etc.).
  - Basic networking features (e.g., searching for other users).

### Product Setup Service

- **Port**: `4001`
- **Responsibilities**:
  - Managing projects, job postings, and applications.
  - Storing job details posted by SMEs.
  - Handling applications from senior citizens.

### Mentorship Service

- **Port**: `4002`
- **Responsibilities**:
  - Managing mentor-mentee relationships.
  - Handling session scheduling and slot booking.
  - Integration with the notification service to remind mentors and mentees of upcoming sessions.

### Chat Service

- **Port**: `4003` (example)
- **Responsibilities**:
  - Real-time chat functionality between SMEs and Senior Citizens.
  - Storing chat history and delivering messages (through RabbitMQ or WebSockets).
  - Possibly integrates with the Notification Service to alert users of new messages.

### Notification / Communication Service

- **Port**: `4004` (example)
- **Responsibilities**:
  - Receives messages from other services via **RabbitMQ**.
  - Sends email, SMS, and push notifications.
  - Uses [OneSignal](https://onesignal.com/) for push notifications to mobile and web.

---

## Technologies Used

1. **Frontend**  
   - [Next.js](https://nextjs.org/) (web)  
   - [React Native](https://reactnative.dev/) (mobile)

2. **Backend**  
   - [NestJS](https://nestjs.com/) (Auth, Product Setup, Mentorship, Chat, Notifications)
   - [MySQL](https://www.mysql.com/) (database)

3. **Messaging & Notifications**  
   - [RabbitMQ](https://www.rabbitmq.com/) (inter-service communication)
   - [OneSignal](https://onesignal.com/) (push notifications)
   - Emails & SMS (via external providers or specialized NestJS modules/plugins)

---

## Getting Started

### Prerequisites

- **Node.js** (v14+ recommended)
- **npm** or **yarn**
- **MySQL** server
- **RabbitMQ** server

### Installation & Setup

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/YourOrganization/senior-experts-platform.git
   cd senior-experts-platform
   ```
2. **Install Dependencies**  
   - For the web app (`/web` folder):  
     ```bash
     cd web
     npm install
     ```
   - For the mobile app (`/mobile` folder):  
     ```bash
     cd mobile
     npm install
     ```
   - For each backend service (`/services/auth`, `/services/product-setup`, etc.):  
     ```bash
     cd services/auth
     npm install
     # Repeat for each service
     ```

3. **Configure Environment Variables**  
   - Each service has its own `.env` file. Refer to [Environment Variables](#environment-variables) for key settings.

### Running the Services

1. **Start MySQL**  
   Ensure MySQL is running on the default port (3306) or update the services’ `.env` files accordingly.

2. **Start RabbitMQ**  
   Ensure RabbitMQ is running, typically accessible on `amqp://localhost:5672` (or another configured host).

3. **Start Backend Services**  
   - **Auth Service**:
     ```bash
     cd services/auth
     npm run start
     ```
   - **Product Setup Service**:
     ```bash
     cd services/product-setup
     npm run start
     ```
   - **Mentorship Service**:
     ```bash
     cd services/mentorship
     npm run start
     ```
   - **Chat Service**:
     ```bash
     cd services/chat
     npm run start
     ```
   - **Notification / Communication Service**:
     ```bash
     cd services/notifications
     npm run start
     ```

4. **Start the Web App**  
   ```bash
   cd web
   npm run dev
   ```
   The web app should be accessible at [http://localhost:3000](http://localhost:3000).

5. **Start the Mobile App**  
   - For Android:
     ```bash
     cd mobile
     npm run android
     ```
   - For iOS (on macOS):
     ```bash
     cd mobile
     npm run ios
     ```

---

## Environment Variables

Below are some common environment variables needed across different services. Each microservice may have its own `.env` file with additional variables.

**Common Variables**:
- `DATABASE_HOST` – MySQL database host
- `DATABASE_PORT` – MySQL database port
- `DATABASE_USER` – MySQL user
- `DATABASE_PASSWORD` – MySQL password
- `DATABASE_NAME` – MySQL database name
- `RABBITMQ_HOST` – RabbitMQ host
- `RABBITMQ_PORT` – RabbitMQ port
- `ONESIGNAL_APP_ID` – OneSignal app ID
- `ONESIGNAL_API_KEY` – OneSignal REST API key
- `EMAIL_SMTP_HOST`, `EMAIL_SMTP_PORT`, `EMAIL_USER`, `EMAIL_PASSWORD` – SMTP details if using email notifications

**Service-Specific Variables**:
- **Auth Service**: `JWT_SECRET`, `PORT`, etc.
- **Product Setup Service**: `PORT`, any product-specific environment variables.
- **Mentorship Service**: `PORT`, scheduling configuration, etc.
- **Chat Service**: `PORT`, chat message queue routing, etc.
- **Notification Service**: `PORT`, email/SMS provider keys, etc.

---

## Contributing

1. Fork the repository on GitHub.
2. Create a new branch for your feature/bug fix.
3. Commit your changes to that branch.
4. Push your work to GitHub and open a Pull Request against the `main` branch.

Please follow the existing code style and add relevant tests for any new features or bug fixes.

---

## License

[MIT License](LICENSE) – Feel free to use and modify this repository under the terms of this license.

---

## Contact

For questions or support, please reach out to the maintainers:

- **Email**: [support@senior-experts.com](mailto:support@senior-experts.com)
- **Issues**: [GitHub Issues](https://github.com/YourOrganization/senior-experts-platform/issues)

We appreciate your feedback and contributions to make **Senior Experts** even better!
