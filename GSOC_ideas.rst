================================================================================
Google Summer of Code Ideas
================================================================================
MarkUs is a web application for grading programming assignments.  The main project page is http://markusproject.org.  There is a demo instance of MarkUs running there that you can play with.  Login with user id "a" and any non-empty password.

Here is the summary of all the ideas we have for GSoC

.. contents::

Performance analysis 
================================================================================

As we begin to use MarkUs is classes larger than 500 students, we need to get a better picture of the performance limitations, and what we can do to mitigate them. If a large number of students try to submit their assignments using MarkUs in a short time window, where are the performance bottlenecks? Rails? The database? Subversion?

This project would involve setting up a test environment, profiling, and stress testing MarkUs. An applicant should have enough Linux knowledge to be able to set up concurrent tests and measure performance, and enough database knowledge to be able to do some profiling. Basic Ruby and Rails knowledge, web application knowledge would be a strong asset. (We realize this is a lot to ask, but for the right student, this could be a really rewarding project.)

Integrate Git, Bazaar and Merkurial into MarkUs
================================================================================

MarkUs can be configured either to allow students to submit code through a
web interface, or to provide students an svn repository. As DCVS
becomes more and more popular, students often use svn2X tools in order to
use DCVS, hosting the code on hosting platforms such as Github, Launchpad or
Bitbucket. The idea would be to create an interface for MarkUs to retrieve
automatically the code to be graded (student providing a URL to their clone of the repository).

Requirements for this project are good familiarity with at least one DCVS, and preferably some experience with Ruby to explore the library bindings.

Web based PDF annotations
================================================================================

Markus has a web-based PDF annotations module that uses ImageMagick (http://www.imagemagick.org) to convert a PDF file into an image, in order to be able to annotate it. There are several limitations to this approach: the PDF document is limited to 30pages, substantial processing time is required to perform the conversion, documents cannot easily be viewed in different sizes. This project is of an exploratory nature, to find a better solution to PDF annotations.


Mapping Graders to Groups
================================================================================

The facility in MarkUs that maps Graders to the groups that they are responsible for marking currently provides only very simple mapping functions. Professors can either assign graders randomly to groups, or can upload a specific mapping. We have had requests to implement more sophisticated mapping ability.  For example:

- Map graders favoring students previously graded by these graders
- Map graders favoring students NOT graded by these graders
- Map graders to all students of one section
- Map graders randomly to all students of a section

This project will require Ruby and Rails skills. It will involve some interesting UI work, but should be a fairly straightforward project. A student who chooses this project will also end up working on several other small projects.

Integrated documentation system
================================================================================

As the user base for MarkUs grows, the need for better documentation becomes clear. It will be an interesting software design problem to create an integrated documentation system that tracks versions and configurations.

This project requires some Ruby/Rails knowledge and a desire to create simple, elegant software.

A VM harness for the automated test framework
================================================================================

We have been working towards an automated test framework that allows students to submit their work and receive immediate feedback. To run student submitted code on a server, we need to think carefully about how to do this securely. Running the tests inside a locked down VM seems to be the most promising solution. 

This project requires Linux and VM knowledge, preferably with some system administration skills.



Integrated Microsoft docx/Open Document Format Support
================================================================================

Convert Microsoft's docx or Open Document suite of formats (such as ODT,ODF and ODP) into a reasonable format which can be annotated. Suggested format conversions could be properly styled HTML, images, or plain text. For HTML and plain text conversion use of XSL-T stylesheets could be used to leverage XSL-T translation support of modern Web browsers.

Migrating MarkUs to Rails3 (and eventually to Ruby 1.9)
================================================================================

Ruby on Rails version 3 is the new major release of Ruby on Rails. MarkUs is now three years old and migrating to Ruby on Rails is a long work. The work has already started (see branch 'rails_3_migration' on http://github.com ). Moreover, Ruby 1.9.2 is the new implementation of the Ruby language. MarkUs and Ruby on Rails 2 are based on Ruby 1.8.

MarkUs tests (units and functionals) will need some updates. Some gems used are deprecated and will not work anymore.

Moving to new Ruby and Ruby on Rails 3 is a step to have a more and more professional web application.

This project requires good Ruby and Ruby on Rails skills, with deployment abilities (MarkUs must still be deployable once the migration will be done)

Develop a plug-in integrating Markus core to an existing e-learning whole platform
================================================================================

E-learning platforms have become a keystone to educational environment. Free software-based platforms arise and are now widely used (at least in Europe). The major two software are Claroline and Moodle. For every course, it provides features like publishing documents w.r.t. courses, manage public and private forums, create groups of students, prepare online exercises, … The MarkUs tool nowadays appears as an additional tool to branch to the existing e-learning environment in many institutions, which could restrain its use. It would be useful, for both teachers and students, to be able to access to MarkUs features through the usual platform they use in their daily tasks. This will result in no differentiation between CS courses and other courses, meaning that everyone would be benefit from the annotation features provided by MarkUs. 

The idea of this proposal is to create a MarkUs plug-in for either Claroline or Moodle. Claroline has the advantage to be currently in use in École Centrale de Nantes (France) and in many french schools. This engineering school collaborates with École Centrale de Lyon, which is part of the consortium leading the Claroline development. 

[1] http://www.claroline.net/
[2] http://moodle.org/