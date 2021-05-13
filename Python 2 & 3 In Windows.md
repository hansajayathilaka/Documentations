Install two (or more, using their installers) versions of Python on Windows 10 (for me work with 3.9 and 2.7). 

Follow the instuctions below, changing the parameters for your needs.

Create the following environment variable (to default on double click):
```
Name:  PY_PYTHON
Value: 3
```
To launch a script in a particular interpreter, add the following shebang (beginning of script):

    #! python2

To execute a script using a specific interpreter, use the following prompt command:

    > py -2 MyScript.py

To launch a specific interpreter:

    > py -2

To launch the default interpreter (defined by the PY_PYTHON variable):

    > py

**Resources**

Documentation: [Using Python on Windows][1]

[PEP 397][2] - Python launcher for Windows


  [1]: http://docs.python.org/3.3/using/windows.html
  [2]: http://www.python.org/dev/peps/pep-0397/
