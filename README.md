````markdown
# Mini Project 2: Smart Task Manager API — Database Edition

## Overview

In this mini project, you will upgrade your previous **Smart Task Manager API** from an in-memory application into a real database-backed backend system.

In the previous project, tasks were stored temporarily in a Python list or dictionary. This means all tasks disappeared whenever the server restarted. In this version, you will store tasks permanently in a database and improve your API with filtering, search, pagination, testing, and basic performance improvements.

This project is designed to help you practice:

- Intro to databases
- Nonrelational databases
- Interacting with databases
- Integrating APIs with databases
- Code performance basics

Estimated required time: **24 hours**

---

## Project Goal

Your goal is to take your previous Task Manager API and improve it so that tasks are stored permanently in a database.

Your API should still support:

- Creating tasks
- Getting all tasks
- Getting one task by ID
- Updating tasks
- Deleting tasks
- Filtering and searching tasks

However, this time all task data should come from the database, not from in-memory storage.

---

## Database Options

You can choose one of the following database paths.

### Option A: Relational Database

Use a relational database such as:

- SQLite
- PostgreSQL

Recommended tools:

- SQLAlchemy
- SQLModel
- Alembic, optional

### Option B: Nonrelational Database

Use a nonrelational database such as:

- MongoDB

Recommended tools:

- PyMongo
- Motor

Both options are acceptable as long as your API behavior follows the project requirements.

---

## Task Data Structure

Each task should include the following fields:

```json
{
  "id": "string or integer",
  "title": "string",
  "description": "string",
  "status": "todo | in_progress | done",
  "priority": "low | medium | high",
  "tags": ["string"],
  "created_at": "datetime",
  "updated_at": "datetime"
}
````

Compared to the previous project, you should add an `updated_at` field.

The `updated_at` value should change whenever a task is updated.

---

## Main Requirements

### 1. Database Integration

You should connect your FastAPI application to a real database.

Your application should:

* Store tasks in the database
* Keep data available after restarting the server
* Avoid using a global Python list or dictionary as the main storage
* Keep database connection/configuration separate from route logic
* Avoid hardcoding sensitive database configuration when possible

Examples of better configuration options:

* Environment variables
* `.env` file
* Configuration module

---

### 2. CRUD API Endpoints

You should implement the following endpoints:

| Method           | Endpoint           | Description        |
| ---------------- | ------------------ | ------------------ |
| `POST`           | `/tasks`           | Create a new task  |
| `GET`            | `/tasks`           | Get all tasks      |
| `GET`            | `/tasks/{task_id}` | Get one task by ID |
| `PUT` or `PATCH` | `/tasks/{task_id}` | Update a task      |
| `DELETE`         | `/tasks/{task_id}` | Delete a task      |

Your API should:

* Return `404` when a task does not exist
* Return `422` when request data is invalid
* Use proper HTTP status codes
* Keep response formats consistent
* Validate task status and priority values

Allowed status values:

```text
todo
in_progress
done
```

Allowed priority values:

```text
low
medium
high
```

---

### 3. Filtering and Search

Your `GET /tasks` endpoint should support query parameters.

Examples:

```text
/tasks?status=done
/tasks?priority=high
/tasks?tag=backend
/tasks?search=api
```

You should support:

* Filtering by status
* Filtering by priority
* Filtering by one tag
* Searching inside task title and description

Important:

Filtering and search should happen through database queries, not by loading all tasks from the database and filtering them manually in Python.

---

### 4. Pagination

You should add pagination to `GET /tasks`.

Examples:

```text
/tasks?limit=10&offset=0
/tasks?limit=10&offset=10
```

Pagination requirements:

* Default `limit`: `10`
* Maximum `limit`: `50`
* Default `offset`: `0`
* The response should include the task data and pagination metadata

Example response:

```json
{
  "items": [
    {
      "id": 1,
      "title": "Learn database queries",
      "description": "Practice filtering tasks from database",
      "status": "todo",
      "priority": "high",
      "tags": ["backend", "database"],
      "created_at": "2026-05-11T10:00:00",
      "updated_at": "2026-05-11T10:00:00"
    }
  ],
  "limit": 10,
  "offset": 0,
  "count": 1
}
```

---

### 5. Code Performance Improvement

You should identify and apply at least one simple performance improvement.

Examples:

* Add an index on `status`
* Add an index on `priority`
* Add an index on `created_at`
* Add a text index for search if you use MongoDB
* Avoid loading all records before filtering
* Use pagination instead of returning unlimited results

You should briefly explain your chosen performance improvement in your README file.

Your explanation can be short, but it should answer:

* What did you improve?
* Why does it help performance?

Example:

```text
I added an index on the status field because the API allows users to filter tasks by status. This helps the database find matching tasks faster instead of scanning all records.
```

---

### 6. Database Seeding Script

You should create a simple script that inserts sample tasks into your database.

Example command:

```bash
python seed.py
```

Your seed script should create at least 20 tasks with different:

* Status values
* Priority values
* Tags
* Titles
* Descriptions

This will help you test filtering, searching, pagination, and performance behavior.

---

### 7. Testing

You should write tests for your database-backed API.

Minimum required tests:

* Create a task
* Get all tasks
* Get one task by ID
* Update a task
* Delete a task
* Invalid task ID returns `404`
* Invalid status returns `422`
* Invalid priority returns `422`
* Filter by status
* Filter by priority
* Search by keyword
* Pagination works correctly

Important:

Your tests should not depend on production data.

You can use one of these approaches:

* A separate test database
* A temporary SQLite database
* A MongoDB test collection
* Database cleanup before each test

---

## Optional Bonus Features

You can implement one or more of the following bonus features.

---

### Bonus 1: Task Statistics Endpoint

Add this endpoint:

```text
GET /tasks/stats
```

Example response:

```json
{
  "total_tasks": 25,
  "by_status": {
    "todo": 10,
    "in_progress": 8,
    "done": 7
  },
  "by_priority": {
    "low": 5,
    "medium": 12,
    "high": 8
  }
}
```

The statistics should be calculated using database queries or database aggregation.

---

### Bonus 2: Sorting Tasks

Support sorting in `GET /tasks`.

Examples:

```text
/tasks?sort_by=created_at&order=desc
/tasks?sort_by=priority&order=asc
```

Allowed `sort_by` values:

```text
created_at
updated_at
priority
status
```

Allowed `order` values:

```text
asc
desc
```

---

### Bonus 3: Archive Tasks

Add an archive feature.

Endpoint:

```text
PATCH /tasks/{task_id}/archive
```

Archived tasks should not appear in the default task list unless requested.

Example:

```text
/tasks?include_archived=true
```

For this feature, you should add an `is_archived` field to your task data.

---

## Suggested Folder Structure

You can organize your project in different ways, but the following structures are recommended.

---

### Relational Database Option

```text
project/
├── app/
│   ├── main.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── crud.py
│   ├── routes/
│   │   └── tasks.py
│   └── templates/
│       └── index.html
│
├── tests/
│   └── test_tasks.py
│
├── seed.py
├── requirements.txt
└── README.md
```

---

### MongoDB Option

```text
project/
├── app/
│   ├── main.py
│   ├── database.py
│   ├── schemas.py
│   ├── routes/
│   │   └── tasks.py
│   ├── services/
│   │   └── task_service.py
│   └── templates/
│       └── index.html
│
├── tests/
│   └── test_tasks.py
│
├── seed.py
├── requirements.txt
└── README.md
```

---

## Estimated Time Breakdown

| Work Item                                              | Estimated Time |
| ------------------------------------------------------ | -------------: |
| Review your previous project and plan database changes |        2 hours |
| Set up database connection and models/collections      |        4 hours |
| Implement database-backed CRUD                         |        5 hours |
| Implement filtering and search                         |        3 hours |
| Add pagination                                         |        2 hours |
| Add performance improvement/indexing                   |        2 hours |
| Add seed script                                        |        2 hours |
| Write tests                                            |        3 hours |
| README and cleanup                                     |         1 hour |

Total estimated time: **24 hours**

---

## Submission Guidelines

You should submit your project as a GitHub repository.

Your repository should include:

* Complete FastAPI application code
* Database integration
* Seed script
* Tests
* `requirements.txt` or equivalent dependency file
* README file with setup and run instructions

Your README should explain:

* Which database you used
* How to install dependencies
* How to configure environment variables, if needed
* How to run the application
* How to run the seed script
* How to run tests
* Which performance improvement you added

---

## Evaluation Criteria

| Criteria                                      | Points |
| --------------------------------------------- | -----: |
| Database integration and persistence          |     20 |
| Correct CRUD behavior                         |     20 |
| Filtering and search through database queries |     15 |
| Pagination implementation                     |     10 |
| Validation and error handling                 |     10 |
| Testing quality                               |     15 |
| Performance improvement and explanation       |      5 |
| Code organization and README                  |      5 |

Total: **100 points**

---

## Important Notes

* You should not use a global Python list or dictionary as your main task storage.
* You should not filter all tasks manually in Python after loading everything from the database.
* You should not commit sensitive credentials such as database passwords or API keys.
* You should keep your code readable and organized.
* You should commit your changes regularly with meaningful commit messages.

---

## Tips

* Start by connecting your application to the database.
* Then implement create and get endpoints.
* After that, add update and delete.
* Add filtering and search after the basic CRUD is working.
* Add pagination before testing with many tasks.
* Use the seed script to create enough data for testing.
* Test your API with Swagger UI at `/docs`.

Good luck and build something solid.

```
```
