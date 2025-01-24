# La Suite numérique with Python

In this section, we will describe our best practices when dealing with Python code.

## PEP 8

Your code should at least follow the [PEP 8](https://www.python.org/dev/peps/pep-0008/) style guide with one single exception: the maximal line length is 99 characters \(instead of the default 79 characters\). 

Usually, we enforce this convention by adding a linting step to our continuous integration workflow using [`flake8`](https://flake8.readthedocs.io/en/latest/) with the following settings in the project's `setup.cfg`:

```ini
[flake8]
max-line-length = 99
exclude =
    .git,
    .venv,
    venv,
    __pycache__,
    node_modules,
*/migrations/*
```

## Documentation

The minimal documentation your code should have is a docstring \(see [PEP 257](https://www.python.org/dev/peps/pep-0257/)\) per module, method, class and function. For now we do not force you to use a particular style or describe all method or function arguments; we want you to explain to your peers \(and maybe you in a few months\) what your code is trying to achieve. Remember that it should be written for humans, while your code is for machines \(and humans 🤓\).

This also applies for tests, _e.g._:

```py
class FooTestCase(TestCase):
    """Test the Foo class in condition X"""
    
    def test_my_method_when_bar_is_None(self):
        """
        Test that my method asserts a FooException when bar is None
        
        Steps:
        
        * create a new Foo instance
        * update foo.bar with None
        * call foo.my_method()
        * assert it raises a FooException
        """
        pass
```

## Indentation

We had long discussions about following the PEP 8 \(with maximal line length\) and having an indentation style that satisfies everyone. Our consensus follows:

Until you hit the 99 characters limit, write your statement in one line:

```py
my_function(foo, bar, baz=None, spam='YUMMY', summary='lorem ipsum', is_fake=True)
```

If your statement is too long, then split your line with one argument per line:

```py
my_function(
    foo,
    bar,
    baz=None,
    spam='YUMMY',
    summary='lorem ipsum',
    is_fake=True,
    verbose=True,
    debug=True,
)
```

**Pro tip**: To fit with the maximal line length limit for long strings, think parentheses!

```py
foo = (
    "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor"
    "incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud"
    "exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure"
    "dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur."
    "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt"
    "mollit anim id est laborum."
)
```

## Import

Import statements should respect the following requirements:

* write global import statements first \(`import logging`\) then partial imports \(`from copy import copy`\),
* import statements should be written in the following order: 1. standard library, 2. Django imports 3. third party dependencies, and 4. application modules \(relative imports\); with an empty row between each,
* import statements should be sorted alphabetically \(`import bar` is written before `import foo`\),
* imported objects from a module should also be sorted alphabetically \(`from foo import bar, baz, lol`\).

An example follows:

```py
"""
This is a docstring for my module

[...]
"""
import logging
import re

from django.contrib import messages
from django.contrib.auth.mixins import LoginRequiredMixin
from django.db.models import Q
from django.http import HttpResponse, HttpResponseRedirect
from django.urls.base import reverse
from django.utils import timezone
from django.utils.translation import ugettext as _
from django.views.generic import (
    DetailView, FormView, ListView, RedirectView, View
)
from django.views.generic.detail import BaseDetailView
from django.views.generic.edit import FormMixin

import pandas

from apps.core.models import Bar, Foo, Lol
from .forms import (
    BarForm, FooExportForm, FooFightersBandRegistrationForm, FooFiltersForm,
    FooFightersLongForm,FooFightersUserShortForm, FooSelectForm
)
from .utils import export_bars, export_foos
```

## Test files

In Django, we want good names for our test files: they should bear the name of the Python module being tested (followed by the snakecased name of the class or method if necessary), from the more generic to the more specific.

Some examples of good names for test files:

```
test_forms_contact.py
test_models_user_send_welcome_email.py
test_forms_fields_datetimerange.py
```
