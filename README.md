# Mini Project 2: Smart Task Manager API Database Edition

## Overview

In this assignment, you will continue working on your previous Smart Task Manager API project.

In the first version, your tasks were stored in memory using a Python list or dictionary. In this version, you will upgrade your application so that tasks are stored permanently in a real database.

The main goal of this project is to practice integrating a FastAPI backend with a database, writing database queries, improving API performance, and keeping your code clean and maintainable.

This project is designed to take approximately 24 hours.

---

## Learning Goals

By completing this project, you will practice:

* Working with relational or nonrelational databases
* Connecting a FastAPI application to a database
* Performing CRUD operations using database queries
* Filtering and searching data from the database
* Adding pagination to API responses
* Applying basic performance improvements
* Writing tests for a database-backed API

---

## Project Requirements

## 1. Database Integration

You will replace the in-memory storage from the previous project with a real database.

You can choose one of the following options:

### Option A: Relational Database

You can use:

* SQLite
* PostgreSQL

Recommended tools:

* SQLAlchemy
* SQLModel
* Alembic, optional

### Option B: Nonrelational Database

You can use:

* MongoDB

Recommended tools:

* PyMongo
* Motor

Your task data should persist after restarting the server.

You should not use a global Python list or dictionary as the main storage for tasks anymore.

---

## 2. Task Data Model

Each task should contain the following fields:

```json
{
  "id": "integer or string",
  "title": "string",
  "description": "string",
  "status": "todo | in_progress | done",
  "priority": "low | medium | high",
  "tags": ["string"],
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

In this version, you should add an `updated_at` field.

The `updated_at` value should change whenever a task is updated.

---

## 3. API Endpoints

You will implement the following API endpoints:

| Method       | Endpoint           | Description        |
| ------------ | ------------------ | ------------------ |
| POST         | `/tasks`           | Create a new task  |
| GET          | `/tasks`           | Get all tasks      |
| GET          | `/tasks/{task_id}` | Get one task by ID |
| PUT or PATCH | `/tasks/{task_id}` | Update a task      |
| DELETE       | `/tasks/{task_id}` | Delete a task      |

Your API should:

* Return `404` when a task does not exist
* Return `422` when request data is invalid
* Use proper HTTP status codes
* Return consistent response formats

---

## 4. Filtering and Search

The `GET /tasks` endpoint should support filtering and search using query parameters.

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
* Searching in title and description

Important: filtering and search should happen through database queries. You should not load all tasks from the database and then filter them manually in Python.

---

## 5. Pagination

You will add pagination to the `GET /tasks` endpoint.

Example requests:

```text
/tasks?limit=10&offset=0
/tasks?limit=10&offset=10
```

Pagination requirements:

* Default `limit`: `10`
* Maximum `limit`: `50`
* Default `offset`: `0`

Example response format:

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

## 6. Performance Improvement

You will identify and apply at least one simple performance improvement.

Examples:

* Add an index on `status`
* Add an index on `priority`
* Add an index on `created_at`
* Add a text index for search if you use MongoDB
* Avoid loading all records before filtering
* Use pagination instead of returning unlimited results

In your README, briefly explain:

* What performance improvement you added
* Why it helps

---

## 7. Database Seeding

You will add a simple seed script that inserts sample tasks into your database.

Example command:

```bash
python seed.py
```

Your seed script should create at least 20 sample tasks with different:

* statuses
* priorities
* tags
* titles
* descriptions

This will help you test filtering, searching, and pagination properly.

---

## 8. Testing

You will write tests for your database-backed API.

Your tests should cover at least:

* Creating a task
* Getting all tasks
* Getting one task by ID
* Updating a task
* Deleting a task
* Returning `404` for an invalid task ID
* Returning `422` for invalid status or priority
* Filtering by status
* Filtering by priority
* Searching by keyword
* Pagination behavior

Your tests should not depend on production data.

You can use:

* A separate test database
* A temporary SQLite database
* A test MongoDB collection
* Database cleanup before each test

---

## Optional Bonus Features

You can add one or more of the following bonus features.

## Bonus 1: Task Statistics Endpoint

Add an endpoint:

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

## Bonus 2: Sorting

Support sorting in the `GET /tasks` endpoint.

Example requests:

```text
/tasks?sort_by=created_at&order=desc
/tasks?sort_by=priority&order=asc
```

Allowed `sort_by` values:

* `created_at`
* `updated_at`
* `priority`
* `status`

Allowed `order` values:

* `asc`
* `desc`

---

## Bonus 3: Archive Tasks

Add an endpoint:

```text
PATCH /tasks/{task_id}/archive
```

Archived tasks should not appear in the default task list.

To include archived tasks, support:

```text
/tasks?include_archived=true
```

This feature requires adding an `is_archived` field to your task model.

---

## Suggested Project Structure

If you choose a relational database, you can use this structure:

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

If you choose MongoDB, you can use this structure:

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

You do not have to follow this exact structure, but your project should be clean and easy to understand.

---

## Estimated Time Breakdown

| Work Item                                         | Estimated Time |
| ------------------------------------------------- | -------------: |
| Review previous project and plan database changes |        2 hours |
| Set up database connection and models/collections |        4 hours |
| Implement database-backed CRUD                    |        5 hours |
| Implement filtering and search                    |        3 hours |
| Add pagination                                    |        2 hours |
| Add performance improvement/indexing              |        2 hours |
| Add seed script                                   |        2 hours |
| Write tests                                       |        3 hours |
| README and cleanup                                |         1 hour |

Total: approximately 24 hours

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

Total: 100 points

---

## Submission Guidelines

Your submission should include:

* A GitHub repository
* A working FastAPI project
* A database-backed task manager API
* A seed script
* Tests
* A README file

Your README should explain:

* How to install dependencies
* How to configure the database
* How to run the application
* How to run the seed script
* How to run tests
* Which database you used
* Which performance improvement you added and why

---

## Important Notes

* Do not use an in-memory list or dictionary as the main task storage.
* Keep your code organized and readable.
* Use meaningful function and variable names.
* Validate user input properly.
* Handle not-found cases clearly.
* Make sure your application still works after restarting the server.
* Commit your work regularly with meaningful commit messages.
