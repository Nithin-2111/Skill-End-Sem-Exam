# MySQL Database Setup - Step by Step Guide

## Method 1: Run SQL File in MySQL Workbench (Recommended)

### Step 1: Create a New Database

1. In MySQL Workbench, make sure you're connected to your MySQL server
2. In the Navigator panel (left side), under **SCHEMAS**, you'll see your databases
3. Right-click in the **SCHEMAS** area → Click **"Create Schema"**
4. Enter database name: `student_learning_tracker`
5. Click **"Apply"** → **"Finish"**

### Step 2: Select the Database

1. In the **SCHEMAS** panel, click on `student_learning_tracker` to select it
2. Or double-click on it to make it active

### Step 3: Open SQL File

**Option A: Using File Menu**
1. Click **File** → **Open SQL Script**
2. Navigate to: `D:\FRONTEND\new_frountrn_projuct\task-tracker-crud\server\schema.sql`
3. Click **Open**

**Option B: Copy-Paste**
1. Open the `schema.sql` file in a text editor
2. Copy all the content (Ctrl+A, Ctrl+C)
3. Paste into the Query Editor in MySQL Workbench

### Step 4: Update Database Name (If Needed)

If you want to use a different database name, add this line at the very top of the SQL:

```sql
USE student_learning_tracker;
```

Or if using `crud_app`, change it to:
```sql
USE crud_app;
```

### Step 5: Execute the Script

1. Make sure the correct database is selected in the Navigator
2. In the Query Editor, you should see all the SQL commands
3. Click the **Execute** button (⚡ lightning bolt icon) in the toolbar
   - Or press **Ctrl+Shift+Enter**
   - Or click **Query** → **Execute (All or Selection)**

### Step 6: Verify Tables Created

1. In the Navigator panel, expand `student_learning_tracker` (or your database)
2. Expand **Tables**
3. You should see these tables:
   - ✅ users
   - ✅ courses
   - ✅ course_enrollments
   - ✅ learning_outcomes
   - ✅ assessments
   - ✅ assessment_outcomes
   - ✅ student_assessments

### Step 7: Check Sample Data

Run this query to verify sample users were inserted:

```sql
SELECT * FROM users;
```

You should see 3 rows (teacher1, student1, student2).

---

## Method 2: Using Command Line

If you prefer command line:

```bash
# Navigate to server directory
cd D:\FRONTEND\new_frountrn_projuct\task-tracker-crud\server

# Run the schema (replace with your MySQL credentials)
mysql -u root -p student_learning_tracker < schema.sql
```

---

## Method 3: Execute Step by Step

If you encounter errors, you can run each section separately:

### 1. First, create/select database:
```sql
CREATE DATABASE IF NOT EXISTS student_learning_tracker;
USE student_learning_tracker;
```

### 2. Then run each CREATE TABLE statement one by one:

1. **Users table** (lines 4-14)
2. **Courses table** (lines 17-26)
3. **Course enrollments** (lines 29-39)
4. **Learning outcomes** (lines 42-50)
5. **Assessments** (lines 53-68)
6. **Assessment outcomes** (lines 71-81)
7. **Student assessments** (lines 84-103)

### 3. Finally, insert sample data (lines 106-109)

---

## Troubleshooting

### Error: "Table already exists"
If you see this error, you have two options:

**Option 1: Drop existing tables** (⚠️ This will delete all data!)
```sql
USE student_learning_tracker;

DROP TABLE IF EXISTS student_assessments;
DROP TABLE IF EXISTS assessment_outcomes;
DROP TABLE IF EXISTS assessments;
DROP TABLE IF EXISTS learning_outcomes;
DROP TABLE IF EXISTS course_enrollments;
DROP TABLE IF EXISTS courses;
DROP TABLE IF EXISTS users;
```

Then run the schema.sql again.

**Option 2: The schema uses `CREATE TABLE IF NOT EXISTS`**
This means it will skip tables that already exist. This is safe but won't update existing tables.

### Error: "Foreign key constraint fails"
This usually means:
- Tables are being created in the wrong order
- Referenced tables don't exist yet
- Sample data is trying to reference non-existent records

**Solution:** Make sure to run the schema in the correct order, or run the entire file at once.

### Error: "Database doesn't exist"
Create the database first:
```sql
CREATE DATABASE student_learning_tracker;
USE student_learning_tracker;
```

---

## Quick Setup for Your Current Database (crud_app)

If you want to use your existing `crud_app` database:

1. Add this at the top of your schema.sql:
```sql
USE crud_app;
```

2. Then run the entire schema.sql file

3. All tables will be created in the `crud_app` database

---

## Update Your .env File

After creating the database, update your `server/.env` file:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password_here
DB_NAME=student_learning_tracker
PORT=4000
```

---

## Verify Everything Works

After setup, test your database connection:

1. In MySQL Workbench, run:
```sql
USE student_learning_tracker;
SHOW TABLES;
```

2. You should see all 7 tables listed

3. Check the users table:
```sql
SELECT * FROM users;
```

4. Should show 3 sample users

---

## Next Steps

After the database is set up:
1. ✅ Start your backend server: `cd server && npm run dev`
2. ✅ Start your frontend: `cd client && npm run dev`
3. ✅ Test the application!



