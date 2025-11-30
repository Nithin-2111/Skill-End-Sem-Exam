# Database Tables Summary

This document provides a quick reference for all database tables in the Student Learning Outcomes Tracking System.

## Table List

### 1. **users**
**Purpose:** Stores all users (teachers and students)

**Key Columns:**
- `id` - Primary key
- `username` - Unique username
- `email` - Unique email
- `password` - Hashed password (bcrypt)
- `role` - Either 'teacher' or 'student'
- `first_name`, `last_name` - User's name

**Relationships:**
- One teacher can create many courses
- One student can enroll in many courses
- One teacher can grade many assessments

---

### 2. **courses**
**Purpose:** Stores course/subject information

**Key Columns:**
- `id` - Primary key
- `name` - Course name (e.g., "Introduction to Computer Science")
- `code` - Unique course code (e.g., "CS101")
- `description` - Course description
- `teacher_id` - Foreign key to users.id (the teacher who created it)

**Relationships:**
- Belongs to one teacher (users)
- Has many enrollments
- Has many assessments
- Has many learning outcomes

---

### 3. **course_enrollments**
**Purpose:** Junction table linking students to courses (Many-to-Many)

**Key Columns:**
- `id` - Primary key
- `student_id` - Foreign key to users.id
- `course_id` - Foreign key to courses.id
- `enrolled_at` - Timestamp of enrollment

**Unique Constraint:**
- One student can only enroll once per course (unique combination of student_id and course_id)

**Relationships:**
- Links students to courses
- Enables many-to-many relationship between users and courses

---

### 4. **learning_outcomes**
**Purpose:** Stores learning objectives/outcomes for each course

**Key Columns:**
- `id` - Primary key
- `course_id` - Foreign key to courses.id
- `title` - Outcome title
- `description` - Detailed description of the outcome

**Relationships:**
- Belongs to one course
- Can be linked to many assessments (via assessment_outcomes)

---

### 5. **assessments**
**Purpose:** Stores all assessments (quizzes, exams, assignments, etc.)

**Key Columns:**
- `id` - Primary key
- `course_id` - Foreign key to courses.id
- `title` - Assessment title
- `description` - Assessment description
- `assessment_type` - Type: 'quiz', 'assignment', 'exam', 'project', 'participation'
- `max_score` - Maximum possible score
- `due_date` - Optional due date
- `created_by` - Foreign key to users.id (teacher who created it)

**Relationships:**
- Belongs to one course
- Created by one teacher
- Has many student_assessments (grades)
- Can be linked to many learning outcomes (via assessment_outcomes)

---

### 6. **assessment_outcomes**
**Purpose:** Junction table linking assessments to learning outcomes (Many-to-Many)

**Key Columns:**
- `id` - Primary key
- `assessment_id` - Foreign key to assessments.id
- `learning_outcome_id` - Foreign key to learning_outcomes.id
- `weight` - Weight of this outcome in the assessment (default 1.00)

**Unique Constraint:**
- One assessment can only link to each outcome once

**Relationships:**
- Links assessments to learning outcomes
- Enables tracking which outcomes are evaluated in each assessment

---

### 7. **student_assessments**
**Purpose:** Stores student grades/results for assessments

**Key Columns:**
- `id` - Primary key
- `student_id` - Foreign key to users.id
- `assessment_id` - Foreign key to assessments.id
- `score` - Student's score
- `max_score` - Maximum possible score (copied from assessment)
- `percentage` - Calculated percentage (score/max_score * 100)
- `feedback` - Teacher's feedback
- `submitted_at` - When student submitted (optional)
- `graded_at` - When teacher graded
- `graded_by` - Foreign key to users.id (teacher who graded)

**Unique Constraint:**
- One student can only have one grade per assessment

**Relationships:**
- Belongs to one student
- Belongs to one assessment
- Graded by one teacher (optional)

---

## Entity Relationship Diagram (Text)

```
users (teachers)
  └─── courses (1:N)
         ├─── course_enrollments (N:M with students)
         ├─── learning_outcomes (1:N)
         └─── assessments (1:N)
                ├─── assessment_outcomes (N:M with learning_outcomes)
                └─── student_assessments (1:N)
                       └─── users (students) (N:1)
```

## SQL to Create All Tables

Run the `server/schema.sql` file to create all tables with proper relationships and constraints.

## Sample Queries

### Get all courses for a teacher:
```sql
SELECT * FROM courses WHERE teacher_id = ?;
```

### Get all students enrolled in a course:
```sql
SELECT u.* FROM users u
JOIN course_enrollments ce ON u.id = ce.student_id
WHERE ce.course_id = ?;
```

### Get all grades for a student:
```sql
SELECT sa.*, a.title, a.assessment_type
FROM student_assessments sa
JOIN assessments a ON sa.assessment_id = a.id
WHERE sa.student_id = ?;
```

### Get course average for all students:
```sql
SELECT 
    u.id,
    u.first_name,
    u.last_name,
    AVG(sa.percentage) as average_percentage
FROM users u
JOIN course_enrollments ce ON u.id = ce.student_id
LEFT JOIN assessments a ON a.course_id = ce.course_id
LEFT JOIN student_assessments sa ON sa.assessment_id = a.id AND sa.student_id = u.id
WHERE ce.course_id = ?
GROUP BY u.id;
```

## Notes

- All foreign keys have appropriate CASCADE or SET NULL actions
- Timestamps are automatically managed
- Unique constraints prevent duplicate enrollments and grades
- Indexes are created on frequently queried columns (foreign keys)

