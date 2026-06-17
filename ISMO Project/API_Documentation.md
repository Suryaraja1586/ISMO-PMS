# Project Management System - API Documentation

Base URL for all endpoints is `/api`. All endpoints except Auth require a valid JWT Bearer token in the `Authorization` header.

## Authentication (`/api/auth`)

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| `POST` | `/register` | Register a new user | `{ name, email, password }` | `201 Created` with JWT token and user info |
| `POST` | `/login` | Authenticate a user | `{ email, password }` | `200 OK` with JWT token and user info |
| `POST` | `/logout` | Logout a user | None | `200 OK` |

## Dashboard (`/api/dashboard`)

| Method | Endpoint | Description | Query Parameters | Response |
|--------|----------|-------------|------------------|----------|
| `GET` | `/stats` | Get dashboard statistics | None | `200 OK` with counts (projects, tasks), recent projects, and tasks due soon |

## Projects (`/api/projects`)

| Method | Endpoint | Description | Payload / Query | Response |
|--------|----------|-------------|-----------------|----------|
| `POST` | `/` | Create a new project | Body: `{ name, description?, status?, startDate?, endDate? }` | `201 Created` with project data |
| `GET` | `/` | List user's projects | Query: `search`, `status`, `sortBy`, `sortOrder`, `page`, `limit` | `200 OK` with paginated projects array |
| `GET` | `/:id` | Get project details | None | `200 OK` with project data and associated tasks |
| `PUT` | `/:id` | Update a project | Body: Partial project fields | `200 OK` with updated project data |
| `DELETE` | `/:id` | Delete a project | None | `200 OK` |

> [!WARNING]
> Deleting a project will permanently delete all associated tasks due to the Cascade delete relation.

## Tasks (`/api/tasks`)

| Method | Endpoint | Description | Payload / Query | Response |
|--------|----------|-------------|-----------------|----------|
| `POST` | `/` | Create a new task | Body: `{ name, description?, priority?, status?, dueDate?, projectId }` | `201 Created` with task data |
| `GET` | `/` | List user's tasks | Query: `projectId`, `search`, `status`, `priority`, `sortBy`, `sortOrder`, `page`, `limit` | `200 OK` with paginated tasks array |
| `GET` | `/:id` | Get task details | None | `200 OK` with task data |
| `PUT` | `/:id` | Update a task | Body: Partial task fields | `200 OK` with updated task data |
| `DELETE` | `/:id` | Delete a task | None | `200 OK` |
