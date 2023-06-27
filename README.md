# Python-polarion

This package allows the user to access many Polarion items like workitems, test run, plans and documents.


# Feature overview

This package can, among others, read, modify and create:
- Workitems
- Test runs from templates
- Plans
- Documents

Work with attachments in workitems and test runs. 
Work with custom field in workitems and documents.

# Installation

## From PIP

```
pip install polarion
```

## From this repository

```
git clone http://svn.dis.lan:3000/werner.pirkl/python-polarion.git
python3 -m pip install -e python-polarion
```

If you have problems because libxml2 is not installing, get the wheel for your
python version from: https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml

# Username / Password

In order not to check-in your username and password, please use dotenv:

```
pip install python-dotenv
```

Create a .env file where you run your code (next to the jupyter notebook for example)

```
username=<your username>
password=<your password>
```

load the environment in a python script:

```
from dotenv import load_dotenv

load_dotenv()
```

load the environment in jupyter:

```
%load_ext dotenv
%dotenv
```

you can now retreive your username and password like that:

```
username = os.getenv('username')
password = os.getenv('password')
```

# Getting started

Creating the Polarion client and getting workitems, test runs or plans:

```python
from polarion import polarion
client = polarion.Polarion('http://example.com/polarion', 'user', 'password')
project = client.getProject('Python')
workitem = project.getWorkitem('PYTH-510')
run = project.getTestRun('SWQ-0001')
plan = project.getPlan('00002')
```

Modifying workitems:

```python
workitem.setDescription('Some description..')
workitem.addComment('test comment', 'sent from Python')
workitem.addHyperlink('google.com', workitem.HyperlinkRoles.EXTERNAL_REF)
```

Or test run results:
```python
run = project.getTestRun('SWQ-0001')
run.records[0].setResult(record.Record.ResultType.PASSED, ' Comment with test result')
```

Adding workitems to a plan:
```python
plan.addToPlan(workitem)
plan.removeFromPlan(workitem)
```


More examples to be found in the quick start section of the documentation.
[Go to the documentation](https://python-polarion.readthedocs.io/)

# How does it work?

This project uses the SOAP API of Polarion. This API exposes most of the user interactions you can do with Polarion like creating or editing workitems, plans and test runs.
The API is divided in seven different services which you can find from your Polarion instance at the url http://domain.com/polarion/ws/services.
Each of the services provides a WSDL file detailing the available functions. (Also available form you local instance at http://domain.com/polarion/ws/services/TrackerWebService?wsdl)
For this project the TrackerWebService, PlanningWebService and TestManagementWebService are the most used ones.

In general the project attempts for the objects (like workitems) to behave like Python objects which you can modify and are saved in the background. 
Where the API provide operation to preform an action that API call is used, and the object is reloaded from polarion to reflect the changes locally.

The API does not allow access to the project administration.

# Dependencies 

The package uses; requests, urllib3 and zeep.

It is tested for Python version 3.6 through 3.10.

# Known issues or missing features
- No way of knowing the test run possible statuses.

