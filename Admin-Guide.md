# Admin Guide

## Table of Contents

- [Navigating to the Administration Page](#navigating-to-the-administration-page)
- [Managing Courses](#managing-courses)

## Navigating to the Administration Page

After logging in to MarkUs as an Admin, you will be directed to the MarkUs Administration home page.

![Markus Admin Dashboard](images/markus-admin-dashboard.png)

If you navigate away from the Administration Page and wish to return, click on the "Administration" tab found in the header.

![Markus Admin Tab](images/markus-admin-tab.png)

**Note**: Ensure that you are viewing MarkUs as an admin and have not switched roles. Otherwise, you will be unable to access the "Administration" tab.

## Managing Courses

To manage courses, navigate to the "Courses" sub tab on the MarkUs Administration Page.

![Markus Course Tab](images/markus-admin-course-tab.png)

This will take you to a page that lists all the courses of this particular MarkUs instance. From this page, you can view and manage information about each course.

![Markus Course List](images/markus-admin-course-list.png)

### Creating and editing a course

In order to create a course, click on the "Create Course" link located at the top right corner of the page.

![Markus Course New Link](images/markus-admin-course-new-link.png)

This will redirect you to a page where you can specify the following course properties:

![Markus Course New Page](images/markus-admin-course-new.png)

- **Name**: The name or course code for this course
- **Display Name**: A longer course name or title for users to see
- **Course Visibility**: Selecting "hidden" will hide the course from students in a course. Graders and instructors for the course can still see and manage the course as usual.

After clicking "Save", the course will be created and you will be taken back to the list of all courses.

If you later wish to modify any properties of a course you can reach any course's edit page by going to the list of all courses and clicking on the "edit" action of the course you wish to modify.

![Markus Course Edit Link](images/markus-admin-course-edit-link.png)

From the edit page of a course you can modify the same properties specified when creating the course.

![Markus Course Edit Page](images/markus-admin-course-edit.png)

### Accessing a course

In order to access a specific course you may do so by going to the list of all courses and clicking on the "Go to Course" action of the course you wish to access.

![Markus View Course Page](images/markus-admin-go-to-course.png)

Doing so will take you to the course's dashboard page where you can view and access the course as if you were an instructor. This is accomplished by giving you an "AdminRole" for the course. This role is created automatically for you when you try and access a course via the UI for the first time. If you wish to create, view or update this role manually, please see the relevant ![api documentation](RESTful-API.md).
> :spiral_notepad: **NOTE:** Only you have the ability to view and access the Admin role. Instructors cannot see that you have this role when using either the UI or API.

## Managing Users

As with courses, to manage users, navigate to the "Users" sub tab on the MarkUs Administration Page.

![Markus User Tab](images/markus-admin-user-tab.png)

This will take you to a page that lists data about every admin and end user.

![Markus User List](images/markus-admin-users-list.png)

### Creating and Editing Users

In order to create a new user, click on the "Create User" link located at the top right corner of the page.

![Markus User New Link](images/markus-admin-user-new-link.png)

This will redirect you to a page where you can specify the following user information:

![Markus User New Page](images/markus-admin-user-new.png)

- **User Name**: The username alias for this user.
- **First Name**: This user's first name.
- **Last Name**: This user's last name.
- **ID Number**: This user's id number (optional).
- **Email**: This user's email address (optional).
- **Type**: A user can either be an "Admin User" or an "End User". In most cases, you will likely want to create an "End User". "End Users" are regular users who can later be specified as an "Instructor", "TA", or "Student" for a particular course. "Admin Users" have the ability to manage MarkUs and view all courses as if they were instructors. Be sure to only give an Admin User account to trusted users.

Clicking the "Save" button will create the user and take you back to the list of all users.

If you later wish to modify any properties of a user, you may do so by navigating to the list of users on the MarkUs Administration Page and clicking on the "edit" action of the user you wish to modify.

![Markus User Edit Link](images/markus-admin-user-edit-link.png)

This will take you to the user's edit page, where you can update the same properties specified when creating a user.

![Markus User Edit Page](images/markus-admin-user-edit.png)
