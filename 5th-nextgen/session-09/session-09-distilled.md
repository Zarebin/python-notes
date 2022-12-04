# Script Writing
Any file can execute either as a script or as a library imported with import. Script code is often enclosed with a conditional check against the module name.  
\__name__ is a built-in variable that always contains the name of the enclosing module.  
* run as the main script such as python readport.py --> \__name__ = \__main__
* imported using a statement such as import readport --> \__name__ = readport

To get filename from user:
* use the built-in `input()`
* use sys.argv list
example:
```python
def main(argv):
    if len(argv) == 1:
        filename = input('Enter filename: ')
    elif len(argv) == 2:filename = argv[1]
    else:
        raise SystemExit(f'Usage: {argv[0]} [ filename ]')
    portfolio = read_portfolio(filename)
    for name, shares, price in portfolio:
        print(f'{name:>10s} {shares:10d} {price:10.2f}')
if __name__ == '__main__':
    import sys
    main(sys.argv) 
```

run in two different ways:
1. 
```python
python readport.py
Enter filename: portfolio.csv
... 
```
len(argv) is 1 so it gets input from user.

2. 
```python
bash % python readport.py portfolio.csv 
```
len(argv) is 2 so `argv[1]` will be the filename.  

The following example leads to `SystemExit` because len(argv) is more than 2:
```python
bash % python readport.py a b c
Usage: readport.py [ filename ] 
```

For more advanced usage, the `argparse` standard library module can be used (instead of argv).

# Packages
It is common to organize code into packages. A package is a hierarchical collection of modules. Put your code as a directory like this:
```python
tutorial/
    __init__.py
    readport.py
    pcost.pystack.py
    ... 
```
The directory should have an `__init__.py` file, which may be empty.  

One issue with packages is imports between files within the same package. For example, if the pcost.py and readport.py files are moved into a package, we must use a fully qualified module import:
``python
# pcost.py
from tutorial import readport
...
```
or use package-relative import (which has the benefit of not hardcoding the package name):
``python
# pcost.py
from . import readport
...
```
# Structuring an Application
First, it is standard practice to organize large code bases into packages. You might additionally have tests, examples, scripts, and documentation. This additional material
usually lives in a separate set of directories than the package containing your source code. Thus, it is common to create an enclosing top-level directory.
e.g:
```python
tutorial-project/
    tutorial/
        __init__.py
        readport.py
        pcost.py
        stack.py
        ...
    tests/
        test_stack.pytest_pcost.py
        ...
    examples/
        sample.py
        ...
    doc/
        tutorial.txt
        ...
```
but there is more than one way to do it.

# Managing Third-Party Packages
install a third-party package, use a command such as pip.  
Installed packages are placed into a special *site-packages* directory that you can find if you inspect the value of sys.path.  
If you are ever left wondering where a package is coming from, inspect the \__file__ attribute. e.g:
```python
>>> import pandas
>>> pandas.__file__
'/usr/local/lib/python3.8/site-packages/pandas/__init__.py'
>>>
```

Altering the installation of that version of Python is often a bad idea. So it is recommended to use *virtual environment*.
```console 
bash % python3 -m venv myproject
```
Then you’ll get an interpreter configured for your personal use. 
```console 
bash % ./myproject/bin/python3 -m pip install somepackage
```

# Python: It Fits Your Brain
The core of Python is a small programming language along with a useful collection of built-in objects. A vast
array of practical problems can be solved using nothing more than the basic features.