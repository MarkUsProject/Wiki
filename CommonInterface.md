<i class="icon-split"></i>**Criteria: Common Interface**
===================
After an assignment is created, an admin can create one or more criteria for it. These criteria can be of any type (rubric, flexible, checkbox).

> **Note:** We use the term "criteria", to refer to either rubric criteria, flexible criteria or checkbox criteria, which have different settings, but serve the same purpose of guiding the marking for an assignment.

We  have created a common interface for our criteria, so that we can use the same method name to access methods that have the same functionality, but different implementation depending on the type of criterion. These methods are defined under their appropriate criterion class.

----------

<i class="icon-list"> **Methods**
===================
All the methods below are defined in all of the criteria models.

#### **Class methods**
:
-- **load_from_yml(criterion_yml)**
:  &nbsp;&nbsp; Instantiates the appropriate criterion from a YML entry.
:
-- **to_yml(criterion)**
:  &nbsp;&nbsp; Returns a hash containing the information of an appropriate single criterion.
:
-- **symbol**
:  &nbsp;&nbsp; Returns a Ruby symbol for the appropriate criterion class. Eg:
: &nbsp;&nbsp;&nbsp;&nbsp; FlexibleCriterion.symbol  #=>  :flexible
:
> **Note:** These methods are called on a criterion class, and therefore, they are called as follows:
> RubricCriterion.shared_class_method(arguments)
> FlexibleCriterion.shared_class_method(arguments)
> CheckboxCriterion.shared_class_method(arguments)

#### **Instance methods**
:
-- **set_mark_by_criterion(mark_to_change, mark_value)**
:  &nbsp;&nbsp; Sets the mark for a criterion appropriately, depending on its type.
:
> **Note:** This method is called on an instance of a class, and therefore, it is called as follows:
> my_instance = RubricCriterion.create(name: 'My Rubric criterion')
> my_instance.instance_method(parameters)
