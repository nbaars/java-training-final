# CodeIT

## Introduction 

CodeIT is a platform where students can upload their coding solutions in a ZIP file. Teachers are able to see the solutions given by students and give grades.

## Assignments

### Domain model

Model a course, teacher, students. Each class has a teacher and a class has 1 or more students. A solution is related to a class and a student. A solution also has a grade. A course is given in a certain period.

A student has a first name lastname and email address. A teacher has a first name, lastname and email address. A course has a name and a period, a course has solutions. Each solution is linked to a course and a student. A solution has a grade.

A course has multiple assignments, an assignment has a solution for each student. A solution has a grade.

### Create a course

A teacher should be able to create a course. And assignment students to this course.

Endpoint:

```
POST /code-it/course/{course_id}
assignments: ["week1", "week2", "week3"]
```

```
POST /code-it/course/{course_id}
["John", "Mary", "Megan"]
```

We can assume that students are already present. Use an insert script to create some students.

### Upload a solution

A student should be able to upload a solution in a ZIP file, endpoint:

```
POST /code-it/solutions/{course}/{assignment/{assignment_id}
```

The solutions should be stored in the database. Tip: the name of the student can be fetched by getting the logged-in user with: `SecurityContextHolder.getContext().getAuthentication().getName()`

Tip: lookup `multipart file support` in Spring Boot it works a bit different.
A student can upload solution multiple times, each time it replaces the original uploaded solution.

### Additional endpoints

We want to expose several endpoints:

- Endpoint for a teacher to delete a student from a course (`DELETE /code-it/course/{course_id}/student/{student_id}`). This can only be done by a teacher.
- Endpoint to grade a solution, this can only be done by a teacher (`PUT /code-it/course/{course_id}/solutions/{id}/grade`). The grade is a number between 1 and 10. This can only be done by a teacher.
- Endpoint to see a solution (`/code-it/course/{course_id}/solutions/{id}`). A teacher should be able to see all solutions. The teacher wants to download the solution as a ZIP file.
- Endpoint to query the highest score per course. This can only be requested by a teacher.
- Endpoint to see all the data of all students in a course. This can only be requested by a teacher.
- Endpoint to get all grades in a `csv` file. This can only be requested by a teacher.
- Endpoint to show a historical overview of a course between different periods: `/code-it/course/{id}/historial-overview`
- Endpoint to see whether all students have submitted a solution for each assignment per  course: `/code-it/course/{id}/all-submitted`. The response is open for discussion.
- Endpoint to get a list of all students passing the course. Each solution needs to have a grade of 5 or higher. `/code-it/course/{id}/passing-students`


### Security

Requirements:

- Only a teacher can see all solutions
- Only a student can post a solution
- A student should not able to see each other solutions.
- The director should be able to see all grades of all students in a course.

### Batch jobs

Each night the solutions needs to be checked against a plagiarism API. The result should be stored with the solution.
Hint: use `@Scheduled` in your application, and use a REST client to call the API. You can mock this API of course.


See https://github.com/nbaars/java-training-library/jpa for more information about JPA.
See https://github.com/nbaars/java-training-library/jdbc for more information about JDBC.

https://start.spring.io/

