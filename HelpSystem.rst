================================================================================
Help System
================================================================================

What is the Help System?
--------------------------------------------------------------------------------
The Help System is embedded documentation throughout the MarkUs application. It allows for users to toggle help boxes and tool tips with relevant information as they naviage the site.


Current Help System Design
--------------------------------------------------------------------------------
Currently there are 3 possible ways that the documentation can be displayed. 

1. Tool Tips - These are small boxes which will appear next to specific fields in form.

2. Large Boxes - These larger boxes are used for displaying large amounts of text for specific sections.

3. Title Boxes - These are used to explain an entire page under the page title or header.


Adding New Help Content
--------------------------------------------------------------------------------
Code only needs to be added into the view of the page you want to add the help system to.

**Adding Help Icon**

The first step in adding new help content is adding a help icon which the user can click to display the message.

The following code can be inserted into the page you want, creating an active help button for tooltips and large boxes::
 
<div class="help page_section_help"></div>

The following is for Title Boxes, where the code is added within the h1 tag of the title::

 <h1><%= I18n.t("page_title") %><span class="title-help page_section_help"></span></h1>

`"page_section_help" is a place holder for whatever page or section you are adding the button to. For example if you are adding a help button to the assignments properties section, you would use "assignment_properties_help".`

**Including the JavaScript**

The Help System JavaScript code needs to be inculded into the page you are adding the content to. To do this add the following code::

<%= javascript_include_tag 'help_system' %>

**Add the box to the view**

Next is to add the appropraite box. The following code can be added to where you would like the box to show up

    **Tooltip**::
   
    <p class="help-message-tooltip page_section_field" ><%= t("help_text")%></p>
    
    **Large box**::
    
    <p class="help-message-box page_section" ><%=t("help_text")%></p>
    
    **Title box**::
    
    <p class="help-message-title page_help"><%= t("help_text")%></p>

    
    `"page_section_field" and "page_section are place holders for whatever field you want the tooltip or box to appear next to . And "help_text" will be the actual text from the yml file` 
    
**Add Text to lagunage files**

The final step is to add the appropraite text to the language files.
    
   

