# Developer Guide: A technical tour through the autotester.

The autotester is a program designed to automatically run student assignments against preconfigured test files.

## Python Autotester

![Autotest Diagram](images/autotest_diagram.png)

The autotester has three major components:
1. Client - Flask API Client
2. Server - Python Server
3. Cache - Redis Cache

The python autotester has two testers to choose from:
1. Pytest - Third party PyPy project
2. Unittest - Builtin testing framework


### Test Creation

1. Markus
  i. The  `update_test_specs` [function](https://github.com/MarkUsProject/Markus/blob/master/app/controllers/api/assignments_controller.rb#L199) updates several local records with the information provided.
  ii. The `AutotestSpecsJob` which calls the `automated_tests_helper.update_settings` function
  iii. The `update_settings` obtains all the settings information and makes a call to the Autotester Client api, to either update the settings or create a new settings record
2. Autotest Client
  i. The autotest client on a new test creation, will have it's `/settings` [endpoint](https://github.com/MarkUsProject/markus-autotesting/blob/master/client/autotest_client/__init__.py#L213) called which does 2 things. 1. Incremement the `autotest:settings_id` and 2. save the settings provided from MarkUs onto the redis cache as `autotest:settings`

### Test Execution

1. Markus
  i. When a test is scheduled, a resque



