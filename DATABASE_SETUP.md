# Database Setup Guide

## Database Tables

This document describes all the database tables required for the Student Learning Outcomes Tracking System.

### 1. **users**
Stores information about teachers and students.

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| username | VARCHAR(100) | Unique username |
| email | VARCHAR(255) | Unique email address |
| password | VARCHAR(255) | Hashed password |
| role | ENUM('teacher', 'student') | User role |
| first_name | VARCHAR(100) | First name |
| last_name | VARCHAR(100) | Last name |
| created_at | TIMESTAMP | Account creation date |
| updated_at | TIMESTAMP | Last update date |

### 2. **courses**
Stores course/subject information.

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| name | VARCHAR(255) | Course name |
| code | VARCHAR(50) | Unique course code (e.g., CS101) |
| description | TEXT | Course description |
| teacher_id | INT (FK) | Reference to users.id (teacher) |
| created_at | TIMESTAMP | Course creation date |

### 3. **course_enrollments**
Junction table for student-course relationships (Many-to-Many).

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| student_id | INT (FK) | Reference to users.id (student) |
| course_id | INT (FK) | Reference to courses.id |
| enrolled_at | TIMESTAMP | Enrollment date |

### 4. **learning_outcomes**
Stores learning objectives/outcomes for each course.

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| course_id | INT (FK) | Reference to courses.id |
| title | VARCHAR(255) | Outcome title |
| description | TEXT | Detailed description |
| created_at | TIMESTAMP | Creation date |

### 5. **assessments**
Stores assessment information (quizzes, exams, assignments, etc.).

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| course_id | INT (FK) | Reference to courses.id |
| title | VARCHAR(255) | Assessment title |
| description | TEXT | Assessment description |
| assessment_type | ENUM | Type: quiz, assignment, exam, project, participation |
| max_score | DECIMAL(10,2) | Maximum possible score |
| due_date | DATE | Due date (optional) |
| created_by | INT (FK) | Reference to users.id (teacher) |
| created_at | TIMESTAMP | Creation date |
| updated_at | TIMESTAMP | Last update date |

### 6. **assessment_outcomes**
Junction table linking assessments to learning outcomes (Many-to-Many).

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| assessment_id | INT (FK) | Reference to assessments.id |
| learning_outcome_id | INT (FK) | Reference to learning_outcomes.id |
| weight | DECIMAL(5,2) | Weight of this outcome in assessment (default 1.00) |

### 7. **student_assessments**
Stores student grades/results for assessments.

| Column | Type | Description |
|--------|------|-------------|
| id | INT (PK, AUTO_INCREMENT) | Unique identifier |
| student_id | INT (FK) | Reference to users.id (student) |
| assessment_id | INT (FK) | Reference to assessments.id |
| score | DECIMAL(10,2) | Student's score |
| max_score | DECIMAL(10,2) | Maximum possible score |
| percentage | DECIMAL(5,2) | Calculated percentage |
| feedback | TEXT | Teacher feedback |
| submitted_at | TIMESTAMP | When student submitted (if applicable) |
| graded_at | TIMESTAMP | When teacher graded |
| graded_by | INT (FK) | Reference to users.id (teacher) |
| created_at | TIMESTAMP | Record creation date |
| updated_at | TIMESTAMP | Last update date |

## Setup Instructions

1. **Create the database:**
   ```sql
   CREATE DATABASE student_learning_tracker;
   ```

2. **Update your `.env` file in the server directory:**
   ```env
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=your_password
   DB_NAME=student_learning_tracker
   PORT=4000
   ```

3. **Run the schema:**
   ```bash
   mysql -u root -p student_learning_tracker < server/schema.sql
   ```
   Or import the `server/schema.sql` file using your MySQL client.

## Relationships

- **users** → **courses** (One-to-Many: One teacher can have many courses)
- **users** ↔ **courses** (Many-to-Many: Students enroll in courses via `course_enrollments`)
- **courses** → **learning_outcomes** (One-to-Many: One course has many outcomes)
- **courses** → **assessments** (One-to-Many: One course has many assessments)
- **assessments** ↔ **learning_outcomes** (Many-to-Many: via `assessment_outcomes`)
- **users** → **student_assessments** (One-to-Many: One student has many assessment results)
- **assessments** → **student_assessments** (One-to-Many: One assessment has many student results)

## Sample Data

The schema includes sample data for testing. You can create test accounts by registering through the application or by inserting directly into the database.

