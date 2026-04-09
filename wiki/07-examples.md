> [<< 第六章：文档编写规范](06-documentation.md) | [目录](Home.md) | [第八章：团队协作 >>](08-teamwork.md)

---

# 第七章：实战案例

本章通过完整的案例演示 OpenSpec 的实际使用。

---

## 案例一：Todo App（Web）

### 项目背景
从零开始构建一个 Todo App，包含任务的增删改查、分类和优先级管理。

### 第一个变更：基础 CRUD

```bash
/opsx:propose "创建 Todo App 的基础任务管理功能，支持创建、查看、更新、删除任务"
```

**生成的规范文档**：
- proposal.md：项目背景、目标、范围
- specs/requirements.md：数据结构、功能需求、场景
- design.md：技术栈（React + Node.js）、API 设计、数据库 Schema
- tasks.md：项目初始化、数据库、后端 API、前端页面、测试

### 后续变更
```bash
/opsx:propose "为 Todo App 添加任务分类功能"
/opsx:propose "为任务添加优先级（高/中/低），任务列表按优先级排序"
```

---

## 案例二：学生成绩管理系统（C++ / Qt）

### 项目背景
使用 C++ 和 Qt 开发一个桌面应用，用于管理学生信息和成绩。支持添加学生、录入成绩、查询统计、数据导出等功能。

### 技术栈
- **语言**：C++17
- **GUI 框架**：Qt 6
- **数据库**：SQLite
- **构建工具**：CMake

### 第一个变更：项目框架和基础数据模型

```bash
/opsx:propose "创建学生成绩管理系统的项目框架，包括 Qt 项目结构、CMake 配置、基础数据模型（Student、Course、Score）"
```

**生成的 proposal.md**：
```markdown
# 提案：学生成绩管理系统 - 项目框架

## 背景
需要开发一个桌面应用管理学生成绩，替代现有的 Excel 表格管理方式。

## 目标
- 建立 C++/Qt 项目框架
- 设计基础数据模型
- 实现数据库访问层

## 范围
**包含**：
- Qt 项目结构和 CMake 配置
- Student、Course、Score 数据模型
- SQLite 数据库和 DAO 层
- 基础的主窗口界面框架

**不包含**：
- 具体的功能界面（后续迭代）
- 数据导入导出（后续迭代）
- 报表统计功能（后续迭代）

## 成功标准
- 项目可以编译运行
- 数据库表创建成功
- 可以增删改查基础数据
```

**生成的 design.md**：
```markdown
# 技术设计

## 项目结构
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

## 数据模型

### Student（学生）
```cpp
class Student {
    int id;
    QString studentId;      // 学号，唯一
    QString name;           // 姓名
    QString gender;         // 性别
    QDate birthDate;        // 出生日期
    QString className;      // 班级
    QString phone;          // 联系电话
    QDateTime createdAt;
};
```

### Course（课程）
```cpp
class Course {
    int id;
    QString courseId;       // 课程编号
    QString name;           // 课程名称
    int credit;             // 学分
    QString semester;       // 学期（如：2024-2025-1）
    QDateTime createdAt;
};
```

### Score（成绩）
```cpp
class Score {
    int id;
    int studentId;          // 关联学生
    int courseId;           // 关联课程
    double score;           // 成绩（0-100）
    QString examType;       // 考试类型（平时/期中/期末）
    QDate examDate;         // 考试日期
    QString remark;         // 备注
    QDateTime createdAt;
};
```

## 数据库设计

### students 表
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

### courses 表
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

### scores 表
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

## Qt 界面设计

### 主窗口布局
- 左侧：导航栏（学生管理、课程管理、成绩管理、统计报表）
- 右侧：内容区域（根据导航切换）
- 顶部：工具栏（添加、删除、导入、导出）
- 底部：状态栏

### 技术要点
- 使用 QStackedWidget 管理多个页面
- 使用 QTableView + QSqlTableModel 显示数据
- 使用 QDialog 处理添加/编辑操作
```

**生成的 tasks.md**：
```markdown
# 实现任务

## 1. 项目初始化
- [ ] 1.1 创建项目目录结构
- [ ] 1.2 编写 CMakeLists.txt（Qt6 + SQLite）
- [ ] 1.3 配置 Qt 资源文件（.qrc）

## 2. 数据模型层
- [ ] 2.1 创建 Student 类（头文件和实现）
- [ ] 2.2 创建 Course 类（头文件和实现）
- [ ] 2.3 创建 Score 类（头文件和实现）
- [ ] 2.4 实现模型的序列化/反序列化

## 3. 数据库访问层（DAO）
- [ ] 3.1 创建 Database 类（连接管理）
- [ ] 3.2 创建 StudentDao 类（CRUD）
- [ ] 3.3 创建 CourseDao 类（CRUD）
- [ ] 3.4 创建 ScoreDao 类（CRUD）
- [ ] 3.5 实现数据库初始化（创建表）

## 4. 基础界面
- [ ] 4.1 创建 MainWindow 主窗口
- [ ] 4.2 设计主窗口布局（导航栏 + 内容区）
- [ ] 4.3 创建左侧导航栏
- [ ] 4.4 创建状态栏

## 5. 测试
- [ ] 5.1 测试数据库连接
- [ ] 5.2 测试 DAO 层 CRUD
- [ ] 5.3 测试项目编译运行
```

### 第二个变更：学生管理模块

```bash
/opsx:propose "实现学生管理模块，包括学生列表展示、添加学生、编辑学生信息、删除学生、按条件搜索学生"
```

**关键设计要点**：
- 使用 QTableView 展示学生列表
- 使用 QSortFilterProxyModel 实现排序和筛选
- 使用 QDialog 实现添加/编辑对话框
- 支持按姓名、学号、班级搜索

### 第三个变更：成绩录入和查询

```bash
/opsx:propose "实现成绩管理模块，支持成绩录入、成绩查询、成绩统计（平均分、最高分、最低分）、成绩导出"
```

**关键设计要点**：
- 成绩录入时自动计算总分（平时30% + 期中30% + 期末40%）
- 使用 QChart 绘制成绩分布图
- 支持按学生或课程查询成绩
- 导出为 Excel 或 CSV 格式

### 第四个变更：数据导入导出

```bash
/opsx:propose "实现数据导入导出功能，支持从 Excel/CSV 导入学生和成绩数据，导出成绩报表"
```

---

## 案例三：Blog System（全栈）

### 项目背景
构建一个完整的博客系统，包含用户认证、文章管理、评论系统、标签分类等功能。

### 技术栈
- **前端**：Next.js + TypeScript + Tailwind CSS
- **后端**：Node.js + Express + Prisma
- **数据库**：PostgreSQL
- **部署**：Vercel + Railway

### 变更列表
```bash
/opsx:propose "实现用户认证系统，支持注册、登录、JWT Token"
/opsx:propose "实现文章管理，支持创建、编辑、发布、删除文章"
/opsx:propose "实现评论系统，支持文章评论和回复"
/opsx:propose "实现标签和分类功能"
/opsx:propose "实现文章搜索和分页"
```

---

## 查看项目演进

完成多个变更后，archive/ 目录记录了完整历史：

```
openspec/archive/
├── 2025-03-16-todo-basic-crud/
├── 2025-03-16-qt-student-framework/
├── 2025-03-17-qt-student-management/
├── 2025-03-17-qt-score-module/
├── 2025-03-18-todo-categories/
└── 2025-03-18-blog-auth/
```

使用 `/opsx:onboard` 可以生成项目概览，帮助新成员快速了解项目。

---

> [<< 第六章：文档编写规范](06-documentation.md) | [目录](Home.md) | [第八章：团队协作 >>](08-teamwork.md)