# RESTful API Documentation

This document provides an overview of the MarkUs RESTful API for use by developers as well as instructors. The API allows the use of standard HTTP methods such as GET, PUT, POST and DELETE to manipulate and retrieve resources. Those resources may also be retrieved either individually or from within collections.

## General

### Authentication

Authentication with the RESTful API is done using the HTTP Authorization header. The authorization method used is "MarkUsAuth", and should precede the encoded API token that can be found on the MarkUs Dashboard.

To retrieve your API token: 

1. Log in to MarkUs. 
2. At the top right of the page click the "Settings" button and go to the "Your API Key" section. Your api key should be visible (or it may be "unavailable" if it hasn't been generated for the first time yet)
3. copy the api key or click the "Reset API Key" button to generate a new one.

Given `MzNjMDcwMDhjZjMzY2E0NjdhODM2YWRkZmFhZWVjOGE=` as one's MarkUs API token, an example header would include: `Authorization: MarkUsAuth MzNjMDcwMDhjZjMzY2E0NjdhODM2YWRkZmFhZWVjOGE=`

**Resetting Authentication Keys**

In case of stolen authentication tokens, they can be globally reset by the system administrator using the `markus:reset_api_key` rake task. For example:

    $ cd path/to/markus/app
    $ bundle exec rake markus:reset_api_key

### Response Formats

Both XML and JSON responses are supported. XML version 1.0 with UTF-8 encoding is the default response format used by the API. The response consists of an XML declaration followed by a root element, attributes, and may contain child elements and nested attributes. Due to it being the default format, the API will respond with XML if a .xml extension is present in the URL, or if no extension is provided. The following is an example XML response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<user>
  <notes-count>1</notes-count>
  <grace-credits>0</grace-credits>
  <last-name>admin</last-name>
  <type>Admin</type>
  <first-name>admin</first-name>
  <id>1</id>
  <user-name>a</user-name>
</user>
```
If a .json extension is used in the URL, a JSON response will be rendered. Its simpler format consists of objects, represented as associative arrays. To request a JSON response using CURL, one can use the following:

    curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users/1.json"

Which would result in the following output:


```json
{
  "id": 1,
  "user_name": "a",
  "last_name": "instructor",
  "first_name": "instructor",
  "type": "EndUser",
  "email": null,
  "id_number": null
}
```

### Common optional parameters

#### filter

The filter parameter is commonly used when multiple records are expected, this parameter will filter the records so that only those that match the filter are returned. 

For example, if a route returns multiple user records, you may choose to filter on the first name by passing the following parameter (formatted as json): 

```json
{"filter": {"first_name": "Steve"}}
```

and only users with the first name "Steve" will be returned


#### fields

The fields parameter is commonly used when multiple fields per record are expected, this parameter will filter the fields so that only the requested fields are returned. By default all fields are returned.

For example, if a route normally returns the following user record:

```json
{
  "id": 1,
  "user_name": "a",
  "last_name": "instructor",
  "first_name": "instructor",
  "type": "EndUser",
  "email": null,
  "id_number": null
}
```

but you only care about the id and user_name fields you can pass the following parameter (formatted as json):

```json
{"fields": ["id", "user_name"]}
```

and the route will now return the following instead:

```json
{
  "id": 1,
  "user_name": "a"
}
```

### GET /api/users

- description: Display all user information
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[{
  "id": 1,
  "user_name": "a",
  "last_name": "instructor",
  "first_name": "instructor",
  "type": "EndUser",
  "email": null,
  "id_number": null
}]
```

NOTE: this method is only available to AdminUser users

### POST /api/users

- description: Create a user
- required parameters:
  - user_name (string)
  - type (one of "EndUser", "AdminUser")
  - first_name (string)
  - last_name (string)
- optional parameters:
  - email (string)
  - id_number (string)

NOTE: this method is only available to AdminUser users

### PUT /api/users/update_by_username

- description: Update attributes for a single user identified by their user_name
- required parameters:
  - user_name (string)
- optional parameters:
  - first_name (string)
  - last_name (string)
  - email (string)
  - id_number (string)

NOTE: this method is only available to AdminUser users

### GET /api/users/:id

- description: Display user information for a single user
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
{
  "id": 1,
  "user_name": "a",
  "last_name": "instructor",
  "first_name": "instructor",
  "type": "EndUser",
  "email": null,
  "id_number": null
}
```

NOTE: this method is only available to AdminUser users

### PUT /api/users/:id

- description: Update attributes for a single user
- optional parameters:
  - first_name (string)
  - last_name (string)
  - email (string)
  - id_number (string)

NOTE: this method is only available to AdminUser users

### GET /api/courses

- description: Display all course information 
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[
  {
    "id": 1,
    "name": "course1",
    "is_hidden": false,
    "display_name": "a longer course name to display in the UI"
  }
]
```

NOTE: If not an AdminUser, this will only return courses where the current user is enrolled as an Instructor

### POST /api/courses

- description: Create a course
- required parameters:
  - name (string)
  - is_hidden (boolean)
  - display_name (string)

NOTE: this method is only available to AdminUser users

### GET /api/courses/:id

- description: Display course information for a single course
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
{
  "id": 1,
  "name": "course1",
  "is_hidden": false,
  "display_name": "a longer course name to display in the UI"
}
```

NOTE: If not an AdminUser, this will only return courses where the current user is enrolled as an Instructor

### PUT /api/courses/:id

- description: Update attributes for a single course
- optional parameters:
  - name (string)
  - is_hidden (boolean)
  - display_name (string)

NOTE: this method is only available to AdminUser users

### PUT /api/courses/:id/update_autotest_url

- description: Update or set the url of the server running the [automated test software](https://github.com/MarkUsProject/markus-autotesting) for this course 
- required parameters:
  - url (string: well formed URL)

NOTE: this method is only available to AdminUser users

### GET /api/courses/:course_id/roles

- description: Display all role information for the given course
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[
  {
    "id": 230,
    "type": "Student",
    "hidden": false,
    "grace_credits": 0,
    "user_name": "role123",
    "email": null,
    "id_number": null,
    "first_name": "Stu",
    "last_name": "Dent"
  }
]
```

### POST /api/courses/:course_id/roles

- description: Create a role for a user in the given course
- required parameters:
  - user_name (string: a user with this user name must exist)
  - type (string: one of "Instructor", "Ta", "Student")
- optional parameters:
  - grace_credits (integer)
  - hidden (boolean)
  - section_name (string: name of a Section for the given course)

### POST /api/courses/:course_id/roles/create_or_unhide

- description: Create a role for a user in the given course, if the given role already exists set the hidden attribute to `false` instead
- required parameters:
  - user_name (string: a user with this user name must exist)
  - type (string: one of "Instructor", "Ta", "Student")
- optional parameters:
  - grace_credits (integer)
  - hidden (boolean)
  - section_name (string: name of a Section for the given course)

## PUT /api/courses/:course_id/roles/update_by_username

- description: Update attributes for a role identified by the user name attribute
- required parameters:
  - user_name (string: a user with this user name must exist)
- optional parameters:
  - grace_credits (integer)
  - hidden (boolean)
  - section_name (string: name of a Section for the given course)

### GET /api/courses/:course_id/roles/:id

- description: Display role information for a single role
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
{
  "id": 230,
  "type": "Student",
  "hidden": false,
  "grace_credits": 0,
  "user_name": "role123",
  "email": null,
  "id_number": null,
  "first_name": "Stu",
  "last_name": "Dent"
}
```

### PUT /api/courses/:course_id/roles/:id

- description: Update attributes for a single role
- optional parameters:
  - grace_credits (integer)
  - hidden (boolean)
  - section_name (string: name of a Section for the given course)


### GET /api/courses/:course_id/grade_entry_forms

- description: Display all grade entry form information for the given course
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[
  {
    "id": 7,
    "short_identifier": "Quiz1",
    "description": "Class Quiz on Variables",
    "due_date": "2080-12-16T11:26:48.264-05:00",
    "is_hidden": false,
    "show_total": false,
    "grade_entry_items": [
      {
        "id": 1,
        "name": "Q1",
        "out_of": 3
      },
      {
        "id": 2,
        "name": "Q2",
        "out_of": 4
      },
      {
        "id": 3,
        "name": "Q3",
        "out_of": 5
      }
    ]
  }
]
```

### POST /api/courses/:course_id/grade_entry_forms

- description: Create a grade entry form for the given course
- required parameters:
  - short_identifier (string)
- optional parameters:
  - description (string)
  - is_hidden (boolean)
  - show_total (boolean)
  - due_date (string: that can be parsed into a Ruby DateTime object)
  - grade_entry_items:
    - name (string)
    - out_of (integer)
    - bonus (boolean)

### GET /api/courses/:course_id/grade_entry_forms/:id

- description: Display grade entry form information for a single form
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
{
  "id": 7,
  "short_identifier": "Quiz1",
  "description": "Class Quiz on Variables",
  "due_date": "2080-12-16T11:26:48.264-05:00",
  "is_hidden": false,
  "show_total": false,
  "grade_entry_items": [
    {
      "id": 1,
      "name": "Q1",
      "out_of": 3
    },
    {
      "id": 2,
      "name": "Q2",
      "out_of": 4
    },
    {
      "id": 3,
      "name": "Q3",
      "out_of": 5
    }
  ]
}
```

### PUT api/courses/:course_id/grade_entry_forms/:id

- description: Update attributes for a grade entry form for the given course  
- optional parameters:
  - short_identifier (string)
  - description (string)
  - is_hidden (boolean)
  - show_total (boolean)
  - due_date (string: that can be parsed into a Ruby DateTime object)
  - grade_entry_items:
    - id: (integer or null : if null a new grade_entry_item will be created)
    - name (string)
    - out_of (integer)
    - bonus (boolean)


## PUT /api/courses/:course_id/grade_entry_forms/:id/update_grades

- description: Update the grade(s) in this grade_entry_form for a student
- required parameters:
  - user_name (string : user name of a student in this course)
  - grade_entry_items (list of list of strings: the first string is a grade entry item name and the second is an integer to be assigned as a score. For example: `[["Q1", 100], ["Q2", 23]]`)

### GET /api/courses/:course_id/assignments

- description: Display all assignment information for the given course
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[
  {
    "id": 18,
    "description": "Assignment the first",
    "short_identifier": "A1",
    "message": "",
    "due_date": "1934-12-23T12:09:24.976-05:00",
    "group_min": 1,
    "group_max": 1,
    "tokens_per_period": 0,
    "allow_web_submits": true,
    "student_form_groups": false,
    "remark_due_date": null,
    "remark_message": null,
    "assign_graders_to_criteria": false,
    "enable_test": true,
    "enable_student_tests": true,
    "allow_remarks": false,
    "display_grader_names_to_students": false,
    "group_name_autogenerated": false,
    "repository_folder": "autotest_racket",
    "is_hidden": false,
    "vcs_submit": false,
    "token_period": 1,
    "non_regenerating_tokens": false,
    "unlimited_tokens": true,
    "token_start_date": "1956-12-16T12:09:00.000-05:00",
    "has_peer_review": false,
    "starter_file_type": "simple",
    "default_starter_file_group_id": null
  }
]
```

### POST /api/courses/:course_id/assignments

- description: Create an assignment for the given course
- required parameters:
  - short_identifier (string)
  - description (string)
  - due_date (string: that can be parsed into a Ruby DateTime object)
- optional parameters:
  - message (string)
  - group_min (integer)
  - group_max (integer)
  - tokens_per_period (integer)
  - allow_web_submits (boolean)
  - student_form_groups (boolean)
  - remark_due_date (string: that can be parsed into a Ruby DateTime object)
  - remark_message (string)
  - assign_graders_to_criteria (boolean)
  - enable_test (boolean)
  - enable_student_tests (boolean)
  - allow_remarks (boolean)
  - display_grader_names_to_students (boolean)
  - group_name_autogenerated (boolean)
  - is_hidden (boolean)
  - vcs_submit (boolean)
  - token_period (integer)
  - non_regenerating_tokens (boolean)
  - unlimited_tokens (boolean)
  - token_start_date (string: that can be parsed into a Ruby DateTime object)
  - has_peer_review (boolean)
  - starter_file_type (one of "simple", "sections", "shuffle", "group")

### GET /api/courses/:course_id/assignments/:id

- description: Display attributes for a single assignment
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
{
  "id": 18,
  "description": "Assignment the first",
  "short_identifier": "A1",
  "message": "",
  "due_date": "1934-12-23T12:09:24.976-05:00",
  "group_min": 1,
  "group_max": 1,
  "tokens_per_period": 0,
  "allow_web_submits": true,
  "student_form_groups": false,
  "remark_due_date": null,
  "remark_message": null,
  "assign_graders_to_criteria": false,
  "enable_test": true,
  "enable_student_tests": true,
  "allow_remarks": false,
  "display_grader_names_to_students": false,
  "group_name_autogenerated": false,
  "repository_folder": "autotest_racket",
  "is_hidden": false,
  "vcs_submit": false,
  "token_period": 1,
  "non_regenerating_tokens": false,
  "unlimited_tokens": true,
  "token_start_date": "1956-12-16T12:09:00.000-05:00",
  "has_peer_review": false,
  "starter_file_type": "simple",
  "default_starter_file_group_id": null
}
```

### PUT /api/courses/:course_id/assignments/:id

- description: Update attributes for a single assignment
- optional parameters:
  - description (string)
  - due_date (string: that can be parsed into a Ruby DateTime object)
  - message (string)
  - group_min (integer)
  - group_max (integer)
  - tokens_per_period (integer)
  - allow_web_submits (boolean)
  - student_form_groups (boolean)
  - remark_due_date (string: that can be parsed into a Ruby DateTime object)
  - remark_message (string)
  - assign_graders_to_criteria (boolean)
  - enable_test (boolean)
  - enable_student_tests (boolean)
  - allow_remarks (boolean)
  - display_grader_names_to_students (boolean)
  - group_name_autogenerated (boolean)
  - is_hidden (boolean)
  - vcs_submit (boolean)
  - token_period (integer)
  - non_regenerating_tokens (boolean)
  - unlimited_tokens (boolean)
  - token_start_date (string: that can be parsed into a Ruby DateTime object)
  - has_peer_review (boolean)
  - starter_file_type (one of "simple", "sections", "shuffle", "group")

### GET /api/courses/:course_id/assignments/:id/test_files

- description: Download a zip file containing all autotesting test files for this assignment

### GET /api/courses/:course_id/assignments/:id/grades_summary

- description: Download a csv file containing a summary of grades the given assignment

### GET /api/courses/:course_id/assignments/:id/test_specs

- description: Download a json string containing the autotesting settings for this assignment
- example response (json):

```json
{
  "testers": [
    {
      "test_data": [
        {
          "category": [
            "instructor"
          ],
          "extra_info": {
            "name": "Test group",
            "display_output": "instructors",
            "test_group_id": 12
          },
          "script_files": [],
          "timeout": 30
        }
      ],
      "tester_type": "java"
    }
  ]
}
```

### POST /api/courses/:course_id/assignments/:id/update_test_specs

- description: Update the autotesting settings for this assignment
- required parameters:
  - specs (json string : see the `GET test_specs` description above for an example of the expected format)

NOTE: This will also send the updated specs to the server running the autotester

### GET /api/courses/:course_id/assignments/:assignment_id/groups

- description: Get all group information for the given assignment
- optional parameters:
  - [filter](#filter)
  - [fields](#fields)
- example response (json):

```json
[
  {
    "id": 1,
    "group_name": "group_0001",
    "members": [
      {
        "membership_status": "inviter",
        "user_id": 9
      },
      {
        "membership_status": "accepted",
        "user_id": 24
      }
    ]
  }
]
```

### GET /api/courses/:course_id/assignments/:assignment_id/groups/annotations

- description: Get all annotation information for the given assignment
- example response (json):

```json
[
    {
    "type": "TextAnnotation",
    "content": "Ut magni.",
    "filename": "hello.py",
    "path": "A0",
    "page": null,
    "group_id": 15,
    "category": "asperiores",
    "creator_id": 1,
    "content_creator_id": 1,
    "line_end": 12,
    "line_start": 12,
    "column_start": 1,
    "column_end": 26,
    "x1": null,
    "y1": null,
    "x2": null,
    "y2": null
  },
  {
    "type": "PdfAnnotation",
    "content": "Officia et dolore.",
    "filename": "pdf.pdf",
    "path": "A0/another-test-files-in-inner-dirs",
    "page": 1,
    "group_id": 15,
    "category": "suscipit eos",
    "creator_id": 1,
    "content_creator_id": 1,
    "line_end": null,
    "line_start": null,
    "column_start": null,
    "column_end": null,
    "x1": 27740,
    "y1": 58244,
    "x2": 4977,
    "y2": 29748
  }
]
```

### GET /api/courses/:course_id/assignments/:assignment_id/groups/group_ids_by_name

- description: Get a mapping of group names to id for the given assignment
- example response (json):

```json
{
  "c9talmal": 204,
  "c8tchaik": 205,
  "g6takemi": 206,
  "c6weinbe": 207,
  "c7franca": 208,
  "c5nancar": 209,
  "c9simpso": 210,
  "c7kimear": 211,
  "g9koppel": 212,
  "c6lloydg": 213,
}
```

GET      /api/courses/:course_id/assignments/:assignment_id/groups/:id
PATCH    /api/courses/:course_id/assignments/:assignment_id/groups/:id
PUT      /api/courses/:course_id/assignments/:assignment_id/groups/:id
GET      /api/courses/:course_id/assignments/:assignment_id/groups/:id/annotations
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:id/add_annotations
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:id/add_members
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:id/create_extra_marks
PUT      /api/courses/:course_id/assignments/:assignment_id/groups/:id/update_marks
PUT      /api/courses/:course_id/assignments/:assignment_id/groups/:id/update_marking_state
DELETE   /api/courses/:course_id/assignments/:assignment_id/groups/:id/remove_extra_marks

DELETE   /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/submission_files/remove_file
DELETE   /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/submission_files/remove_folder
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/submission_files/create_folders
GET      /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/submission_files
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/submission_files
GET      /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/feedback_files
POST     /api/courses/:course_id/assignments/:assignment_id/groups/:group_id/feedback_files


GET      /api/courses/:course_id/assignments/:assignment_id/starter_file_groups
POST     /api/courses/:course_id/assignments/:assignment_id/starter_file_groups

GET      /api/courses/:course_id/feedback_files/:id
PATCH    /api/courses/:course_id/feedback_files/:id
PUT      /api/courses/:course_id/feedback_files/:id
DELETE   /api/courses/:course_id/feedback_files/:id
PATCH    /api/courses/:course_id/submission_files/:id
PUT      /api/courses/:course_id/submission_files/:id
DELETE   /api/courses/:course_id/submission_files/:id

GET      /api/courses/:course_id/starter_file_groups/:id
PATCH    /api/courses/:course_id/starter_file_groups/:id
PUT      /api/courses/:course_id/starter_file_groups/:id
DELETE   /api/courses/:course_id/starter_file_groups/:id
GET      /api/courses/:course_id/starter_file_groups/:id/entries
POST     /api/courses/:course_id/starter_file_groups/:id/create_file
POST     /api/courses/:course_id/starter_file_groups/:id/create_folder
DELETE   /api/courses/:course_id/starter_file_groups/:id/remove_file
DELETE   /api/courses/:course_id/starter_file_groups/:id/remove_folder
GET      /api/courses/:course_id/starter_file_groups/:id/download_entries
