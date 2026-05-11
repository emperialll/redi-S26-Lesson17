````markdown
# Mini Project 2: Smart Task Manager API - Database Edition

## Overview

In this mini project, you will continue improving your previous Smart Task Manager API.

In the first version, tasks were stored in memory using a Python list or dictionary. In this version, you will replace that temporary storage with a real database so that tasks are stored permanently.

The main goal of this project is to practice:

- Intro to databases
- Nonrelational databases
- Interacting with databases
- Integrating APIs with databases
- Basic code performance improvements

By the end of this project, your API should be able to create, read, update, delete, filter, search, and paginate tasks using a database.

---

## Estimated Time

This project is designed to take around 24 hours.

---

## Project Goal

You will upgrade your previous Task Manager API into a database-backed backend application.

Your API should still manage tasks, but all task data should now come from a database instead of an in-memory Python list or dictionary.

You can choose one of the following database options:

### Option A: Relational Database

Use SQLite or PostgreSQL with SQLAlchemy.

### Option B: Nonrelational Database

Use MongoDB with PyMongo or Motor.

Both options are acceptable as long as your API behavior matches the project requirements.

---

## Task Model

Each task should have the following structure:

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
```

Compared to the previous project, you should add an `updated_at` field.

The `created_at` field should show when the task was created.

The `updated_at` field should show when the task was last updated.

---

## Database Requirements

You must integrate a real database into your project.

Your database implementation should meet the following requirements:

- Task data should persist after restarting the server
- A global Python list or dictionary should not be used as the main storage
- Database connection logic should be separated from route logic
- Database configuration should not be hardcoded when possible
- Your API should interact with the database for creating, reading, updating, deleting, filtering, and searching tasks

---

## Required API Endpoints

You should implement the following endpoints:

| Method | Endpoint | Description |
|---|---|---|
| POST | `/tasks` | Create a new task |
| GET | `/tasks` | Get all tasks |
| GET | `/tasks/{task_id}` | Get one task by ID |
| PUT or PATCH | `/tasks/{task_id}` | Update a task |
| DELETE | `/tasks/{task_id}` | Delete a task |

---

## Expected API Behavior

Your API should behave consistently and return meaningful responses.

Expected behavior:

- Return `404 Not Found` if a task does not exist
- Return `422 Unprocessable Entity` for invalid request data
- Return proper HTTP status codes for create, update, and delete operations
- Keep response formats consistent across endpoints
- Validate allowed values for `status` and `priority`

Allowed `status` values:

```text
todo
in_progress
done
```

Allowed `priority` values:

```text
low
medium
high
```

---

## Filtering and Search

Your `GET /tasks` endpoint should support filtering and searching using query parameters.

Examples:

```text
/tasks?status=done
/tasks?priority=high
/tasks?tag=backend
/tasks?search=api
```

You should support:

- Filtering by status
- Filtering by priority
- Filtering by one tag
- Searching in title and description

Important:

Filtering and searching should happen through database queries.

You should not load all tasks from the database and then filter them manually in Python.

---

## Pagination

You should add pagination to the `GET /tasks` endpoint.

Examples:

```text
/tasks?limit=10&offset=0
/tasks?limit=10&offset=10
```

Pagination requirements:

- Default `limit`: `10`
- Maximum `limit`: `50`
- Default `offset`: `0`
- The response should include both task data and pagination metadata

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

## Performance Improvement

You should identify and apply at least one simple performance improvement.

Examples:

- Add an index on `status`
- Add an index on `priority`
- Add an index on `created_at`
- Add a text index for search if you use MongoDB
- Avoid loading all records before filtering
- Use pagination instead of returning unlimited results

In your README, briefly explain:

- Which performance improvement you added
- Why it improves the application
- Where it is implemented in your code

---

## Database Seeding

You should add a simple seed script to insert sample tasks into the database.

Example command:

```bash
python seed.py
```

Your seed script should create at least 20 tasks with different:

- statuses
- priorities
- tags
- titles
- descriptions

This will help you test filtering, searching, pagination, and performance behavior more easily.

---

## Testing

You should write tests for your database-backed API.

Minimum required tests:

- Create a task
- Get all tasks
- Get one task by ID
- Update a task
- Delete a task
- Invalid task ID returns `404`
- Invalid status or priority returns `422`
- Filter by status
- Filter by priority
- Search by keyword
- Pagination works correctly

Important:

Your tests should not depend on production data.

You can use:

- A separate test database
- A temporary SQLite test database
- A MongoDB test collection
- Database cleanup before each test

---

## Optional Bonus Features

You can add one or more of the following bonus features.

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

The statistics should be calculated using database queries or aggregation.

---

### Bonus 2: Sort Tasks

Support sorting in the `GET /tasks` endpoint.

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

Add this endpoint:

```text
PATCH /tasks/{task_id}/archive
```

Archived tasks should not appear in the default task list unless they are requested.

Example:

```text
/tasks?include_archived=true
```

This feature requires adding an `is_archived` field to your task model.

---

## Suggested Folder Structure

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

| Work item | Estimated time |
|---|---:|
| Review previous project and plan database changes | 2 hours |
| Set up database connection and models/collections | 4 hours |
| Implement database-backed CRUD | 5 hours |
| Implement filtering and search | 3 hours |
| Add pagination | 2 hours |
| Add performance improvement/indexing | 2 hours |
| Add seed script | 2 hours |
| Write tests | 3 hours |
| README and cleanup | 1 hour |

Total: around 24 hours

---

## Evaluation Criteria

| Criteria | Points |
|---|---:|
| Database integration and persistence | 20 |
| Correct CRUD behavior | 20 |
| Filtering and search through database queries | 15 |
| Pagination implementation | 10 |
| Validation and error handling | 10 |
| Testing quality | 15 |
| Performance improvement and explanation | 5 |
| Code organization and README | 5 |

Total: 100 points

---

## Submission Guidelines

Your submission should include:

- A GitHub repository link
- A working FastAPI application
- A database-backed implementation
- A seed script
- Tests
- A clear README file

Your README should include:

- Project description
- Database choice and setup instructions
- How to install dependencies
- How to run the server
- How to run the seed script
- How to run the tests
- API endpoint documentation
- Explanation of your performance improvement

---

## Rules

- Do not use an in-memory list or dictionary as the main task storage
- Do not hardcode sensitive database credentials
- Do not copy code from classmates
- Keep your code clean and organized
- Commit your changes regularly with meaningful commit messages

---

## Tips

- Start by connecting your database first
- Then move create, get, update, and delete logic one by one
- Test each endpoint after changing it
- Use Swagger UI at `/docs` to test your API
- Use seed data to test filters and pagination
- Keep database logic separate from route logic
- Focus on correctness before adding bonus features
````
