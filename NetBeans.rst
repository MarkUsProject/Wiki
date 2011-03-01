================================================================================
Using NetBeans with Rails
================================================================================

To use NetBeans with Rails, install the the Ruby and Rails plugin (from
Tools>Plugins).

It is not recommended to create a NetBeans project out of the existing MarkUs
source.  Use it only to edit individual files with.  Otherwise, NetBeans will
change the configuration files you have carefully set up already, causing many
things to stop working.  If this happens, look in particular inside
config/database.yml and restore any settings that NetBeans has changed.
