# Python Addition Function with GitHub Actions CI & Pytest
This project is a simple __Python application__ that demonstrates how to use __GitHub Actions__ to implement Continuous Integration (CI) with the __pytest testing framework__. It automatically runs tests every time code changes are pushed to a repository. The project is done in two ways as Github hosted runner and as Self hosted runner on EC2.
## Project Overview
* A simple Python function: add(a, b)

* Basic unit testing using pytest

* A CI workflow using GitHub Actions GitHub Hosted Runner and Self Hosted Runner

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
## Part 1: GitHub Actions CI Workflow (GitHub Hosted Runner)
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
## Part 2: Self Hosted Runner
In here we deploy a self hosted runner on an EC2 instance.
## Prerequisites
* Create an EC2 instance and ssh into it. 
* Edit both inbound and outbound traffic rules to include HTTP and HTTPS. This is because GitHub Actions is a cloud-based CI/CD service, and communication between GitHub Actions and your EC2 instance typically involves HTTPS (port 443).
## Edit .github/workflows file
The only change needed to be done here is to swap ubuntu-latest with self-hosted
```
jobs:
  build:
    runs-on: self-hosted
```
## Add New self-hosted runner
In your github repo go to settings> Actions tab> runners> add new self hosted runner
Select the type of operating system and CPU architecture.
There will be some commands given below that which you have to execute in your ec2 instance in order to configure the runner inside it.
After excuting the ./run.sh the runner will show a success message.
![connect1](https://github.com/user-attachments/assets/6c5e9664-fc58-438c-bbf8-ed2efd0a26e6)
## Testing
Do a code change to the addition.py and commit it.
Monitor the terminal and it will automatically display the results of the job running as triggered by the commit.
![runner](https://github.com/user-attachments/assets/813fc1ee-71e3-4223-96fe-a58630b4c47a)
