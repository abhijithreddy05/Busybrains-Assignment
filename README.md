# Ecommerce Full-Stack Application

A modern, full-stack E-commerce web application with standard JWT authentication, Google OAuth 2.0 Single Sign-On (SSO), and Role-Based Access Control (RBAC).

## 🚀 Technologies Used
- **Frontend**: React (Create React App), React Router Dom, Axios
- **Backend**: Java 17, Spring Boot 3.x, Spring Security (JWT + OAuth2)
- **Database**: MySQL 8.0 (Dockerized)

---

## 🏗 System Architecture
The application follows a client-server architecture:
`User -> React UI -> API Calls (Axios) -> Spring Boot -> MySQL -> Response -> UI`

---

## 🛠 Setup & Installation

### Prerequisites
- Node.js (v16+)
- Java 17+ and Maven
- Docker (for MySQL database)
- Google Cloud Console Account (for OAuth2 credentials)

### 1. Database Setup (Docker)
A `docker-compose.yml` is provided to spin up a local MySQL instance quickly:
```bash
cd ecommerce-app-backend
docker-compose up -d
```

### 2. Google OAuth2 Configuration (SSO)
To enable Google Login, you need a Google Client ID and Secret:
1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create OAuth 2.0 credentials.
3. Add the authorized redirect URI: `http://localhost:8080/login/oauth2/code/google`
4. Update `application.properties` in `ecommerce-app-backend/src/main/resources`:
   ```properties
   spring.security.oauth2.client.registration.google.client-id=YOUR_CLIENT_ID
   spring.security.oauth2.client.registration.google.client-secret=YOUR_SECRET
   ```

### 3. Backend Setup
```bash
cd ecommerce-app-backend
./mvnw clean install
./mvnw spring-boot:run
```
The backend will run on `http://localhost:8080`.
*(Note: Initial starting will seed 2 users (`admin` & `user`) and sample products).*

### 4. Frontend Setup
```bash
cd ecommerce-app-frontend
npm install
npm start
```
The frontend will run on `http://localhost:3000`.

---

## 🔐 Role-Based Access Control (RBAC) & Predefined Users
| Username | Password | Role | Capabilities |
| --- | --- | --- | --- |
| `admin` | `password` | **ADMIN** | Can view, add, edit, and delete products. |
| `user` | `password` | **USER** | Can only participate and view products. |

---

## 📡 API Details
### Auth & Profile
- `POST /api/auth/login` - Authenticate using username and password. Returns JWT.
- `POST /api/auth/register` - Create a standard user account.
- `GET /oauth2/authorization/google` - Initiate Google SSO flow.
- `GET /api/profile` - Get logged-in user profile details.
- `PUT /api/profile` - Update profile information.
- `PUT /api/profile/change-password` - Change the current user password.

### Products
- `GET /api/products` - View all products (Public/Authenticated).
- `POST /api/products` - Add product (**ADMIN ONLY**).
- `PUT /api/products/{id}` - Update product (**ADMIN ONLY**).
- `DELETE /api/products/{id}` - Delete product (**ADMIN ONLY**).

---

## 🛡 Security Best Practices Implemented
- State-of-the-art **BCrypt Password Hashing**.
- **JWT (JSON Web Tokens)** for stateless, secure API communication.
- Role-based `@PreAuthorize` API restrictions.
- Dynamic React navigation and protected routing based on user context.
