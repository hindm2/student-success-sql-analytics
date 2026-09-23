# Student Success Analytics | PostgreSQL

## Project Overview

This project explores student academic performance and engagement using the Open University Learning Analytics Dataset (OULAD).

I designed and implemented a relational database in PostgreSQL to organize student demographics, course registrations, assessments, and virtual learning environment (VLE) activity.

Using SQL, I analyze student performance, assessment scores, course outcomes, and engagement patterns to uncover insights into student success.

## Project Objectives

* Design and implement a relational database using PostgreSQL.
* Import, clean, and validate large educational datasets.
* Analyze student performance and engagement using advanced SQL.
* Apply CTEs, subqueries, window functions, and aggregations to answer analytical questions.
* Demonstrate practical data modeling and data quality techniques.

## Technologies Used

* **Database:** PostgreSQL
* **Database Management:** pgAdmin 4
* **Language:** SQL
* **Dataset:** Open University Learning Analytics Dataset (OULAD)


  ## Database Schema

The project uses a PostgreSQL relational database containing six tables from the Open University Learning Analytics Dataset (OULAD).

The database integrates course information, student demographics, assessment results, and virtual learning environment (VLE) activity to support analysis of student performance and engagement.

### Database Tables

| Table                | Description                                                                         |
| -------------------- | ----------------------------------------------------------------------------------- |
| `courses`            | Stores course information and presentation details.                                 |
| `assessments`        | Contains assessment metadata, including assessment type and weight.                 |
| `student_assessment` | Records student assessment submissions and scores.                                  |
| `student_course`     | Contains student demographics, registration information, and final course outcomes. |
| `student_vle`        | Stores student interactions with online learning resources.                         |
| `vle`                | Contains metadata describing virtual learning resources.                            |

### Data Modeling and Design

The database was designed to support efficient querying and maintain relationships between students, courses, assessments, and learning activities.

Key design considerations include:

* Using course module and presentation identifiers to distinguish course offerings.
* Establishing appropriate primary and foreign keys to maintain referential integrity.
* Addressing duplicate records during data preparation.
* Introducing a surrogate key for `student_vle` because the original activity fields did not provide a reliable unique identifier.


### Primary Keys, Foreign Keys, and Data Integrity

The database uses primary and foreign key constraints to preserve data integrity and establish relationships between course offerings, student assessments, and online learning activity.

**Key design decisions:**

* **Composite keys:** Course offerings are identified using `code_module` and `code_presentation`, allowing the same module to appear in different presentations.
* **Assessment identification:** Each assessment has a unique `id_assessment`, while student assessment records use a composite primary key.
* **Surrogate key:** A generated `student_vle_id` was introduced to identify individual student activity records uniquely.
* **Referential integrity:** Foreign key constraints connect assessments, course offerings, and VLE resources to their related tables.

These design decisions support reliable joins, reduce ambiguity, and provide a structured foundation for analyzing student performance and engagement.

* Validating data integrity after loading the source files.

The relational schema provides the foundation for advanced SQL analysis of academic performance and student engagement.


### Entity Relationship Diagram

```mermaid
erDiagram
    courses ||--o{ assessments : contains
    courses ||--o{ student_course : offers
    courses ||--o{ vle : provides
    courses ||--o{ student_vle : records
    assessments ||--o{ student_assessment : receives
    vle ||--o{ student_vle : tracks

```
## Data Cleaning and Validation

Data quality and integrity checks were performed during the preparation and loading of the Open University Learning Analytics Dataset (OULAD).

### Handling Repeated Student Activity Records

The `student_vle` table contains student interactions with virtual learning environment resources.

An examination of the original activity fields identified repeated combinations of course module, presentation, student ID, site ID, date, and click count.

Instead of automatically removing these records, I retained them and added a surrogate primary key (`student_vle_id`) to uniquely identify each row.

**Validation results:**

| Metric                                 |     Result |
| -------------------------------------- | ---------: |
| Total activity records                 | 10,655,280 |
| Unique activity combinations           |  9,868,110 |
| Repeated combinations beyond the first |    787,170 |
| Duplicate surrogate IDs                |          0 |

This approach preserves the loaded activity records while maintaining unique row identifiers. Repeated combinations were not assumed to be erroneous duplicates because the source fields do not establish whether identical records represent separate interactions.

### Handling Missing Values

Missing assessment-related values were handled during data import. The `?` placeholder in the source assessment data was interpreted as SQL `NULL`, allowing missing values to be represented appropriately.

### Referential Integrity

Primary and foreign key constraints were implemented to maintain relationships between course offerings, assessments, student records, and virtual learning environment resources.

Referential integrity checks were also performed to identify potential orphan records.



