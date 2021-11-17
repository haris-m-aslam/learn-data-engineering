# Unit Testing for Data Science in Python

## Why unit test?

Unit tests automate the repetitive testing process and saves time.

Unit tests will reduce the testing time over the life cycle of a single function

A function is tested after the first implementation and then any time the function is modified, which happens mainly when new bugs are found, new features are implemented or the code is refactored


## Write a simple unit test using pytest

there are many python libraries for writing unit tests
pytest
unittest
nosetests
doctest

## To start Unit testing

* Create a file called test_row_to_list.py
test_ indicate unit tests inside (naming convention)

Files holding unit tests are also called test modules

In test modules we import

```
import pytest
import row_to_list
```

A unit test is written as a Python function, whose name starts with tests_, 
just like test module. This way, pytest can tell that it is a unit test and not
an ordinary function

The unit test usually corresponds to exactly one entry in the argument and return value table for row_to_list().

The unit test check whether the function has expected return value 
when called on this particular argument

The actual check is done via an assert statement, and every test must contain one.

## Theoretical structure of an assert

The assert statement has a required first argument, which can be any boolean expression

``` assert boolean_expression```

If the expression is True, the assert statement passes, giving us a blank output

If the expression is False, it raises an AssertError.

## Checking for None value

The correct way to check if a variable's value is None is to use the boolean express var is None and not var equals None.

```assert var is None```

## Running unit tests

To test whether the function is working at any time in its life cycle, we simply run the test module.

Standard way to run tests is open a command line and type pytest followed by the test module name.

```pytest test_row_to_list.py```


## write a unit test for function converting number commas to integer

```
import the pytest package#
import pytest

#Import the function convert_to_int()
from preprocessing_helpers import convert_to_int

#Complete the unit test name by adding a prefix
def test_on_string_with_one_comma():
  Complete the assert statement#
  assert convert_to_int("2,081") == 2081
```

Run the unit test#
pytest test_convert_to_int.py
in ipython
!pytest test_convert_to_int.py


## Understanding test result report

The output says "Collected 3 items", which means that pytest found three tests to run.
This is correct the test module contains three unit tests.


The next line contains the test module name, which is test_row_to_list.py, followed by the characters dot, capital F and dot. Each character represents the result of a unit test.

.F. means first test passed second failed third passed

The character capital F stands for failure. A unit test fails if an exception is raised when running the unit test code.

A unit test may also fail if a different exception is raised while running the unit test code. In this case, execution will stop before the assert statement is run. For example, if we wrote None with a small n, this would raise a NameError. Since the assert statement did not run, we cannot conclude anything about the function under test from the capital F result. In this case, we should fix the unit test so that it can actually run the assert statement.

The line raising the exception is marked by a greater than sign on the left.

The following lines marked with capital E on the left contains details of the exception.

The lines which contain the word "where" displays any return values that were calculated when running the assert statement. In this case, it shows that the actual return value of row_to_list() is a list containing an empty string and the string "293,410". The expected return value is, of course, None. The mismatch between the expected and actual value will be our starting point for debugging.

The next section contains detailed information about failed tests


The fine line is a test result summary

If you get an AssertionError, this means the function has a bug and you should fix it. 
If you get another exception, e.g. NameError, this means that something else is wrong 
with the unit test code and you should fix it so that the assert statement can actually run.


## More benefits and test types

The unit tests also serve as documentation

The argument and return value table give good idea about what function does, helping them understand the function's code faster.

Unit tests also increase trust in a package(From package we can check unit tests badge)

Unit tests can also reduce downtime for a productive system.
Suppose we make a mistake and push bad code to a productive system.
and push bad code to a productive system.

We can cure this by setting up Continuous Integration or CI. 
CI runs all unit tests when any code is pushed, 



## Mastering assert statements

## assert syntax

assert boolean_expression

```
assert boolean_expression, message
assert1 == 2, "One is not equal to two!"
```

Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>
AssertionError: One is not equal to two!


But the assert statement can take an optional second argument, called the message.

The message is only printed when the assert statement raises an AssertionError
If the assert statement passes, nothing is printed.

## Adding a message to a unit test


## Without assert message

```
import pytest

def test_for_missing_area():
  assert row_to_list("\t293,410\n") is None
```

## With assert message

```
import pytest

def test_for_missing_area_with_message():
  actual = row_to_list("\t293,410\n")
  expected = None
  message = ("row_to_list('\t293,410\n') "
             "returned {0} instead "
             "of {1}".format(actual, expected)
             )
  assert actual is expected, message
```


## Beware of float return values

0.1 + 0.1 + 0.1 return 0.300000004

assert 0.1 + 0.1 + 0.1 == 0.3 //Don't do this

assert 0.1 + 0.1 + 0.1 == pytest.approx(0.3

In Python, comparisons between floats don't always work as expected, 
as we can see in this surprising example.

Because of the way Python represents floats, the digits on the 
right might be different from what we expect, causing comparisons to fail. 

Instead, we should use the pytest.approx() function to wrap the expected value


## Multiple assertions in one unit test

Unit tests can have more than one assert statement

Unit test will pass if both assert statements pass. 
It will fail if any of them raises an AssertionError.

```
import pytest
...

def test_on_string_with+one_comma():
  return_value = convert_to_int("2,081")
  assert isinstance(return_value, int)
  assert retrun_value = 2081
```

Unit test will pass only if both assertions pass


def test_on_six_rows():
    example_argument = np.array([[2081.0, 314942.0], [1059.0, 186606.0],
                                 [1148.0, 206186.0], [1506.0, 248419.0],
                                 [1210.0, 214114.0], [1697.0, 277794.0]]
                                )
    Fill in with training array's expected number of rows#

    expected_training_array_num_rows = int(.75* example_argument.shape[0])

    Fill in with testing array's expected number of rows#

    expected_testing_array_num_rows = example_argument.shape[0] - expected_training_array_num_rows
    
    actual = split_into_training_and_testing_sets(example_argument)

    Write the assert statement checking training array's number of rows#

    assert actual[0].shape[0] == expected_training_array_num_rows, "The actual number of rows in the training array is not {}".format(expected_training_array_num_rows)

    Write the assert statement checking testing array's number of rows#

    assert actual[1].shape[0] == expected_testing_array_num_rows, "The actual number of rows in the testing array is not {}".format(expected_testing_array_num_rows)



## Testing for exceptions instead of return values

Some functions may not return anything, but rather raise an exception

Consider the split_into_training_and_testing_sets() function 
that you tested in the last exercise. 
This function returns a two-tuple containing the training and the testing array.
It puts 75% of the rows of the argument NumPy array into the training array, 
and the rest of the rows into the testing array.

This function expects the argument array to have rows and columns, 
that is, the argument array must be two dimensional. 
Otherwise, splitting by rows is undefined.

So if we pass a one dimensional array to this function, 
it should not return anything, but rather raise a ValueError, 
which is a specific type of exception.

## Unit testing exceptions

Any code that is inside the with statement is known as the context.

The with statement takes a single argument, which is known as a context manager.

The context manager runs some code before entering and exiting the context
just like a security guard who checks or does something 
when we enter or leave a building.

To check exeception raises or not
In Unit tests, we are using a context manager called pytest.raises().
If the code in the context raises a ValueError, 
the context manager silences the error. -- Unit test passes
And if the code in the context does not raise a ValueError, 
the context manager raises an exception itself. -- Unit test fails


Getting back to the unit test, we call the function on the one dimensional array
 inside the context. If the function raises a ValueError as expected, 
it will be silenced and the test will pass. 

Getting back to the unit test, we call the function on the one dimensional array
 inside the context. If the function raises a ValueError as expected, 
it will be silenced and the test will pass. 

```
def test_valueerror_on_one_dimensional_argument():
    example_argument = np.array([2081, 314942, 1059, 186606, 1148, 206186])
    with pytest.raises(ValueError):        
       split_into_training_and_testing_sets(example_argument)
```

If functionraises expected ValueError, testwill pass.

If function is buggy and does not raise ValueError, test will fail.






## Testing the error message

We can unit test details of the raised exception as well. 
For example, we might want to check if the raised ValueError 
contains the correct error message which starts with 
"Argument data array must be two dimensional".

In order to do that, we extend the 'with' statement with the as clause. 
If the ValueError is raised within the context, 
then exception_info will contain information about the silenced ValueError.

After the context ends, we can check whether exception_info has the correct message.
To do this, we use a simple assert statement with the match() method of 
exception_info. The match method takes a string as argument and checks 
if the string is present in the error message.

```
def test_valueerror_on_one_dimensional_argument():
    example_argument = np.array([2081, 314942, 1059, 186606, 1148, 206186])
    with pytest.raises(ValueError) as exception_info:    # store the exception        
       split_into_training_and_testing_sets(example_argument)
    
    assert exception_info.match("Argument data array must be two dimensional. "
           "Got 1 dimensional array instead!"
            )

```

exception_info stores the ValueError

exception_info.match(expected_msg) checks if expected_msg 
is present in the actual error message.


```
import pytest

with pytest.raises(ValueError) as exc_info:
    raise ValueError("Silence me!")
   Check if the raised ValueError contains the correct message#
assert exc_info.match("Silence me!")

```


```
import numpy as np
import pytest
from train import split_into_training_and_testing_sets

def test_on_one_row():
    test_argument = np.array([[1382.0, 390167.0]])
    Fill in with a context manager for checking ValueError#
    with pytest.raises(ValueError) as exc_info:
      split_into_training_and_testing_sets(test_argument)
    expected_error_msg = "Argument data_array must have at least 2 rows, it actually has just 1"
    # Check if the raised ValueError contains the correct message
    assert exc_info.match(expected_error_msg)

```
!pytest test_split_into_training_and_testing_sets.py




## The well tested function

The best practice is to pick a few from each of the following categories 
of arguments, which are called 
bad arguments, 
special arguments and 
normal arguments

Bad arguments are arguments for which the function raises an exception 
instead of returning a value

Next comes special arguments. These are of two types: boundary values 
and argument values for which the function uses a 
special logic to produce the return value.

So what are boundary values? If we look at the number of rows of the argument, 
we see that the function raises a ValueError for one row, but returns training 
and testing array for arguments having more than one row.

The value two marks the boundary for this behavior change, and therefore, 
is a boundary value.


## Test Driven Development (TDD)

write unit tests before function implementation

Test Driven Development alters the usual life cycle by adding a single step 
before implementation. This step involves writing unit tests for the function.

When we write unit tests first, we have to think of possible arguments 
and return values - which includes normal, special and bad arguments. 
This type of thinking before implementation actually helps in finalizing 
the requirements for a function


Normal arguments for convert_to_int() are integer strings with comma as 
thousand separators. Since the best practice is to test a function for two 
to three normal arguments, here are three examples with no comma, one comma 
and two commas respectively.

Special arguments are string with missing comma in thousand place, or 
argument string with misplaced comma, argument string with multiple commas

Yes! In TDD, the first run of the tests always fails with a 
NameError or ImportError because the function does not exist yet. 




## How to organize a growing set of tests?

The developers of pytest recommend that we create a directory called tests 
at the same level as src. This directory is also called the test suite.


Inside this folder, we simply mirror the inner structure of src and 
create empty packages called data, features and models respectively.

The general rule is that for each python module my_module.py, 
there should be a corresponding test module called test_my_module.py. 
For example, for the module preprocessing_helpers.py, we create a test module 
called test_preprocessing_helpers.py.

pytest solves this problem using a construct called the test class.

A test class is just a simple container for tests of a specific function.

To declare a test class, we start with the class keyword

The name of the class should be in CamelCase, and 
should always start with “Test”. The best way to name a test class 
is to follow the “Test” with the name of the function, 
for example, TestRowToList.

``` class TestRowToList(object):```

For the other function convert_to_int(), we create another test 
class TestConvertToInt, and put the tests for convert_to_int() inside that class.

``` class TestConvertToInt(object):```


```

import pytest
import numpy as np

from models.train import split_into_training_and_testing_sets

# Declare the test class
class TestSplitIntoTrainingAndTestingSets(object):
    Fill in with the correct mandatory argument #
    def test_on_one_row(self):
        test_argument = np.array([[1382.0, 390167.0]])
        with pytest.raises(ValueError) as exc_info:
            split_into_training_and_testing_sets(test_argument)
        expected_error_msg = "Argument data_array must have at least 2 rows, it actually has just 1"
        assert exc_info.match(expected_error_msg)

```


Finally, anything that is not a bad or special argument is a normal argument.





## Mastering test execution

##  Running all tests

pytest provides an easy way to run all tests contained in the tests folder.

We simply change to the tests directory and run the command pytest.

```
cd test
pytest
```

This command automatically discovers tests by recursing into the subtree of the working directory.

In real life, the !pytest or !pytest -x command is often used in CI servers.

```
cd test
pytest -x
```

Adding the -x flag to the pytest command can save time and resources. This flag makes pytest stop after the first failing test


## Running tests in a test module

we would only want to run a subset of tests. For example, 
we might want to just run tests contained in a particular test module, say, test_preprocessing_helpers.py. 

``` pytest test_preprocessing_helpers.py```


## Running only a particular test class

For example, when we are working on a particular function, say, row_to_list(),


``` pytest data/test_preprocessing_helpers.py::TestRowToList```

## Running tests using node ID

``` pytest data/test_preprocessing_helpers.py::TestRowToList::test_on_one_tab_with_missing_value ```

## Node ID
During automatic test discovery, pytest assigns a node ID to every test class and unit test that it encounters. 
The node ID of a test class is the path to the test module followed by the name of the test class, separated by two colons.
The node ID of a unit test follows the same format, with the unit test name added to the end using another double colon separator.

NodeID of a testclass: <path to test module>::<test class name>
NodeID of an unittest: <path to test module>::<test class name>::<unit test name>


## Running tests using keyword expressions

``` pytest -k "pattern" ```


``` pytest -k "TestSplitIntoTrainingAndTestingSets" ```
this will run only the 2 tests within that test class


## Supports Python logical operators

``` pytest -k "TestSplit and not test_on_one_row" ```
the following command will execute all tests in TestSplitIntoTrainingAndTestingSets except the unit test test_on_one_row()



## Expected failures and conditional skipping

If we follow TDD, we first implement unit tests before function implementation. If we run pytest, it will fail.

It would be nice to have a way to tell pytest that we expect this test to fail

We do that by using the xfail decorator. The decorator goes on top of a test, and it starts with the character @.

xfail: marking tests as "expected to fail"

if we run pytest again, we see that one test is xfailed. But there are no reported errors

```
import pytest 

class TestTrainModel(object): 
  @pytest.mark.xfail
  def  test_on_linear_data(self):
```


## Expected failures, but conditionally

At other times, we might know that the test fails only under certain conditions, 
and we don't want to be warned about them. 
Common situations are when some function won't work under a particular Python version or a particular platform.


Tests that are expected to fail
on certain Python versions.
on certain platforms like Windows

## skipif: skip tests conditionally

```
import sys

class TestConvertToInt(object):

    @pytest.mark.skipif(sys.version_info > (2, 7))
    def test_with_no_comma(self):

```


```
import sys

class TestConvertToInt(object):

    @pytest.mark.skipif(sys.version_info > (2, 7))
    def test_with_no_comma(self):
       """Only runs on Python 2.7 or lower"""
       test_argument = "756"
       expected = 756
       actual = convert_to_int(test_argument)
       message = unicode("Expected: 2081, Actual: {0}".format(actual))
       assert actual == expected, message

```


## The reason argument

Showing reason in the test result report

``` pytest -r ```

## Showing reason for skipping

``` pytest -rs ```


## Showing reason for xfail

``` pytest -rx ```

## Showing reason for both skipped and xfail

``` pytest -rsx ```


## Skipping/xfailing entire test classes

```
@pytest.mark.xfail(reason="“Using TDD, train_model() is not implemented")
class TestTrainModel(object):
     ...

@pytest.mark.skipif(sys.version_info > (2, 7), reason="requires Python 2.7")
class TestConvertToInt(object):
    ...
```


# Add a reason for the expected failure
@pytest.mark.xfail(reason="Using TDD, model_test() has not yet been implemented")


## Optional reason argument to xfail

The xfail decorator also takes an optional 'reason' argument.

```
import pytest 

class TestTrainModel(object):

    @pytest.mark.xfail(reason="“Using TDD, train_model() is not implemented")
    deftest_on_linear_data(self):    
        ...
```



## Continuous integration and code coverage


## The build status badge

Build passing = Stable project

Build failing = Unstable project

This badge uses a Continuous Integration server, 
which runs all tests automatically whenever we push a commit to GitHub. 
It shows whether tests are currently passing or failing.

We will use Travis CI as our CI server.


To integrate with Travis CI, we have to create a settings file called .travis.yml at the root of our repository.

repository root
|-- src
|-- tests
|--.travis.yml

* Step1: Create a configuration file

Contents of .travis.yml
```
language: python
python:
  - "3.6"
install:  
  - pip install -e .
script:
  - pytest tests
```

The install setting is a list of commands to install our project and dependencies in the CI server.

The script section lists the commands necessary to run the tests once everything is installed.

* Step2: Push the file to GitHub
```
git add .travis.yml
git push origin master
```

We push this settings file to GitHub.

* Step3: Install the Travis CI app

Now we go to the GitHub profile page and click on MarketPlace.

We search for Travis CI, click on it

We search for Travis CI, click on it

From now on, whenever we push a commit to the GitHub repo, we should see a build appearing in the Travis CI dashboard.

Every commit leads to a build

* Step 4: Showing the build status badge

When the build finishes, the badge appears here. We click on the badge,

choose Markdown from the dropdown

and paste the markdown code in the README file on GitHub. This adds the badge to the GitHub repo.



## Codecoverage

This badge comes from a service called Codecov that integrates seamlessly with GitHub and Travis CI.

* Step1: Modify the Travis CI configuration file

```
language: python
python:
  - "3.6"
install:
  - pip install -e .
  - pip install pytest-cov codecov    # Install packages for code coverage report
script:
  - pytest --cov=src tests            # Point to the source directory
after_success:
  - codecov                           # uploads report to codecov.io

```

In the install setting, pip install pytest-cov and codecov, as they are necessary to generate and upload coverage reports.

The usual pytest command to run the tests should be modified by 
adding a command line flag --cov which points to the application directory src. 
This new command will not only run the tests, but also produce a coverage report

Finally, add a setting called after_success and add the command codecov. 
This makes Travis CI push the code coverage results to Codecov after every build.

* Step2 : Install Codecov

To enable Codecov for our repository, we install the Codecov app in the GitHub marketplace in the same way we installed Travis CI.

To enable Codecov for our repository, we install the Codecov app in the GitHub marketplace in the same way we installed Travis CI.

* Step3: Showing the badge in GitHub

To enable Codecov for our repository, we install the Codecov app in the GitHub marketplace in the same way we installed Travis CI.


## Testing Models, Plots and Much More


## Beyond assertion: setup and teardown

As an example, consider the function preprocess(), which accepts paths to a raw data file and a clean file as arguments.
preprocess() is different from other functions because it has a precondition to work properly. 
The precondition is the presence of a raw data file in the environment.

Another difference is that when we call the function, it modifies the environment by creating a clean data file.

We create a test called test_on_raw_data() for this function.

We create the raw data file first. This step is called 'setup', and 
it is used to bring the environment to a state where testing can begin.


Then we call the preprocess(), which creates the clean data file. 
We open that file, read it and assert that it contains the expected lines.

Afterwards, we need to remove the clean and raw data file so that 
the next run of the test gets a clean environment. This step is called 'teardown'


To summarize, instead of a sequence of assert statements, 
we have to follow the workflow: setup, assert and teardown.

In pytest, the setup and teardown is placed outside the test, in a function called a fixture. 
A fixture is a function which has the 'pytest.fixture' decorator.

The first section is the setup. Then the function returns the data that the test needs. 
The test can access this data by calling the fixture passed as an argument

But instead of using the return keyword, the fixture function actually uses the yield keyword instead. 
The next section is the teardown. This section runs only when the test has finished executing.


## Fixture

```
@pytest.fixture
def raw_and_clean_data_file():
    raw_data_file_path = "raw.txt"
    clean_data_file_path = "clean.txt"
    with open(raw_data_file_path, "w") as f:
        f.write("1,801\t201,411\n"
                "1,767565,112\n"
                "2,002\t333,209\n"
                "1990\t782,911\n"
                "1,285\t389129\n"
                )
        yield raw_data_file_path, clean_data_file_path
        os.remove(raw_data_file_path)
        os.remove(clean_data_file_path)
```

## Test

```
import os
import pytest
def test_on_raw_data(raw_and_clean_data_file):
    raw_path, clean_path = raw_and_clean_data_file
    preprocess(raw_path, clean_path)
    with open(clean_data_file_path) as f:
        lines = f.readlines()
    first_line = lines[0]
    assert first_line == "1801\t201411\n"
    second_line = lines[1]
    assert second_line == "2002\t333209\n"

```

raw_and_clean_data_file is the fixture which passed as argument to test function
preprocess is the actual function to be tested


## Fixture

```
import pytest

@pytest.fixture
def my_fixture():
  # Do setup here
  return data

def test_something(my_fixture):
    ...    
    data = my_fixture
    ...
```


## The built-in tmpdir fixture

There is a built-in pytest fixture called tmpdir, which is useful when dealing with files. 
This fixture creates a temporary directory during setup and deletes the temporary directory during teardown.


We can pass this fixture as an argument to our fixture. 
This is called fixture chaining, which results in the setup of tmpdir to be called first, followed by the setup of our fixture. 
When the test finishes, the teardown of our fixture is called first, followed by the teardown of tmpdir.

The tmpdir argument supports all os.path commands such as join. 
We use the join function of tmpdir to create the raw and clean data file inside the temporary directory. 

order will the setup and teardown of empty_file() and tmpdir be executed

setup of tmpdir -> setup of empty_file() -> teardown of empty_file() -> teardown of tmpdir.

```
@pytest.fixture
def raw_and_clean_data_file(tmpdir):
    raw_data_file_path = tmpdir.join("raw.txt")
    clean_data_file_path = tmpdir.join("clean.txt")
    with open(raw_data_file_path, "w") as f:
        f.write("1,801\t201,411\n"
                "1,767565,112\n"
                "2,002\t333,209\n"
                "1990\t782,911\n"
                "1,285\t389129\n"
                )
    yield raw_data_file_path, clean_data_file_path
    # No teardown code necessary





## Use a fixture for a clean data file

```
# Add a decorator to make this function a fixture
@pytest.fixture
def clean_data_file():
    file_path = "clean_data_file.txt"
    with open(file_path, "w") as f:
        f.write("201\t305671\n7892\t298140\n501\t738293\n")
    yield file_path
    os.remove(file_path)
    
# Pass the correct argument so that the test can use the fixture
def test_on_clean_file(clean_data_file):
    expected = np.array([[201.0, 305671.0], [7892.0, 298140.0], [501.0, 738293.0]])
    # Pass the clean data file path yielded by the fixture as the first argument
    actual = get_data_as_numpy_array(clean_data_file, 2)
    assert actual == pytest.approx(expected), "Expected: {0}, Actual: {1}".format(expected, actual) 
```


## Write a fixture for an empty data file

```
@pytest.fixture
def empty_file():
    # Assign the file path "empty.txt" to the variable
    file_path = "empty.txt"
    open(file_path, "w").close()
    # Yield the variable file_path
    yield file_path
    # Remove the file in the teardown
    os.remove(file_path)
    
def test_on_empty_file(self, empty_file):
    expected = np.empty((0, 2))
    actual = get_data_as_numpy_array(empty_file, 2)
    assert actual == pytest.approx(expected), "Expected: {0}, Actual: {1}".format(expected, actual)

```


## Mocking

Mocking is a trick which will allow us to test a function independently of its dependencies
To use mocking in pytest, we will need two packages.

* pytest-mock
* unittest.mock

## MagicMock() and mocker.patch()

The basic idea of mocking is to replace potentially buggy dependencies such as row_to_list() 
with the object 'unittest.mock.MagicMock()', but only during testing.

This replacement is done using a fixture called mocker, and calling its patch method right at the beginning of the test function

The first argument of the mocker.patch method is the fully qualified name of the dependency including module name, as registered by the function under test.

## Side effect

```
def row_to_list_bug_free():
    return_values = {
           "1,801\t201,411\n": ["1,801", "201,411"],
           "1,767565,112\n": None,
           "2,002\t333,209\n": ["2,002", "333,209"],
           "1990\t782,911\n": ["1990", "782,911"],
           "1,285\t389129\n": ["1,285", "389129"],
        }
    return return_values[row]
```


```
from unittest.mock import call

def test_on_raw_data(raw_and_clean_data_file, mocker):
    raw_path, clean_path = raw_and_clean_data_file
    row_to_list_mock = mocker.patch(
        "data.preprocessing_helpers.row_to_list",
        side_effect = row_to_list_bug_free
        )
    preprocess(raw_path, clean_path)
    assert row_to_list_mock.call_args_list == [
        call("1,801\t201,411\n"),
        call("1,767565,112\n"),
        call("2,002\t333,209\n"),
        call("1990\t782,911\n"),
        call("1,285\t389129\n")
        ]
```


## Program a bug-free dependency

```
# Define a function convert_to_int_bug_free
def convert_to_int_bug_free(comma_separated_integer_string):
   # Assign to the dictionary holding the correct return values 
   return_values = {"1,801": 1801,
                     "201,411": 201411,
                     "2,002": 2002,
                     "333,209": 333209,
                     "1990": None,
                     "782,911": 782911,
                     "1,285": 1285,
                     "389129": None,
                     }
   # Return the correct result using the dictionary return_values
   return return_values[comma_separated_integer_string]
```

## Mock a dependency

```
# Add the correct argument to use the mocking fixture in this test
def test_on_raw_data(self, raw_and_clean_data_file, mocker):
    raw_path, clean_path = raw_and_clean_data_file
    # Replace the dependency with the bug-free mock
    convert_to_int_mock = mocker.patch("data.preprocessing_helpers.convert_to_int",
                                       side_effect=convert_to_int_bug_free)
    preprocess(raw_path, clean_path)
    # Check if preprocess() called the dependency correctly
    assert convert_to_int_mock.call_args_list == [call("1,801"), call("201,411"), call("2,002"), call("333,209"), call("1990"), call("782,911"), call("1,285"), call("389129")]
    with open(clean_path, "r") as f:
        lines = f.readlines()
    first_line = lines[0]
    assert first_line == "1801\\t201411\\n"
    second_line = lines[1]
    assert second_line == "2002\\t333209\\n" 
```


## Testing plots

pytest-mpl


## Generating the baseline image

pytest -k "test_plot_for_linear_data" --mpl-generate-path visualization/baseline

## running the tests

pytest -k "test_plot_for_linear_data" --mpl


```
import pytest
import numpy as np
from visualization import get_plot_for_best_fit_line

@pytest.mark.mpl_image_compare    # Under the hood baseline generation and comparison
def test_plot_for_linear_data():
    slope = 2.0
    intercept = 1.0
    x_array = np.array([1.0, 2.0, 3.0])    # Linear data set 
    y_array = np.array([3.0, 5.0, 7.0])
    title = "Test plot for linear data"
    return get_plot_for_best_fit_line(slope, intercept, x_array, y_array, title)
```




