# Lab 05: API system test

- Name: <Ts. Todbileg>
- Student ID: <B232270045>
- `node -v`: <v24.11.1>
- `newman -v`: <6.2.2>

# Choices and equivalence classes

| Choice | Class | Representative value |
|---|---|---|
| studentID validity | active | student with status `active` |
| | inactive | student with status `inactive` |
| | missing | student never created |
| Courses taken by student | satisfy prerequisites | `["CS201"]` |
| | do not satisfy | `[]` |
| courseID validity | exists | course created via PUT |
| | missing | course never created |
| Course prerequisites | all taken | `["CS201"]`, student has `["CS201"]` |
| | none taken | `["CS201","CS202"]`, student has `[]` |
| | some taken | `["CS201","CS202"]`, student has `["CS201"]` |
| | none required (boundary) | `[]` |


# Specifications

| # | Test | Setup | Expected status | Expected result |
|---|---|---|---|---|
| 1 | Happy path | active student with CS201, course requires CS201 | 201 | OK |
| 2 | Missing student | course exists, student not created | 200 | ERROR_NO_STUDENT |
| 3 | Inactive student | student status `inactive` | 200 | ERROR_INACTIVE_STUDENT |
| 4 | Missing course | student exists, course not created | 200 | ERROR_NO_COURSE |
| 5 | Prerequisite missing | student `[]`, course requires CS201 | 200 | ERROR_PREREQUISITES, missing `["CS201"]` |
| 6 | Double error: no student and no course | nothing created | 200 | ERROR_NO_STUDENT |
| 7 | Double error: inactive student and missing prerequisite | inactive, `[]`, course requires CS201 | 200 | ERROR_INACTIVE_STUDENT |
| 8 | Boundary: course has no prerequisites | student `[]`, course `[]` | 201 | OK |
| 9 | Boundary: courseID field missing | body `{"studentID":"X"}` | 400 | ERROR_BAD_REQUEST |
| 10 | Some prerequisites taken | course requires CS201+CS202, student has CS201 | 200 | ERROR_PREREQUISITES, missing `["CS202"]` |


