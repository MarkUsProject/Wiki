================================================================================
Welcome to MarkUs
================================================================================

MarkUs (pronounced "mark us") is an open-source tool which recreates the ease
and flexibility of grading assignments with pen on paper, within a web
application.  Designed to support programming assignments, students may submit
their work through the web interface or through a version control system.  Students
can form groups to collaborate on assignments.  Instructors add marking schemes, 
assign graders to students and then later release the marks to the students.  The
graders can view the students submissions, fill in the marking schemes, and annotate
the students work.  MarkUs facilitates much of the administrative work of setting
up and grading an assignment.

MarkUs is written using Ruby on Rails, and uses Subversion to store the student submissions. MarkUs
is the successor to the OLM project, an earlier grading tool.


MarkUs Users
================================================================================
* `User Guide <UserGuide>`__

  * `Instructor Guide <Doc_Admin>`__
  * `Grader Guide <Doc_Grader>`__
  * `Student Guide <Doc_Student>`__
  * `RESTful API <RESTfulApiDocumentation>`__

Users may also find the `Sandbox <http://www.markusproject.org/admin-demo>`__ useful.



Developer Resources
================================================================================
If you are interested in contributing to MarkUs, there is lots of information below to help you get set up.  Please help us keep this documentation up to date!

The `Developer's Blog <http://blog.markusproject.org>`__ has quite a few useful posts to help you get started or understand design decisions that were made along the way.  It also includes status reports from various terms.

Project communication:

* IRC Channel: #markus on irc.freenode.net 
    * `Logs of the channel <http://www.markusproject.org/irc/>`__ are also available.
* Mailing list address: markus-dev@cs.toronto.edu
    * Mailing list archive at `marc.info <http://marc.info/?l=markus-dev&r=1&w=2>`__

Most of the low-level technical discussion takes place on the `Issue list <https://github.com/MarkUsProject/Markus/issues>` and in `Code Reviews <https://github.com/MarkUsProject/Markus/pulls>`__ . There is a `Guide to creating MarkUs Code Reviews <HowToCodeReview>`__


* Git Resources:

  * `MarkUs and Git <GitHowTo>`__
  * `A Git Tutorial <http://library.edgecase.com/git_immersion/index.html>`__
  * `Progit Book <http://progit.org/book>`__
  * `Gitref.org <http://gitref.org>`__
  * `Git Backend Wiki <GitBackEnd>`__

* `Issue Labels <LabelsWhatTheyMean>`__

* `Updating the Wiki <WikiUpdate>` --

.. TODO Modify User Guide link



MarkUs Developer Installation Guides
================================================================================
GNU/Linux
--------------------------------------------------------------------------------
* `Setting up a development environment on GNU/Linux <InstallationGnuLinux>`__

Mac OS X
--------------------------------------------------------------------------------
* `Setting up a development environment on Mac <InstallationMacOsX.rst>`__

Windows
--------------------------------------------------------------------------------
**NOTE: Windows is not supported anymore**

* `Setting up a development environment on Windows using
  InstantRails <InstallationWindows.rst>`__ (DEPRECATED)

Setting up the Database
--------------------------------------------------------------------------------

* `SQLite <SettingUpSQLite.rst>`__
* `MySQL <SettingUpMySQL.rst>`__
* `PostgreSQL <SettingUpPostgreSQL.rst>`__


MarkUs Developer Documentation
================================================================================

Project Vitals
--------------------------------------------------------------------------------

Repository: Create a GitHub account and fork MarkUsProject/MarkUs (see GitHub
help for more info).



Screencasts
--------------------------------------------------------------------------------

* `Student File Submission: September 2 2009 
  <http://www.youtube.com/watch?v=ofpyaty20FQ>`__
* `Student Group Formation: August 17, 2009
  <http://www.youtube.com/watch?v=Ed_z_tHCAg8>`__
* `The Grader View: June 6, 2009
  <http://www.cs.toronto.edu/~reid/screencasts/OLM-2009-06-03.swf>`__
* `Flexible Marking Scheme Selection: December 1, 2009
  <http://www.youtube.com/watch?v=x4mbE3WBgog>`__
* `Flexible Marking Scheme Criterion: December 1, 2009
  <http://www.youtube.com/watch?v=tVkti9y91RA>`__
* `Notes created through the Modal dialog as an Admin: December 3, 2009
  <http://www.youtube.com/watch?v=eoxriy2cYW0>`__
* `Notes created through the Modal dialog as a TA: December 3, 2009
  <http://www.youtube.com/watch?v=J4r18LNDwPs>`__
* `Creating and editing a grade entry form as an admin: December 4, 2009
  <http://www.youtube.com/watch?v=r7UnaNYe2rw>`__
* `Notes tab: December 11, 2009
  <http://www.youtube.com/watch?v=IcuG6AlJfvQ>`__
* `Entering and releasing the marks for a grade entry form as an admin: April
  4, 2010 <http://www.youtube.com/watch?v=-v6eVy94pdI>`__

Project Contributors
--------------------------------------------------------------------------------
Aaron Lee, Adam Goucher, Aimen Khan, Alexander Kittelberger, Alexandre Lissy, Alex Krassikov, Alysha Kwok, Amanda Manarin, Andrew Hernandez, Andrew Louis, Angelo Maralit, Anthony Le Jallé, Anton Braverman, David Das, Arianne Dee, Benjamin Thorent, Benjamin Vialle, Bertan Guven, Brian Xu, Bryan Shen, Camille Guérin, Catherine Fawcett, Christian Jacques, Christine Yu, Christopher Jonathan, Clément Delafargue, Clément Schiano, Danesh Dadachanji, Daniel St. Jules, Daniyal Liaqat, Daryn Lam, David Liu, Diane Tam, Dina Sabie, Dylan Runkel, Ealona Shmoel, Egor Philippov, Erik Traikov, Eugene Cheung, Evan Browning, Farah Juma, Fernando Garces, Gabriel Roy-Lortie, Gillian Chesnais, Geoffrey Flores, Hanson Wu, Horatiu Halmaghi, Ian Smith, Ibrahim Shahin, Jay Parekh, Jeffrey Ling, Jeremy Merkur, Jeremy Winter, Jérôme Gazel, Jiahui Xu, Jordan Saleh, Joseph Mate, Joseph Maté, Justin Foong, Karel Kahula, Kitiya Srisukvatananan, Kurtis Schmidt, Lawrence Wu, Luke Kysow, Marc Bodmer, Marc Palermo, Mark Rada, Mélanie Gaudet, Michael Lumbroso, Mike Conley, Mike Gunderloy, Mike Stewart, Mike Wu, Misa Sakamoto, Neha Kumar, Nelle Varoquaux, Nicholas Maraston, Nicolas Bouillon, Nick Lee, Nicolas Carougeau, Noé Bedetti, Oloruntobi Ogunbiyi, Ope Akanji, Rafael Padilha, Razvan Vlaicu, Robert Burke, Samuel Gougeon, Sean Budning, Severin Gehwolf, Shenglong Gao, Shion Kashimura, Simon Lavigne-Giroux, Su Zhang, Tara Clark, Tiago Chedraoui Silva, Tianhai Hu, Valentin Roger, Veronica Wong, Victoria Mui, Victor Ivri, Vivien Suen, William Roy, Xiang Yu, Yansong Zang, Zachary Munro-Cape

**Supervisors:** Karen Reid, Morgan Magnin, Benjamin Vialle, David Liu


Term Work
--------------------------------------------------------------------------------

Status Reports:

* `2013 <http://blog.markusproject.org/?m=2013&cat=73>`__

* `2012 <http://blog.markusproject.org/?m=2012&cat=73>`__

* `2011 <http://blog.markusproject.org/?m=2011&cat=73>`__

* `2010 <http://blog.markusproject.org/?m=2010&cat=73>`__

* `2009 <http://blog.markusproject.org/?m=2009&cat=73>`__

Everything a Developer Needs to Know about Ruby, Ruby on Rails and MarkUs
--------------------------------------------------------------------------------

* **Getting Started with Ruby, Ruby on Rails and MarkUs**

  * `Short Rails Debugging HOWTO <RailsDebugging.rst>`__
  * `How to program in Ruby, Rubybook <http://ruby-doc.org/docs/ProgrammingRuby/>`__
  * `Rails 3.0 API <http://railsapi.com/doc/rails-v3.0.8rc1/>`__
  * `Rails 3.2 Guides <http://guides.rubyonrails.org/v3.2.13/>`__
  * `General Guide Lines to code - Code review from Mike Gunderloy 
    <GeneralGuideLines.rst>`__
  * http://apidock.com/rails
  * `Some notes from a Ruby book taken by Tara Clark
    <http://taraclark.wordpress.com/category/ruby-on-rails>`__
  * `How to use MarkUs Testing Framework <TestFramework.rst>`__ (still in alpha)


* **MarkUs Coding Style/Coding Practices/Rails Gotchas**

  * `Basic Guidelines for MarkUs Development <DeveloperGuidelines.rst>`__ (**IMPORTANT!**)
  * `How To Do a Code Review <HowToCodeReview.rst>`__
  * `Rails erb quirks <RailsERbStyle.rst>`__
  * **Please document your code according to the RDoc specification** (see
    `How to Use RDOC <http://rdoc.sourceforge.net/doc/>`__)
  * `Difference between COUNT, LENGTH, and SIZE <http://blog.hasmanythrough.com/2008/2/27/count-length-size>`__
  * `Our Ruby/Rails testing guidelines <TestingGuidelines.rst>`__
  * `Security testing guidelines <SecurityTesting.rst>`__
  * `Internationalization <Internationalization.rst>`__

* **MarkUs API/Test Coverage**

  * `MarkUs Ruby Doc <http://www.markusproject.org/dev/app_doc>`__
  * `MarkUs Test Coverage <http://www.markusproject.org/dev/test_coverage>`__

* **MarkUs Releases**

  * `Preparing a Release and Patch <PreparingReleaseAndPatch.rst>`__

* **User Roles and Stories for MarkUs**

  * General / Constraints

    * `MarkUs is internationalized <GeneralUseCase_Internationalized.rst>`__
    * `MarkUs is configurable <GeneralUseCase_Configurable.rst>`__
    * `Rubrics are not allowed to change once Submissions have been
      collected <GeneralUseCase_NoRubricChangesAfterCollection.rst>`__

    * `Instructor <Role_Instructor.rst>`__

      * `Instructors can create / edit assignments <Instructor_CreateEditAssignments.rst>`__
      * `Instructors can download / export files <Instructor_DownloadExportFiles.rst>`__
      * `Instructors can hide students <Instructor_HideStudents.rst>`__
      * `Instructors can do everything that Graders can do <Instructor_CanDoWhatGradersDo.rst>`__
      * `Instructors can release / unrelease completed marking results 
        <Instructor_ReleaseMarkingResults.rst>`__
      * `Instructors can map particular students / groups to Grader_(s) for marking 
        <Instructor_MapGradersToGroupings.rst>`__
      * `Instructors can download / export a file that describes the Student /Grouping mapping to Graders 
        <Instructor_DownloadMapGradersToGroupings.rst>`__
      * `Instructors can upload a file that will do the Student /Grouping mapping to Graders 
        <Instructor_UploadMapGradersToGroupings.rst>`__
      * `Instructors can manage groups without restrictions 
        <Instructor_ManageGroupsWithoutRestrictions.rst>`__

    * `Grader <Role_Grader.rst>`__

      * `Graders can easily tell which submissions are assigned to them to mark 
        <Grader_EasyToSeeWhatToMark.rst>`__
      * `Graders can view a Submission from a Student  / Grouping 
        <Grader_ViewSubmissions.rst>`__
      * `Graders can view / annotate / mark a particular file from a Submission 
        <Grader_ViewAnnotateMarkParticularFile.rst>`__
      * `Graders can add annotations to particular lines of code within a Submission File 
        <Grader_AnnotateLinesOfCode.rst>`__
      * `Graders can create reusable Annotations <Grader_CreateReusableAnnotations.rst>`__
      * `Graders can create short, formatted overall comments on a Submission 
        <Grader_CreateOverallComment.rst>`__
      * `Graders can view and use a Rubric for marking a Submission for an Assignment 
        <Grader_ViewUseRubric.rst>`__
      * `Graders can view a summary of marked submissions 
        <Grader_ViewSummaryOfMarkedSubmissions.rst>`__
      * `Graders can add bonuses / penalties to submissions 
        <Grader_AddBonusesPenalties.rst>`__
      * `Graders can modify the marking state of a submission result 
        <Grader_CanModifyMarkingStatus.rst>`__
      * `Graders can easily switch to the next / previous Submission for marking 
        <Grader_CanSwitchToNextSubmission.rst>`__

    * `Student <Role_Student.rst>`__

      * `Students can view marks of submissions <Student_ViewMarks.rst>`__
      * `Students can view annotations of marked submissions/assignments <Student_ViewAnnotations.rst>`__
      * `Students can submit files for their assignments <Student_SubmitFiles.rst>`__
      * `Students can view / edit submission files for assignments <Student_ViewEditFiles.rst>`__

* **Database Schema**

  * AutoGenerate Database Schema

    * `View Schema Diagram <images/database_20101001.png>`__

  * `Questions and Answers (Old Document) <SchemaQuestions.rst>`__

* **MarkUs Component Descriptions**

  * `Group / Grouping Behaviour <GroupsGrouping.rst>`__
  * `Groupings and Repositories <GroupsGroupingsRepositories.rst>`__
  * `Authentication and Authorization <Authentication.rst>`__
  * `Annotations <Annotations.rst>`__
  * `How Student Work is Graded and Re-graded  <HowGradingWorks.rst>`__
  * `Submission Rules <SubmissionRules.rst>`__
  * `The FilterTable Class <FilterTable.rst>`__
  * `Simple Grade Entry <SimpleGradeEntry.rst>`__
  * `Notes System <NotesSystem.rst>`__

* **Feedback Notes**

  * `2009-05-22: Phyliss <PhylissFeedback.rst>`__
  * `2009-06-22: Ryan <RyanFeedback.rst>`__

* **Tips and Trick**

  * `Dropping/Rebuilding Database Quickly and Easily <DropAndRebuildDb.rst>`__

* **IDE/Editor Notes**

  * `jEdit <JEdit.rst>`__
  * `NetBeans <NetBeans.rst>`__
  * `Aptana RadRails / Eclipse <AptanaRadRails.rst>`__

MarkUs Deployment Documents
================================================================================

Installation Instructions for MarkUs using RAILS_ENV=production
--------------------------------------------------------------------------------

* `Setup Instructions for MarkUs Stable (MarkUs 0.10.0) <InstallProdStable.rst>`__
* `Hosting several MarkUs applications on one machine (for Production) <MultipleHosting.rst>`__
* `How to use LDAP with MarkUs <LDAP.rst>`__
* `How to use Phusion Passenger instead of Mongrel <ApachePassenger.rst>`__

* `Old Setup Instructions for MarkUs Stable (MarkUs 0.5, 0.6, 0.7 and 0.8 branches) <InstallProdOld.rst>`__

For a complete list of local wiki pages, see `TitleIndex <http://github.com/MarkUsProject/Markus/wiki/_pages>`__.
