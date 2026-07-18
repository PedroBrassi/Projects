# Academic Management System (Java, OOP)

**Course:** Object-Oriented Programming (POO) — UNIFAL-MG
**Language:** Java 21
**Build tool:** Maven

## Overview

A layered academic management system built for the OOP course, covering 12
user stories (US-2361 through US-2372) related to managing classes,
professors, students, assessments (exams, assignments, seminars), reports,
authentication and authorization. The project was merged with a classmate's
implementation into a single, unified codebase.

It includes both a **JavaFX GUI** and a console-based flow, backed by a
pluggable persistence layer (TXT, XML or JSON).

## Architecture

The codebase follows a layered architecture:

```
src/main/java/org/example/academic/system/
├── model/          # Domain entities (AcademicClass, Professor, Assessment, Exam, ...)
├── service/         # Business logic (ClassService, AssessmentService, PersistenceService, ...)
├── repository/      # Persistence layer — In-memory, TXT, JSON and XML implementations
├── gui/              # JavaFX controllers and screens (login, class registration, reports, ...)
├── logging/          # Application, security and audit logging
├── validation/       # Domain validation rules
├── exception/         # Custom exception hierarchy
└── report/            # Report generation
```

Key design points:

- **Repository pattern** with interchangeable persistence backends
  (`TxtAcademicClassRepository`, `JsonAcademicClassRepository`,
  `XmlAcademicClassRepository`, `InMemoryAcademicClassRepository`)
- **Service layer** separating business rules from persistence and UI
- **Custom exceptions** for domain, authentication and authorization errors
- **Audit logging** for security-sensitive actions (`SecurityAuditLogger`,
  `PersistenceAuditLogger`, `ReportAuditLogger`)
- **Bean Validation (Jakarta/Hibernate Validator)** for domain object
  validation
- **Lombok** to reduce boilerplate
- Test suite covering authentication, authorization, persistence, services
  and report generation (JUnit)

## Technologies

- Java 21
- JavaFX (GUI)
- Maven
- Lombok
- Jakarta Bean Validation + Hibernate Validator
- SLF4J + Logback
- JUnit 5
- Docker (a `Dockerfile` is included for containerized execution)

## Build & Run

```bash
mvn clean package
```

Run the generated JAR, or launch the JavaFX GUI entry point
(`gui/JavaFXMain.java`), depending on which persistence backend is
configured.

## Author

Pedro Brassi Luccas — Computer Science, UNIFAL-MG
(developed together with a classmate, as a merged joint submission)
