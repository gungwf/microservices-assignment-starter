# 📊 Microservices System - Analysis and Design

This document outlines the **analysis** and **design** process for your microservices-based system assignment. Use it to explain your thinking and architecture decisions.

---

## 1. 🎯 Problem Statement

The system is an online quiz platform that enables students to participate in multiple-choice exams. It authenticates students, delivers questions, collects answers, enforces time limits, grades submissions, and stores results.

- **Users**:
  - **Students**: users who take quizzes  
  - **administrators**: manage quiz content and results
- **Main Goals**:
  - Authenticate students securely using their name or student ID.
  - Deliver quiz questions and collect answers within a specified time limit.
  - Grade answers accurately and store results for future reference.
  - Ensure scalability and independent deployment of system components.
- **Data Processed**:
  - Student information (name, student ID, authentication credentials).
  - Quiz content (questions, answer options, correct answers).
  - Student responses and quiz results (scores, submission timestamps).

## 2. 🧩 Identified Microservices

The system is divided into microservices, each with specific responsibilities and technology stacks.

| Service Name | Responsibility | Tech Stack |
| --- | --- | --- |
| Auth Service | Handles student authentication and authorization | Python Flask, JWT |
| Quiz Service | Manages quiz content (questions, answers) and delivers questions to students | Python Flask, SQL |
| Submission Service | Collects student answers, enforces time limits, grades submissions, and stores results | Python Flask, SQL |
| Gateway | Routes incoming requests to appropriate services | Nginx, Flask |

---

## 3. 🔄 Service Communication

Services communicate primarily via REST APIs for simplicity and compatibility. Asynchronous communication is used for non-critical operations, such as logging results.

- **Gateway ⇄ Auth Service**: REST API for student authentication (e.g., POST /auth/login).
- **Gateway ⇄ Quiz Service**: REST API for retrieving quiz questions (e.g., GET /quizzes/{quiz_id}/questions).
- **Gateway ⇄ Submission Service**: REST API for submitting answers and retrieving results (e.g., POST /submissions).
- **Internal Communication**:
  - **Quiz Service ⇄ Submission Service**: REST API to fetch correct answers for grading (e.g., GET /questions/{question_id}/answer).
  - **Submission Service → Message Queue (optional)**: Uses RabbitMQ for asynchronous logging of results to ensure reliability.

---

## 4. 🗂️ Data Design

Each service manages its own database to ensure loose coupling and independent scaling

- **Auth Service**:
  - **Database**: PostgreSQL
  - **Schema**:
    - `students(id, student_id, name, password_hash, role)`
  - Stores student credentials and roles for authentication and access control.
- **Quiz Service**:
  - **Database**: PostgreSQL
  - **Schema**:
    - `quizzes(id, title, duration, start_time, end_time)`
    - `questions(id, quiz_id, text, options, correct_answer)`
  - Manages quiz metadata and question content.
- **Submission Service**:
  - **Database**: PostgreSQL
  - **Schema**:
    - `submissions(id, student_id, quiz_id, submission_time, score)`
    - `answers(id, submission_id, question_id, student_answer)`
  - Stores student answers and final scores.

---

## 5. 🔐 Security Considerations

- Use JWT for user sessions
- Validate input on each service
- Role-based access control for APIs

---


## 6. 📦 Deployment Plan

- Use `docker-compose` to manage local environment
- Each service has its own Dockerfile
- Environment config stored in `.env` file

---

## 7. 🎨 Architecture Diagram

> *(You can add an image or ASCII diagram below)*

```
+---------+        +--------------+
| Gateway | <----> | Service A    |
|         | <----> | Auth Service |
+---------+        +--------------+
       |                ^
       v                |
+--------------+   +------------------+
| Service B    |   | Database / Redis |
| Course Mgmt  |   +------------------+
+--------------+
```

---

## ✅ Summary

Summarize why this architecture is suitable for your use case, how it scales, and how it supports independent development and deployment.



## Author

This template was created by Hung Dang.
- Email: hungdn@ptit.edu.vn
- GitHub: hungdn1701


Good luck! 💪🚀
