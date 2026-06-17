# Project Management System - Database Schema

The application uses PostgreSQL with Prisma ORM. Below is the Entity-Relationship (ER) diagram representing the database architecture.

```mermaid
erDiagram
    User ||--o{ Project : "owns"
    Project ||--o{ Task : "contains"

    User {
        String id PK "uuid"
        String email "unique"
        String password 
        String name 
        DateTime createdAt 
        DateTime updatedAt 
    }

    Project {
        String id PK "uuid"
        String name 
        String description "nullable"
        ProjectStatus status "NOT_STARTED, IN_PROGRESS, COMPLETED"
        DateTime startDate "nullable"
        DateTime endDate "nullable"
        String userId FK "Cascade Delete"
        DateTime createdAt 
        DateTime updatedAt 
    }

    Task {
        String id PK "uuid"
        String name 
        String description "nullable"
        Priority priority "LOW, MEDIUM, HIGH"
        TaskStatus status "PENDING, IN_PROGRESS, COMPLETED"
        DateTime dueDate "nullable"
        String projectId FK "Cascade Delete"
        DateTime createdAt 
        DateTime updatedAt 
    }
```

> [!NOTE]
> The database also contains a `_prisma_migrations` table used internally by Prisma to track schema changes.
