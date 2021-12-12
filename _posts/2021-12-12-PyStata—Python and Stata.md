---
layout: post
title:  "PyStata—Python and Stata"
date:   2021-12-12 20:25:28 +0700
categories: [python, stata]
---


PyStata allows you to invoke Stata directly from any standalone Python environment and to call Python directly from Stata, thus, greatly expanding Stata's Python integration features.

New features in PyStata include:
1.the ability to use Stata from an IPython kernel-related environment like Jupyter Notebook, Spyder IDE, or PyCharm IDE;
2.the ability to use Stata from Python Shell, like the Windows Command Prompt, the macOS terminal, or the Unix terminal;
3.three IPython magic commands: stata, mata, and pystata; and
4.a suite of API functions from within Python to run Stata commands and access Stata data and returned results.
These tools, together with the Stata Function Interface (sfi) module, allow users to easily integrate Stata's vast statistical and data management methods into any data science project using Python.

## Let's see it work
Imagine that a health provider is interested in studying the effect of a new hospital admissions procedure on patient satisfaction. They have monthly data on patients before and after the new procedure was implemented in some of their hospitals. The data are in nested JSON format, and the health provider uses Python as the data analysis tool. But they would like to use Stata's new DID regression to analyze the effect of the new admissions procedure on the hospitals that participated in the program. The outcome of interest is patient satisfaction, satisfaction_score, and the treatment variable is procedure.

```python
# Setup Stata
import stata_setup
stata_setup.config("C:/Program Files/Stata17", "se")

# Import json data
from pandas.io.json import json_normalize
import json
with open("did.json") as json_file:
    data = json.load(json_file)
data = json_normalize(data, 'records', ['hospital_id', 'month'])

# Load data to Stata
from pystata import stata
stata.pdataframe_to_data(data, True)

### Run block of Stata code 
stata.run('''
destring satisfaction_score, replace
destring hospital_id, replace
destring month, replace

gen proc = 0
replace proc = 1 if procedure == "New"
label define procedure 0 "Old" 1 "New"
drop procedure
rename proc procedure
label value procedure procedure
''', quietly=True)

stata.run('''
didregress (satisfaction_score) (procedure), group(hospital_id) time(month)
''', echo=True)

# Load Stata results to Python
r = stata.get_return()['r(table)']

# Use Stata results in Python
print("The treatment hospitals had a %4.2f-point increase." 
      % (r[0][0]), end=" ") 
print("The result is with 95%% confidence interval [%4.2f, %4.2f]." 
      % (r[4][0], r[5][0]))

# Generate Stata graph 
stata.run("estat trendplots", quietly=True)
stata.run("graph export did.svg, replace", quietly=True)
```

We use the API function in a Python script, did.py, to interact with Stata. Some highlights of the code are

```python
# Setup Stata from within Python
import stata_setup
stata_setup.config("C:/Program Files/Stata17", "se")

# Import the json file into a Python dataframe
with open("did.json") as json_file:
    data = json.load(json_file)
data = json_normalize(data, 'records', ['hospital_id', 'month'])

# Load Python dataframe into Stata
from pystata import stata
stata.pdataframe_to_data(data, True)

# Run Stata commands in Python
stata.run('''
        didregress (satisfaction_score) (procedure), ///
                group(hospital_id) time(month)
        ''', echo=True)

# Load Stata saved results to Python
r = stata.get_return()['r(table)']

# Use them in Python
print("\n")
print("The treatment hospitals had a %5.2f-point increase." % (r[0][0]), end=" ")
print("The result is with 95%% confidence interval [%5.2f, %5.2f]." % (r[4][0], r[5][0]))

# Generate Stata graph in Python
stata.run("estat trendplots", echo=True)
stata.run("graph export did.svg, replace", quietly=True)

```
