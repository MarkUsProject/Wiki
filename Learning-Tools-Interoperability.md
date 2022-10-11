# Learning Tools Interoperability (LTI)

MarkUs integrates with other Learning Management Systems via the LTI standard.
Currently MarkUs supports the following LMS platforms:

- Canvas

## For LMS Administrators

### Installing MarkUs on an LMS

Each LMS implements their own LTI integration process.
Typically, only administrators can add LTI integrations.

### Canvas

To add MarkUs to a Canvas instance, see their page on
[configuring LTI keys](https://community.canvaslms.com/t5/Admin-Guide/How-do-I-configure-an-LTI-key-for-an-account/ta-p/140).

MarkUs can be added via a configuration URL. The configuration URL is available at  /lti_deployments/get_canvas_config

## For Instructors

Once installed in your course, a 'Launch Markus' page will appear in your
course's navigation (disabled by default), and needs to be added to the navigation:

![Canvas MarkUs Navigation](images/canvas-markus-nav.png)

If you believe MarkUs should be installed in your course but it does not appear,
contact your LMS admistrators.

> **NOTE:**
> The additional navigation item will only be visible to instructors and
> administrators (not students)

### Associating your LMS Course with your MarkUs course

Once MarkUs is configured with your LMS, an association between your
Canvas course and your MarkUs course must be made.
Click 'Launch MarkUs' in your Canvas course. If you are not logged in to MarkUs,
you will be prompted to do so. Once you are logged in, you will be presented with
a list of MarkUs courses to choose from. Select the course that matches your Canvas
course and submit the form. If your course does not appear in the list,
you may click 'create new course', which will create a new
course based on the Canvas course information with you as an instructor.

### Creating a Grade Book entry for a MarkUs Assignment

Once a course association has been established, each assignment will
have an option to create an associated entry in the LMS course. This will allow grades from a MarkUs assignment
to be sent to the associated LMS once grading is complete.
Simply check the box for the course(s) you want to have a gradebook item in the
assignment settings page. After saving the settings, a new column for the assignment will
appear in the LMS grade book.

### Sending grades from MarkUs to the LMS

If an assignment has an LMS grade book entry, the assignment
summary page will have a 'Sync Grades to LMS' button.

![LTI Grade Sync](images/lti-grades-sync.png)

Clicking on this button will open a modal with a checkbox for each
associated LMS course. By default all of the boxes will be checked.
Uncheck the box for any course you would *not* like to sync grades to.

>**Note**: Only grades in the *released* state will be synced.
