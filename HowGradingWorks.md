How Grading and Re-grading Works
================================

After an Assignment is made available, Groupings of students (where a Grouping is 1 or more students) upload/modify their work files through MarkUs's Repository Library, while the Assignment deadline is still in the future.

The Relationship between Submissions, SubmissionFiles and the Repository Library
--------------------------------------------------------------------------------

When the Assignment deadlines passes, Submission and SubmissionFile objects are created. Submission is just a timestamp for the due-date, and has\_many SubmissionFiles, where a SubmissionFile is a single record for a particular file that exists in the File Persistence Layer for each Grouping of students.

It sounds complicated, but think of it this way: the TA's who are marking the work are interested in a particular revision of the work. We are also going to be attaching Annotations to files within this particular revision of work. The Submission record allows us to take this "slice" of work, and the SubmissionFiles let us Annotate the files within this "slice" of work.

The Life-Cycle of a Submission
------------------------------

[See: Submission Collection Questions](wiki:Submission\_Collection)

At some point after the due-date of an Assignment, something triggers the creation of Submissions for each Grouping (where a grouping is 1 or more Students) that worked on this Assignment.

An Assignment has a collect\_submissions\_on (or something similar) date. At some point after that date (during the work flow, or perhaps with a cron job) MarkUs will create Submissions for each Grouping for that date.

For each Submission, the commit timestamp is analyzed against the due-date of the assignment. For an assignment past the due-date, the SubmissionRule for that Assignment comes into effect for that Submission.

The SubmissionRule can take the information about the Assignment, Submission, Grouping / Group, and Students within that Grouping, and apply penalties to the Submission. For example, a SubmissionRule could be written that allows for grace days to be subtracted from a Group.

The Relationship between Submissions and Results
------------------------------------------------

Once the Assignment deadline has passed, (and the maximum number of grace days passed as well?) TA's can log into MarkUs to see the Students' work. The TA will be looking at 1 Submission per Grouping.

Marks, which are connected to RubricCriteria, are connected to a Results object, which are finally attached to the Submission.

Future Plans for Results
------------------------

The reason why Submissions can have *many* Results, is the possibility that we will eventually want *versioned* Results.

Take this remarking case for example:

Brandon, a student, has finished uploading work for Assignment "E1". A few hours later, E1's deadline passes.

Vikram, the TA, logs into MarkUs, and begins marking Brandon's work. Vikram finishes, and sets the marking status of this Submission to "complete".

Eventually, the TA's finish grading all of the Submissions, and the grades are released to the student.

Brandon has noticed that a TA has erroneously claimed that he did not complete a particular problem. He reports this to his instructor, who asks Vikram to remark Brandon's work.

With versioned Results, Vikram tells MarkUs to create a NEW Result for Brandon's Submission. MarkUs creates the new Result, and clones the Marks and Annotations over to the new Result. Vikram makes the appropriate changes, and re-releases the grade. MarkUs marks the new Result as the current one being used, and Brandon is able to see his mark has changed.

Essentially, the advantage to versioning, is the ability to see the history of remarks, and to allow rollbacks.

The Life Cycle of a Remark Request
----------------------------------

Relevant files:

-   app/controllers/results\_controller.rb
-   app/models/submission.rb

New remark requests are created by `results_controller.update_remark_request`, which calls `Submission.create_remark_result`, where a new `Result` is created and linked to the `Submission` by setting `Submission.remark_result_id` to the new `Result.id`. All of the `Mark` and `ExtraMark` in the new remark `Result` are populated with the original marks at the time of creation.

A remark request can be cancelled by the student before submitting or before the instructor completes the remarking. A count for the number of outstanding remark requests of an assignment is stored as `Assignment.outstanding_remark_request_count`.

The following table shows the results of several relevant function calls at different stage of the lifecycle of remark requests.

  -------------------------------------------------------------------------
                     No       Remark     Remark      Remark     Remark
                     remark   request    request     request    request
                     request  created    submitted   marked     cancelled
  ------------------ -------- ---------- ----------- ---------- -----------
  (remark)           N/A      `unmarked` `partial`   `complete` (unchanged)
  Result.marking\_st                                            
  ate                                                           

  Submission.remark\ `nil`    (remark)                          `nil`
  _result\_id                 `Result.id                        
                              `                                 

  Submission.has\_re `false`  `true`                            `false`
  mark?                                                         

  Submission.remark\ `false`             `true`                 `false`
  _submitted?                                                   

  Submission.get\_la original            remark                 original
  test\_result       Result              Result                 Result

  Submission.get\_la original `nil`)                 remark     original
  test\_completed\_r Result                          Result     Result
  esult              (or                                        
  -------------------------------------------------------------------------

Note that currently, cancelling a remark request does not delete the remark result that’s been created, and [Alysha](https://github.com/akwok18) has proposed to change this behaviour in [issue 1017](https://github.com/MarkUsProject/Markus/issues/1017).