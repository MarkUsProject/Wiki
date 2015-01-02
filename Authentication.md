Authentication
==============

Use Case:

1.  User accesses login page.

    a.  If user has an active session, user is redirected to main page.

2.  User inputs user name and password.

    a.  User is redirected to login page if user has invalid username or password.

    b.  Otherwise, user is directed to main page.

Authentication is handled by `main_controller.rb`.