# Student Learning Outcomes Tracking System

A comprehensive web application for tracking, monitoring, and analyzing student learning outcomes. Teachers can record assessments, evaluate performance, and generate reports, while students can view their progress, understand their strengths and weaknesses, and identify areas for improvement.

## Features

### For Teachers
- ✅ Create and manage courses
- ✅ Create assessments (quizzes, assignments, exams, projects, participation)
- ✅ Grade student assessments with feedback
- ✅ View detailed course performance reports
- ✅ Track student enrollment
- ✅ Monitor learning outcomes progress

### For Students
- ✅ View enrolled courses
- ✅ Track assessment results and grades
- ✅ View detailed performance analytics
- ✅ See learning outcomes progress
- ✅ Identify strengths and areas for improvement
- ✅ Receive feedback from teachers

## Tech Stack

### Frontend
- React 18
- React Router DOM
- Axios
- React Icons
- Vite

### Backend
- Node.js
- Express.js
- MySQL2
- Bcryptjs (password hashing)
- CORS

## Database Setup

### Prerequisites
- MySQL Server installed and running
- Node.js (v14 or higher)
- npm or yarn

### Installation Steps

1. **Create the database:**
   ```sql
   CREATE DATABASE student_learning_tracker;
   ```

2. **Configure environment variables:**
   
   Create a `.env` file in the `server` directory:
   ```env
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=your_password_here
   DB_NAME=student_learning_tracker
   PORT=4000
   ```

3. **Run the schema:**
   ```bash
   # Option 1: Using MySQL command line
   mysql -u root -p student_learning_tracker < server/schema.sql
   
   # Option 2: Import using MySQL Workbench or phpMyAdmin
   # Open server/schema.sql and execute it in your MySQL client
   ```

## Project Setup

### Backend Setup

1. Navigate to the server directory:
   ```bash
   cd server
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the server:
   ```bash
   # Development mode (with auto-reload)
   npm run dev
   
   # Production mode
   npm start
   ```

   The server will run on `http://localhost:4000`

### Frontend Setup

1. Navigate to the client directory:
   ```bash
   cd client
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file (optional, defaults shown):
   ```env
   VITE_API_BASE=http://localhost:4000/api
   ```

4. Start the development server:
   ```bash
   npm run dev
   ```

   The client will run on `http://localhost:5173` (or another port if 5173 is busy)

## Database Tables

The system uses the following database tables (see `DATABASE_SETUP.md` for detailed schema):

1. **users** - Stores teachers and students
2. **courses** - Course/subject information
3. **course_enrollments** - Student-course relationships
4. **learning_outcomes** - Learning objectives for courses
5. **assessments** - Assessments (quizzes, exams, assignments, etc.)
6. **assessment_outcomes** - Links assessments to learning outcomes
7. **student_assessments** - Student grades and feedback

See `DATABASE_SETUP.md` for complete table structure and relationships.

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user

### Courses
- `GET /api/courses` - Get all courses (with optional `teacher_id` or `student_id` query)
- `GET /api/courses/:id` - Get course details
- `POST /api/courses` - Create new course
- `PUT /api/courses/:id` - Update course
- `DELETE /api/courses/:id` - Delete course
- `POST /api/courses/:id/enroll` - Enroll student in course
- `DELETE /api/courses/:id/enroll/:student_id` - Unenroll student

### Assessments
- `GET /api/assessments` - Get all assessments (with optional `course_id` or `student_id` query)
- `GET /api/assessments/:id` - Get assessment details
- `POST /api/assessments` - Create new assessment
- `PUT /api/assessments/:id` - Update assessment
- `DELETE /api/assessments/:id` - Delete assessment
- `POST /api/assessments/:id/grade` - Grade student assessment
- `GET /api/assessments/:id/grades` - Get all grades for an assessment

### Students
- `GET /api/students` - Get all students
- `GET /api/students/:id` - Get student with progress
- `GET /api/students/:id/courses/:course_id` - Get student progress for a course

### Reports
- `GET /api/reports/course/:course_id` - Get course performance report
- `GET /api/reports/student/:student_id` - Get student performance report

### Learning Outcomes
- `GET /api/outcomes/course/:course_id` - Get learning outcomes for a course
- `POST /api/outcomes` - Create learning outcome
- `PUT /api/outcomes/:id` - Update learning outcome
- `DELETE /api/outcomes/:id` - Delete learning outcome

## Usage

### For Teachers

1. **Register/Login:**
   - Register as a teacher or login with existing credentials
   - You'll be redirected to the Teacher Dashboard

2. **Create a Course:**
   - Click "New Course" button
   - Fill in course name, code, and description
   - Save the course

3. **Create Assessments:**
   - Navigate to the "Assessments" tab
   - Click "New Assessment"
   - Select course, enter details, and set max score
   - Save the assessment

4. **Grade Students:**
   - Click the grade icon (✓) next to an assessment
   - Enter scores and feedback for each student
   - Click "Save" for each student

5. **View Reports:**
   - Navigate to the "Reports" tab
   - Select a course to view detailed performance reports

### For Students

1. **Register/Login:**
   - Register as a student or login with existing credentials
   - You'll be redirected to the Student Dashboard

2. **View Progress:**
   - See overall statistics and enrolled courses
   - Click "View Progress" on any course to see detailed assessment results
   - View learning outcomes progress

## Development

### Project Structure

```
task-tracker-crud/
├── client/                 # React frontend
│   ├── src/
│   │   ├── components/    # Reusable components
│   │   ├── context/       # React context (Auth)
│   │   ├── pages/         # Page components
│   │   └── api.js         # API client
│   └── package.json
├── server/                 # Express backend
│   ├── routes/            # API route handlers
│   ├── db.js              # Database connection
│   ├── schema.sql         # Database schema
│   └── package.json
└── README.md
```

## Notes

- Passwords are hashed using bcryptjs
- All API endpoints return JSON
- CORS is enabled for development
- Authentication is handled via localStorage (client-side)
- The system uses MySQL for data persistence

## Troubleshooting

### Database Connection Issues
- Verify MySQL is running
- Check `.env` file credentials
- Ensure database exists: `student_learning_tracker`

### Port Already in Use
- Change PORT in server `.env` file
- Update `VITE_API_BASE` in client `.env` if needed

### Module Not Found
- Run `npm install` in both client and server directories

## License

This project is open source and available for educational purposes.
# student_learning_app
# student-learning-app-
"# student-learning-app-" 
# student-learning-app-
