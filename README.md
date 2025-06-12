#  Python Addition Function with GitHub Actions CI & Pytest
This project is a simple __Python application__ that demonstrates how to use __GitHub Actions__ to implement Continuous Integration (CI) with the __pytest testing framework__. It automatically runs tests every time code changes are pushed to a repository.
## Project Overview
* A simple Python function: add(a, b)

* Basic unit testing using pytest

* A CI workflow using GitHub Actions

* Multi-version testing across Python 3.8 and 3.9
## Python Code (addition.py)
```
# src/addition.py

def add(a, b):
    return a + b

def test_add():
    assert add(1, 2) == 3
    assert add(1, -1) == 0
```
* __add(a, b):__ Adds two numbers and returns the result.

* __test_add():__ Unit test that checks whether add() behaves correctly.
## GitHub Actions CI Workflow
The file .github/workflows/first-actions.yml defines the workflow that is triggered __every time code is pushed__ to the repository.
```
name: My First GitHub Actions

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.8, 3.9]

    steps:
    - uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest

    - name: Run tests
      run: |
        python -m pytest addition.py
```
In steps:
1. __Checkout Code:__ Downloads the latest code from your repo.

2. __Set up Python:__ Configures Python 3.8 and 3.9 using a matrix strategy.

3. __Install Dependencies:__ Installs pytest via pip.

4. __Run Tests:__ Runs tests using pytest.
## How pytest Works Here
1. __Test Discovery:__ When pytest is run, it scans for functions named test_* inside files like addition.py.

2. __Test Execution:__ It runs each test function and checks whether assertions are True.

3. __Results Reporting:__ If all assertions pass, tests succeed; if any fail, detailed output is shown.
## Example Output
As soon as you make a change to the addition.py and commit the changes, the workflow will automatically start running. 
![1-](https://github.com/user-attachments/assets/6e9cdbb6-35d8-4a67-a3d7-e5dff8d9b4fb)

When you push changes to GitHub, the Actions tab will show the test results. If all assertions in test_add() pass, you’ll see a green ✔ success. If something breaks, pytest will show exactly which test and value caused the failure.
![2-](https://github.com/user-attachments/assets/1ba4d67d-e04d-46f9-8e1d-059afd02cdf0)
