# Chapter 7: Hands-On Examples

This chapter demonstrates real-world OpenSpec usage through complete examples.

---

## Example 1: Todo App (Web)

### Project Background
Build a Todo App from scratch with task CRUD, categories, and priority management.

### First Change: Basic CRUD

```bash
/opsx:propose "Create the basic task management feature for a Todo App, supporting create, read, update, and delete tasks"
```

**Generated spec documents**:
- proposal.md: Project background, goals, scope
- specs/requirements.md: Data structure, functional requirements, scenarios
- design.md: Tech stack (React + Node.js), API design, database schema
- tasks.md: Project initialization, database, backend API, frontend pages, testing

### Follow-up Changes
```bash
/opsx:propose "Add task categories to the Todo App"
/opsx:propose "Add priority levels (high/medium/low) to tasks, sort task list by priority"
```

---

## Example 2: Student Grade Management System (C++ / Qt)

### Project Background
Develop a desktop application using C++ and Qt for managing student information and grades. Supports adding students, entering grades, querying statistics, data export, and more.

### Tech Stack
- **Language**: C++17
- **GUI Framework**: Qt 6
- **Database**: SQLite
- **Build Tool**: CMake

### First Change: Project Framework and Basic Data Models

```bash
/opsx:propose "Create the project framework for a Student Grade Management System, including Qt project structure, CMake configuration, basic data models (Student, Course, Score)"
```

**Generated proposal.md**:
```markdown
# Proposal: Student Grade Management System - Project Framework

## Background
A desktop application is needed to manage student grades, replacing the current Excel-based approach.

## Goals
- Set up C++/Qt project framework
- Design basic data models
- Implement database access layer

## Scope
**Included**:
- Qt project structure and CMake configuration
- Student, Course, Score data models
- SQLite database and DAO layer
- Basic main window UI framework

**Not included**:
- Specific feature UIs (future iterations)
- Data import/export (future iterations)
- Report statistics (future iterations)

## Success Criteria
- Project compiles and runs
- Database tables created successfully
- Basic data CRUD works
```

**Generated design.md**:
```markdown
# Technical Design

## Project Structure
```
StudentManager/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── mainwindow.cpp/h/ui
│   ├── models/
│   │   ├── student.h/cpp
│   │   ├── course.h/cpp
│   │   └── score.h/cpp
│   ├── database/
│   │   ├── database.h/cpp
│   │   ├── studentdao.h/cpp
│   │   ├── coursedao.h/cpp
│   │   └── scoredao.h/cpp
│   └── utils/
│       └── databaseconfig.h
└── resources/
    └── icons/
```

## Data Models

### Student
```cpp
class Student {
    int id;
    QString studentId;      // Student ID, unique
    QString name;           // Name
    QString gender;         // Gender
    QDate birthDate;        // Date of birth
    QString className;      // Class
    QString phone;          // Phone number
    QDateTime createdAt;
};
```

### Course
```cpp
class Course {
    int id;
    QString courseId;       // Course number
    QString name;           // Course name
    int credit;             // Credits
    QString semester;       // Semester (e.g., 2024-2025-1)
    QDateTime createdAt;
};
```

### Score
```cpp
class Score {
    int id;
    int studentId;          // Associated student
    int courseId;           // Associated course
    double score;           // Score (0-100)
    QString examType;       // Exam type (coursework/midterm/final)
    QDate examDate;         // Exam date
    QString remark;         // Notes
    QDateTime createdAt;
};
```

## Database Design

### students table
```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    student_id TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    gender TEXT,
    birth_date DATE,
    class_name TEXT,
    phone TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### courses table
```sql
CREATE TABLE courses (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    course_id TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    credit INTEGER DEFAULT 0,
    semester TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### scores table
```sql
CREATE TABLE scores (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    student_id INTEGER NOT NULL,
    course_id INTEGER NOT NULL,
    score REAL CHECK(score >= 0 AND score <= 100),
    exam_type TEXT,
    exam_date DATE,
    remark TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

## Qt UI Design

### Main Window Layout
- Left: Navigation panel (Student Management, Course Management, Grade Management, Statistics & Reports)
- Right: Content area (switches based on navigation)
- Top: Toolbar (Add, Delete, Import, Export)
- Bottom: Status bar

### Technical Highlights
- Use QStackedWidget to manage multiple pages
- Use QTableView + QSqlTableModel to display data
- Use QDialog for add/edit operations
```

**Generated tasks.md**:
```markdown
# Implementation Tasks

## 1. Project Initialization
- [ ] 1.1 Create project directory structure
- [ ] 1.2 Write CMakeLists.txt (Qt6 + SQLite)
- [ ] 1.3 Configure Qt resource file (.qrc)

## 2. Data Model Layer
- [ ] 2.1 Create Student class (header and implementation)
- [ ] 2.2 Create Course class (header and implementation)
- [ ] 2.3 Create Score class (header and implementation)
- [ ] 2.4 Implement model serialization/deserialization

## 3. Database Access Layer (DAO)
- [ ] 3.1 Create Database class (connection management)
- [ ] 3.2 Create StudentDao class (CRUD)
- [ ] 3.3 Create CourseDao class (CRUD)
- [ ] 3.4 Create ScoreDao class (CRUD)
- [ ] 3.5 Implement database initialization (create tables)

## 4. Basic UI
- [ ] 4.1 Create MainWindow
- [ ] 4.2 Design main window layout (navigation + content area)
- [ ] 4.3 Create left navigation panel
- [ ] 4.4 Create status bar

## 5. Testing
- [ ] 5.1 Test database connection
- [ ] 5.2 Test DAO layer CRUD
- [ ] 5.3 Test project compilation and execution
```

### Second Change: Student Management Module

```bash
/opsx:propose "Implement the student management module, including student list display, add student, edit student info, delete student, and search by criteria"
```

**Key design points**:
- Use QTableView to display the student list
- Use QSortFilterProxyModel for sorting and filtering
- Use QDialog for add/edit dialogs
- Support search by name, student ID, and class

### Third Change: Grade Entry and Queries

```bash
/opsx:propose "Implement the grade management module, supporting grade entry, grade queries, grade statistics (average, highest, lowest), and grade export"
```

**Key design points**:
- Auto-calculate total score during entry (coursework 30% + midterm 30% + final 40%)
- Use QChart to plot grade distribution
- Support querying grades by student or course
- Export to Excel or CSV format

### Fourth Change: Data Import/Export

```bash
/opsx:propose "Implement data import/export, supporting import of student and grade data from Excel/CSV, and export of grade reports"
```

---

## Example 3: Blog System (Full Stack)

### Project Background
Build a complete blog system with user authentication, article management, comment system, tags, and categories.

### Tech Stack
- **Frontend**: Next.js + TypeScript + Tailwind CSS
- **Backend**: Node.js + Express + Prisma
- **Database**: PostgreSQL
- **Deployment**: Vercel + Railway

### Change List
```bash
/opsx:propose "Implement user authentication system with registration, login, and JWT Token"
/opsx:propose "Implement article management with create, edit, publish, and delete"
/opsx:propose "Implement comment system with article comments and replies"
/opsx:propose "Implement tags and categories"
/opsx:propose "Implement article search and pagination"
```

---

## Viewing Project Evolution

After completing multiple changes, the archive/ directory records the full history:

```
openspec/archive/
├── 2025-03-16-todo-basic-crud/
├── 2025-03-16-qt-student-framework/
├── 2025-03-17-qt-student-management/
├── 2025-03-17-qt-score-module/
├── 2025-03-18-todo-categories/
└── 2025-03-18-blog-auth/
```

Use `/opsx:onboard` to generate a project overview that helps new members quickly understand the project.

---

## Next

→ [Chapter 8: Team Collaboration](08-teamwork-en.md)
