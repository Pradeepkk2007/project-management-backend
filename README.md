# 🚀 Project Camp Backend

A secure and scalable RESTful API backend for a collaborative **Project Management System**. Project Camp enables teams to create and manage projects, organize tasks and subtasks, collaborate with team members, maintain project notes, and securely manage user authentication and authorization.

---

## 📌 Overview

**Project Camp Backend** provides the API layer for a project management application with:

* 🔐 JWT-based authentication
* 👥 Role-based access control
* 📁 Project management
* ✅ Task and subtask management
* 👤 Team member management
* 📝 Project notes
* 📎 Task file attachments
* 📧 Email verification and password reset
* 🔄 Access/refresh token management
* ❤️ API health monitoring

The backend is designed to support three user roles:

| Role              | Description                                         |
| ----------------- | --------------------------------------------------- |
| **Admin**         | Full system access                                  |
| **Project Admin** | Administrative access within assigned projects      |
| **Member**        | Basic project access and task/subtask participation |

---

## ✨ Features

### 🔐 Authentication & Authorization

* User registration
* Email verification
* Secure login with JWT
* Access and refresh token mechanism
* Logout
* Get current user
* Change password
* Forgot password
* Reset password
* Resend email verification
* Role-based authorization

### 📁 Project Management

* Create projects
* View accessible projects
* View project details
* Update projects
* Delete projects
* View project members
* Add members to projects
* Update member roles
* Remove members

### ✅ Task Management

* Create tasks
* View project tasks
* View individual task details
* Update tasks
* Delete tasks
* Assign tasks to team members
* Track task status
* Upload multiple task attachments

### 📋 Subtask Management

* Create subtasks
* Update subtasks
* Delete subtasks
* Allow members to update subtask completion status

### 📝 Project Notes

* Create project notes
* View project notes
* View individual notes
* Update notes
* Delete notes

### ❤️ Health Monitoring

A dedicated health-check endpoint is available to verify API availability and system status.

---

## 👥 Role & Permission Matrix

| Feature                | Admin | Project Admin | Member |
| ---------------------- | :---: | :-----------: | :----: |
| Create Project         |   ✅   |       ❌       |    ❌   |
| Update Project         |   ✅   |       ❌       |    ❌   |
| Delete Project         |   ✅   |       ❌       |    ❌   |
| Manage Project Members |   ✅   |       ❌       |    ❌   |
| Create Tasks           |   ✅   |       ✅       |    ❌   |
| Update Tasks           |   ✅   |       ✅       |    ❌   |
| Delete Tasks           |   ✅   |       ✅       |    ❌   |
| View Tasks             |   ✅   |       ✅       |    ✅   |
| Create Subtasks        |   ✅   |       ✅       |    ❌   |
| Update Subtasks        |   ✅   |       ✅       |    ✅   |
| Delete Subtasks        |   ✅   |       ✅       |    ❌   |
| Create Notes           |   ✅   |       ❌       |    ❌   |
| Update Notes           |   ✅   |       ❌       |    ❌   |
| Delete Notes           |   ✅   |       ❌       |    ❌   |
| View Notes             |   ✅   |       ✅       |    ✅   |

---

## 🏗️ API Structure

The API is versioned under:

```text
/api/v1/
```

### Authentication

```text
/api/v1/auth/
```

| Method | Endpoint                           | Description               | Auth   |
| ------ | ---------------------------------- | ------------------------- | ------ |
| POST   | `/register`                        | Register a new user       | Public |
| POST   | `/login`                           | Login user                | Public |
| POST   | `/logout`                          | Logout user               | 🔒     |
| GET    | `/current-user`                    | Get current user          | 🔒     |
| POST   | `/change-password`                 | Change password           | 🔒     |
| POST   | `/refresh-token`                   | Refresh access token      | Public |
| GET    | `/verify-email/:verificationToken` | Verify email              | Public |
| POST   | `/forgot-password`                 | Request password reset    | Public |
| POST   | `/reset-password/:resetToken`      | Reset password            | Public |
| POST   | `/resend-email-verification`       | Resend verification email | 🔒     |

### Projects

```text
/api/v1/projects/
```

| Method | Endpoint                      | Description              |
| ------ | ----------------------------- | ------------------------ |
| GET    | `/`                           | List accessible projects |
| POST   | `/`                           | Create a project         |
| GET    | `/:projectId`                 | Get project details      |
| PUT    | `/:projectId`                 | Update project           |
| DELETE | `/:projectId`                 | Delete project           |
| GET    | `/:projectId/members`         | List project members     |
| POST   | `/:projectId/members`         | Add project member       |
| PUT    | `/:projectId/members/:userId` | Update member role       |
| DELETE | `/:projectId/members/:userId` | Remove project member    |

### Tasks

```text
/api/v1/tasks/
```

| Method | Endpoint                         | Description        |
| ------ | -------------------------------- | ------------------ |
| GET    | `/:projectId`                    | List project tasks |
| POST   | `/:projectId`                    | Create task        |
| GET    | `/:projectId/t/:taskId`          | Get task details   |
| PUT    | `/:projectId/t/:taskId`          | Update task        |
| DELETE | `/:projectId/t/:taskId`          | Delete task        |
| POST   | `/:projectId/t/:taskId/subtasks` | Create subtask     |
| PUT    | `/:projectId/st/:subTaskId`      | Update subtask     |
| DELETE | `/:projectId/st/:subTaskId`      | Delete subtask     |

### Notes

```text
/api/v1/notes/
```

| Method | Endpoint                | Description        |
| ------ | ----------------------- | ------------------ |
| GET    | `/:projectId`           | List project notes |
| POST   | `/:projectId`           | Create note        |
| GET    | `/:projectId/n/:noteId` | Get note details   |
| PUT    | `/:projectId/n/:noteId` | Update note        |
| DELETE | `/:projectId/n/:noteId` | Delete note        |

### Health Check

```text
GET /api/v1/healthcheck/
```

Returns the current API/system health status.

---

## 🔐 Authentication

Project Camp uses **JWT-based authentication**.

After successful login, the client receives an access token and refresh token.

Protected endpoints require authentication through the access token.

Example:

```http
Authorization: Bearer <access_token>
```

### Token Flow

```text
User
 │
 │ Login
 ▼
API
 │
 ├── Access Token
 └── Refresh Token
       │
       ▼
   Authenticated
       │
       ▼
Protected API Endpoints
```

When the access token expires, the refresh token can be used to obtain a new access token.

---

## 🛡️ Security

The backend includes several security mechanisms:

* JWT authentication
* Refresh token authentication
* Role-based authorization middleware
* Input validation
* Email verification
* Secure password hashing
* Password reset tokens
* CORS configuration
* Secure file upload handling
* File type and size validation
* Protected API routes

> ⚠️ Never commit `.env` files, JWT secrets, database credentials, API keys, or email service credentials to GitHub.

---

## 📎 File Attachments

Tasks support multiple file attachments.

Uploaded files include metadata such as:

```text
URL
MIME Type
File Size
```

Files are stored under:

```text
public/images/
```

The backend uses **Multer** for handling multipart file uploads.

---

## 📊 Data Models

### User Roles

```text
admin
project_admin
member
```

### Task Status

```text
todo
in_progress
done
```

---

## 📂 Recommended Project Structure

```text
project-camp-backend/
│
├── public/
│   └── images/
│
├── src/
│   ├── controllers/
│   ├── db/
│   ├── middlewares/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   ├── validators/
│   └── index.js
│
├── .env
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

> The exact directory structure may differ depending on the implementation.

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone <your-repository-url>
```

### 2. Navigate to the project

```bash
cd project-camp-backend
```

### 3. Install dependencies

```bash
npm install
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
PORT=8000

DATABASE_URL=your_database_url

ACCESS_TOKEN_SECRET=your_access_token_secret
ACCESS_TOKEN_EXPIRY=1d

REFRESH_TOKEN_SECRET=your_refresh_token_secret
REFRESH_TOKEN_EXPIRY=7d

CORS_ORIGIN=http://localhost:3000

SMTP_HOST=your_smtp_host
SMTP_PORT=587
SMTP_USER=your_smtp_username
SMTP_PASSWORD=your_smtp_password
```

> Update the variables according to your actual project configuration.

### 5. Start the development server

```bash
npm run dev
```

The API should then be available at:

```text
http://localhost:<PORT>
```

---

## 🚀 Available Scripts

Example:

```bash
npm run dev
```

Starts the development server using Nodemon.

If your `package.json` contains:

```json
{
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "nodemon src/index.js"
  }
}
```

you can start the project with:

```bash
npm run dev
```

or:

```bash
npm start
```

---

## 🩺 Health Check

Once the server is running, you can verify that the API is available:

```http
GET /api/v1/healthcheck/
```

Example:

```text
http://localhost:8000/api/v1/healthcheck/
```

---

## 🔄 Application Flow

A typical user workflow looks like:

```text
Register
   │
   ▼
Email Verification
   │
   ▼
Login
   │
   ▼
Receive JWT Tokens
   │
   ▼
Create / Join Project
   │
   ▼
Project Members
   │
   ├───────────────┐
   ▼               ▼
Create Tasks     Project Notes
   │
   ▼
Assign Members
   │
   ▼
Create Subtasks
   │
   ▼
Track Progress
   │
   ├── Todo
   ├── In Progress
   └── Done
```

---

## 🧪 API Testing

You can test the API using tools such as:

* Postman
* Insomnia
* Thunder Client
* cURL

Example:

```bash
curl http://localhost:8000/api/v1/healthcheck/
```

For protected endpoints, include the JWT:

```http
Authorization: Bearer <access_token>
```

---

## 🌐 Frontend Integration

Project Camp Backend is designed to work with modern frontend applications such as:

* React
* Angular
* Vue
* Next.js

The frontend communicates with this backend through REST APIs.

Example:

```text
┌───────────────┐
│ React /       │
│ Angular       │
└───────┬───────┘
        │
        │ HTTP / REST API
        ▼
┌─────────────────────┐
│ Project Camp        │
│ Backend             │
└──────────┬──────────┘
           │
     ┌─────┴─────┐
     ▼           ▼
 Database     File Storage
```

---

## 🔑 Environment Variables

Do not commit sensitive environment variables to GitHub.

Add the following to `.gitignore`:

```gitignore
node_modules/
.env
.env.*
public/images/*
```

You can provide a safe template using:

```text
.env.example
```

For example:

```env
PORT=

DATABASE_URL=

ACCESS_TOKEN_SECRET=
ACCESS_TOKEN_EXPIRY=

REFRESH_TOKEN_SECRET=
REFRESH_TOKEN_EXPIRY=

CORS_ORIGIN=

SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASSWORD=
```

---

## 📈 Future Improvements

Potential future enhancements include:

* Swagger / OpenAPI documentation
* Real-time notifications using WebSockets
* Project activity/audit logs
* Task comments
* Task priority levels
* Task labels/tags
* Search and filtering
* Pagination
* Advanced project analytics
* Docker support
* Automated testing and CI/CD
* Cloud file storage
* Production monitoring

---

## 🤝 Contributing

Contributions are welcome.

### Steps

1. Fork the repository
2. Create a feature branch

```bash
git checkout -b feature/my-feature
```

3. Make your changes
4. Commit your changes

```bash
git add .
git commit -m "Add my feature"
```

5. Push the branch

```bash
git push origin feature/my-feature
```

6. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License**.

See the `LICENSE` file for more information.

---

## 👨‍💻 Author

**Project Camp**

Built as a collaborative project management backend with authentication, authorization, task management, project management, and team collaboration capabilities.

---

⭐ If you find this project useful, consider giving the repository a star!
