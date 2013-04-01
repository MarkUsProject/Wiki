================================================================================
RESTful API Documentation
================================================================================

This document provides an overview of the MarkUs RESTful API for use by developers 
as well as instructors. The API allows the use of standard HTTP methods such as 
GET, PUT, POST and DELETE to manipulate and retrieve resources. Those resources 
may also be retrieved either individually or from within collections. 

.. contents::

Authentication
--------------------------------------------------------------------------------

Authentication with the RESTful API is done using the HTTP Authorization header. 
The authorization method used is "MarkUsAuth", and should precede the encoded API 
token that can be found on the MarkUs Dashboard. 

To retrieve your API token:
1. Log in to MarkUs as an Admin or TA
2. Scroll to the bottom of the page, where you'll find your API key/token

Given ``MzNjMDcwMDhjZjMzY2E0NjdhODM2YWRkZmFhZWVjOGE=`` as one's MarkUs API token, 
an example header would include:
``Authorization: MarkUsAuth MzNjMDcwMDhjZjMzY2E0NjdhODM2YWRkZmFhZWVjOGE=``

To test your auth key, feel free to try the following from a terminal::

    curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users"

Replacing YourAuthKey and the example.com as necessary, the above command would 
return a list of all users in that particular MarkUs installation. Otherwise, if 
the authentication isn't successful, you'll receive an error with a 403 Forbidden 
HTTP Status Code. (should be 401 Unauthorized)

**Resetting Authentication Keys**

In case of stolen authentication tokens, they can be globally reset by the
system administrator using the *markus:reset_api_key* rake task. For example::

    $ cd path/to/markus/app
    $ bundle exec rake markus:reset_api_key

Response Formats
--------------------------------------------------------------------------------

As with other RESTful APIs, both XML and JSON responses are supported. XML version 
1.0 with UTF-8 encoding is the default response format used by the API. As would 
be expected, the response consists of an XML declaration followed by a root element, 
attributes, and may contain child elements and nested attributes. Due to it being 
the default format, the API will respond with XML if a .xml extension is present 
in the URL, or if no extension is provided. The following is an example XML 
response::

    <?xml version="1.0" encoding="UTF-8"?>
    <users>
      <last-name>admin</last-name>
      <type>Admin</type>
      <id>1</id>
      <user-name>a</user-name>
      <notes-count>0</notes-count>
      <first-name>admin</first-name>
      <grace-credits>0</grace-credits>
    </users>

If a .json extension is used in the URL, a JSON response will be rendered. Its 
simpler format consists of objects, represented as associative arrays. To request 
a JSON response using CURL, one can use the following::

    curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users/1.json"

Which would result in the following output (which has been formatted for readability)::

    {
      "admin":{
        "last_name":"admin",
        "type":"Admin",
        "id":1,
        "user_name":"a",
        "first_name":"admin",
        "notes_count":0,
        "grace_credits":0
      }
    }

RESTful Resources
--------------------------------------------------------------------------------

Methods on resources and collections available via the MarkUs API conform to Rails' 
RESTful routes. They consist of the following::

    GET    - collection - List resources along with attributes in a collection
    POST   - collection - Create a new entry in the collection
    GET    - resource   - Retrieve a single resource
    PUT    - resource   - Replace a resource, or update parts of a resource
    DELETE - resource   - Delete a resource

And the above correspond to the following default Rails routes: index, new, 
show, update, and destroy. Furthermore, nested routes allow us to take advantage 
of the relationship between different collections, sub-collections, and resources. 

For example, a GET request on ``/api/users``, with users being a collection, 
would return all users by default assuming no filters or other arguments. 
``/api/users/1``, which corresponds to a resource in the collection of users, 
would return only that user which identified by the unique id 1. To further 
illustrate, ``/api/users/1/notes``, with notes being a sub-collection, would 
return a list of all notes related to the user identified by the id 1. 
Sub-collections return only those resources which belong or apply to the parent 
resource.

Common Parameters
--------------------------------------------------------------------------------

The parameters below are available to most of the MarkUS RESTful API features, 
unless otherwise specified::

    limit:
      Use: Collections
      Default: none
      Limit the number of results returned from a collection.

    offset:
      Use: Collections
      Default: 0
      Specify the offset, that is the number of resources to skip in the response.

    filter:
      Use: Collections
      Filter a collection's results by a resource's attributes (name, date, etc)
      It will only return resource whose attributes match all given filter arguments
      Ie: filter=first_name:daniel,user_name:dst

    fields:
      Use: Collections, Resources
      Only return the fields listed in the request parameters.
      Ie: fields=user_name,first_name,last_name

For example, the filter parameter is available to collections such as api/users 
and api/assignments. To return only users that are of type TA, you can use the 
filter parameter with the argument "type:TA"::

    curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users.xml?filter=type:Ta"

You can also use parameters in combination with others. So, to return only a single 
user of type admin, you can make use of "limit"::

    curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users.xml?filter=type:admin&limit=1"

Currently Available Routes
--------------------------------------------------------------------------------

::

    GET     /api/users
    POST    /api/users
    GET     /api/users/id
    PUT     /api/users/id

    GET     /api/assignments
    POST    /api/assignments
    GET     /api/assignments/id
    PUT     /api/assignments/id

    GET     /api/assignments/id/groups
    GET     /api/assignments/id/groups/id

    GET     /api/assignments/id/groups/id/submission_downloads

    GET     /api/assignments/id/groups/id/test_results
    POST    /api/assignments/id/groups/id/test_results
    GET     /api/assignments/id/groups/id/test_results/id
    PUT     /api/assignments/id/groups/id/test_results/id
    DELETE  /api/assignments/id/groups/id/test_results/id

/api/users
--------------------------------------------------------------------------------

| **POST /api/users**
| Description: Creates a new user
| Requires: user_name, type, first_name, last_name
| Optional: section_name, grace_credits
| CURL example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" --data \
    "user_name=testing123&type=admin&last_name=testing&first_name=testagain&grace_credits=3" \
    "http://example.com/api/users.xml"
    <?xml version="1.0"?>
    <rsp status="201">
    The resource has been created.
    </rsp>

| **GET /api/users**
| Description: Returns users and their attributes
| Attributes: id, user_name, type, first_name, last_name, section_name, grace_credits
| Optional: filter, fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <users>
      <user>
        <grace-credits>0</grace-credits>
        <type>Admin</type>
        <id>1</id>
        <notes-count>0</notes-count>
        <last-name>admin</last-name>
        <user-name>a</user-name>
        <first-name>admin</first-name>
      </user>
      <user>
        <grace-credits>0</grace-credits>
        <type>Admin</type>
        <id>2</id>
        <notes-count>0</notes-count>
        <last-name>Reid</last-name>
        <user-name>reid</user-name>
        <first-name>Karen</first-name>
      </user>
    </users>

| **GET /api/users/id**
| Description: Returns a user and its attributes
| Attributes: id, user_name, type, first_name, last_name, section_name, grace_credits
| Optional: fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/users/1.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <users>
      <grace-credits>0</grace-credits>
      <type>Admin</type>
      <id>1</id>
      <notes-count>0</notes-count>
      <last-name>admin</last-name>
      <user-name>a</user-name>
      <first-name>admin</first-name>
    </users>

| **PUT /api/users/id**
| Description: Updates the attributes of the given user
| Optional: user_name, type, first_name, last_name, section_name, grace_credits
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" -X PUT --data \
    "user_name=newUserName&type=admin" "http://example.com/api/users/100.xml"
    <?xml version="1.0"?>
    <rsp status="200">
    Success
    </rsp>

/api/assignments
--------------------------------------------------------------------------------

| **POST /api/assignments**
| Description: Creates a new assignment
| Requires: short_identifier, due_date [YYYY-MM-DD]
| Optional: repository_folder, group_min, group_max, tokens_per_day,
            submission_rule_type, marking_scheme_type, allow_web_submits, 
            display_grader_names_to_students, enable_test, assign_graders_to_criteria, 
            description, message, allow_remarks, remark_due_date, remark_message,
            student_form_groups, group_name_autogenerated, submission_rule_deduction,
            submission_rule_hours, submission_rule_interval
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" --data \
    "short_identifier=AsTest&due_date=2023-12-13" "http://example.com/api/assignments.xml"
    <?xml version="1.0"?>
    <rsp status="201">
    The resource has been created.
    </rsp>

| **GET /api/assignments**
| Description: Returns assignments and their attributes
| Attributes: id, description, short_identifier, message, due_date, 
              group_min, group_max, tokens_per_day, allow_web_submits, 
              student_form_groups, remark_due_date, remark_message,
              assign_graders_to_criteria, enable_test, allow_remarks,
              display_grader_names_to_students, group_name_autogenerated,
              marking_scheme_type, repository_folder
| Optional: filter, fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/assignments.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <assignments>
      <assignment>
        <remark-due-date nil="true"></remark-due-date>
        <student-form-groups>false</student-form-groups>
        <assign-graders-to-criteria>false</assign-graders-to-criteria>
        <tokens-per-day>0</tokens-per-day>
        <description>Conditionals and Loops</description>
        <allow-remarks>true</allow-remarks>
        <remark-message nil="true"></remark-message>
        <message>Learn to use conditional statements, and loops.</message>
        <id>1</id>
        <display-grader-names-to-students>false</display-grader-names-to-students>
        <group-max>1</group-max>
        <due-date>2013-03-23T15:40:39-04:00</due-date>
        <group-name-autogenerated>true</group-name-autogenerated>
        <group-min>1</group-min>
        <short-identifier>A1</short-identifier>
        <repository-folder>A1</repository-folder>
        <enable-test>false</enable-test>
        <allow-web-submits>true</allow-web-submits>
        <marking-scheme-type>rubric</marking-scheme-type>
      </assignment>
      <assignment>
        <remark-due-date nil="true"></remark-due-date>
        <student-form-groups>true</student-form-groups>
        <assign-graders-to-criteria>false</assign-graders-to-criteria>
        <tokens-per-day>0</tokens-per-day>
        <description>Cats and Dogs</description>
        <allow-remarks>true</allow-remarks>
        <remark-message nil="true"></remark-message>
        <message>Basic exercise in Object Oriented
                          Programming.  Implement Animal, Cat, and Dog, as
                          described in class.</message>
        <id>2</id>
        <display-grader-names-to-students>false</display-grader-names-to-students>
        <group-max>3</group-max>
        <due-date>2013-04-23T15:39:40-04:00</due-date>
        <group-name-autogenerated>true</group-name-autogenerated>
        <group-min>2</group-min>
        <short-identifier>A2</short-identifier>
        <repository-folder>A2</repository-folder>
        <enable-test>false</enable-test>
        <allow-web-submits>true</allow-web-submits>
        <marking-scheme-type>rubric</marking-scheme-type>
      </assignment>
    </assignments>

| **GET /api/assignments/id**
| Description: Returns an assignment and its attributes
| Attributes: id, description, short_identifier, message, due_date, 
              group_min, group_max, tokens_per_day, allow_web_submits, 
              student_form_groups, remark_due_date, remark_message,
              assign_graders_to_criteria, enable_test, allow_remarks,
              display_grader_names_to_students, group_name_autogenerated,
              marking_scheme_type, repository_folder
| Optional: fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/assignments/1.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <assignment>
      <remark-due-date nil="true"></remark-due-date>
      <student-form-groups>false</student-form-groups>
      <assign-graders-to-criteria>false</assign-graders-to-criteria>
      <tokens-per-day>0</tokens-per-day>
      <description>Conditionals and Loops</description>
      <allow-remarks>true</allow-remarks>
      <remark-message nil="true"></remark-message>
      <message>Learn to use conditional statements, and loops.</message>
      <id>1</id>
      <display-grader-names-to-students>false</display-grader-names-to-students>
      <group-max>1</group-max>
      <due-date>2013-03-23T15:40:39-04:00</due-date>
      <group-name-autogenerated>true</group-name-autogenerated>
      <group-min>1</group-min>
      <short-identifier>A1</short-identifier>
      <repository-folder>A1</repository-folder>
      <enable-test>false</enable-test>
      <allow-web-submits>true</allow-web-submits>
      <marking-scheme-type>rubric</marking-scheme-type>
    </assignment>

| **PUT /api/assignments/id**
| Description: Updates an assignment
| Requires: short_identifier, due_date [YYYY-MM-DD]
| Optional: repository_folder, group_min, group_max, tokens_per_day,
            submission_rule_type, marking_scheme_type, allow_web_submits, 
            display_grader_names_to_students, enable_test, assign_graders_to_criteria, 
            description, message, allow_remarks, remark_due_date, remark_message,
            student_form_groups, group_name_autogenerated, submission_rule_deduction,
            submission_rule_hours, submission_rule_interval
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" -X PUT --data \
    "short_identifier=As1Test" "http://example.com/api/assignments/1.xml"
    <?xml version="1.0"?>
    <rsp status="200">
    Success
    </rsp>

/api/assignments/id/groups
--------------------------------------------------------------------------------

| **GET /api/assignments/id/groups**
| Description: Returns an assignment's groups along with their attributes
| Attributes:  id, group_name, created_at, updated_at, first_name, last_name, 
               user_name, membership_status, student_memberships
| Optional: filter, fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/assignments/1/groups.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <groups>
      <group>
        <group-name>c8mahler</group-name>
        <id>1</id>
        <student-memberships>
          <student-membership>
            <id>1</id>
            <updated-at>2013-03-23T15:39:40-04:00</updated-at>
            <created-at>2013-03-23T15:39:40-04:00</created-at>
            <membership-status>inviter</membership-status>
            <user>
              <first-name>Gustav</first-name>
              <id>3</id>
              <updated-at>2013-03-23T15:39:34-04:00</updated-at>
              <created-at>2013-03-23T15:39:34-04:00</created-at>
              <last-name>Mahler</last-name>
              <user-name>c8mahler</user-name>
            </user>
          </student-membership>
        </student-memberships>
      </group>
      <group>
        <group-name>c9magnar</group-name>
        <id>2</id>
        <student-memberships>
          <student-membership>
            <id>2</id>
            <updated-at>2013-03-23T15:39:40-04:00</updated-at>
            <created-at>2013-03-23T15:39:40-04:00</created-at>
            <membership-status>inviter</membership-status>
            <user>
              <first-name>Alberic</first-name>
              <id>4</id>
              <updated-at>2013-03-23T15:39:34-04:00</updated-at>
              <created-at>2013-03-23T15:39:34-04:00</created-at>
              <last-name>Magnard</last-name>
              <user-name>c9magnar</user-name>
            </user>
          </student-membership>
        </student-memberships>
      </group>
    </groups>

| **GET /api/assignments/id/groups/id**
| Description: Returns a single group along with its attributes
| Attributes:  id, group_name, created_at, updated_at, first_name, last_name, 
               user_name, membership_status, student_memberships
| Optional: fields
| Example:
::

    $ curl -H "Authorization: MarkUsAuth YourAuthKey" "http://example.com/api/assignments/1/groups/1.xml"
    <?xml version="1.0" encoding="UTF-8"?>
    <group>
      <group-name>c8mahler</group-name>
      <id>1</id>
      <student-memberships>
        <student-membership>
          <id>1</id>
          <updated-at>2013-03-23T15:39:40-04:00</updated-at>
          <created-at>2013-03-23T15:39:40-04:00</created-at>
          <membership-status>inviter</membership-status>
          <user>
            <first-name>Gustav</first-name>
            <id>3</id>
            <updated-at>2013-03-23T15:39:34-04:00</updated-at>
            <created-at>2013-03-23T15:39:34-04:00</created-at>
            <last-name>Mahler</last-name>
            <user-name>c8mahler</user-name>
          </user>
        </student-membership>
      </student-memberships>
    </group>

/api/assignments/id/groups/id/submission_downloads
--------------------------------------------------------------------------------

| **GET /api/assignments/id/groups/id/submission_downloads**
| Description: If filename is specified, it returns the given file from the 
               submission, otherwise it returns a zip containing all submitted 
               files.
| Optional: filename
| Example:
::

    $ curl --header "Authorization: MarkUsAuth YourAuthKey" \
    "http://example.com/api/assignments/1/groups/5/submission_downloads" > Submission.zip
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 26898  100 26898    0     0  26271      0  0:00:01  0:00:01 --:--:-- 26396

Notes on Test Results
--------------------------------------------------------------------------------

Filenames of test results per submission have to be unique. Several HTTP POST requests with
the same filename, but different file_content parameter, will overwrite
already existing test results.

A typical request in order to load test results into MarkUs looks like the
following (using curl)::

    $ file_content=`cat app/controllers/assignments_controller.rb`; curl --header 'Authorization: MarkUsAuth NmY3NGUxNjEyY2FlNzk0NTMwMmQ5YTY1YTE1NzNhZmY=' \
      -F group_name=c5anthei -F assignment=A1 -F filename=test.txt -F "file_content=$file_content" http://example.com/markus/api/test_results

**NOTE** This only works, if for the specified assignment and group a
submission has been "collected". This usually happens after the assignment due
date and after the grace period.

**IMPORTANT** The current implementation of
test results, does not allow binary files to be pushed into MarkUs. The
behaviour of submitting binary test results this way is undefined.

**Note:** MarkUs versions > 0.7 ship with a Python (api_helper.py) and Ruby
(api_helper.rb) script in lib/tools/ which may be of some help for generating
those requests.

