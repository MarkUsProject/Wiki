# Instructor Frequently Asked Questions

## How do I upgrade a grader (t2narges) to an admin?

You can't.  However, there is a workaround.  Edit the logname t2narges
to something which is not a possible account name, then create a new
admin account which is t2narges.  (And if you want to remove her admin
privileges at a future time, rename the new t2narges to something else
and rename the original back to t2narges.)

## How do I give students a personal extension/accept a late file submitted after the final deadline?

Students can submit files after the final deadline - tell students to ignore the warning
and submit as usual.

To collect:

1. Go to "Submissions" view.
2. Click on the repo name of the group (second column in the table).
3. Click on the "Collect and Grade This Revision" button.
4. You should be redirected to the submission, but you can also access it from the Submissions table.

Note: you can upload autotesting results that came later than the big batch.

## How do I accept work which a student submitted to the wrong assignment?

For an **individual** assignment: tell the student to submit it to the correct assignment.
You can manually collect and grade this late submission even if it happens after the final deadline
(see previous question).

For a **group** assignment: if the student has already created a group for the correct
assignment, tell them to submit and then follow the instructions in the previous question.

If the student has not already created a group, you can do the same thing, but first you must
create one manually for the student (under the Groups tab).


## How do I make a grade change?

For an **assignment**: unrelease the assignment, then make the desired changes, and re-release it.

For a **marks spreadsheet**: you can change the mark directly in the table, or upload
a CSV file to update the marks. Unreleasing is possible, but not necessary.


## How do I checkout all submissions for an assignment?

Before assignments have been collected (e.g., for doing an autotesting dry run):

1. Go to the Submissions table and click on "Subversion Repo List".
You can extract the SVN repo urls directly from there.

After assignments have been collected - uses **correct revision number automatically**:

1. Go to the Submissions table and click on "Subversion Checkout File".
2. The downloaded file is a script you can use to checkout all student submissions,
with the correct revision numbers (with respect to the assignment due date).
