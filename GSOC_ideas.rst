================================================================================
Google Summer of Code Ideas
================================================================================


Here is the summary of all the ideas we have for GSoC

.. contents::

Performance analysis 
================================================================================


Integrating Git, bzr and hg to Markus
================================================================================

Markus can be configured either to allow students to submit code throught a
web interface, or to provide students an svn repository. As DCVS are
becoming more and more popular, students often use svn2X tools in order to
use DCVS, hosting the code in plateforms like github, launchpad or
bitbucket. The idea would be to create an interface for Markus to retrieve
automatically the code to be graded (student providing a url).

Web based PDF annotations
================================================================================

Markus has a web based PDF annotations module that used ImageMagick to convert
pdf into an image, in order to be able to annotate it. Unfortunately, the
current implementation limits the pdf being annotated to 30pages: not enough
to annotate a full project reports. The whole process needs to be rethinked
and reimplemented.



