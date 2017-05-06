# Python Style Guide

## Table of Contents

  1. [Inports](#inports)
  2. [Indentation](#indentation)
  
  ## Inports

  <a name="inports--sort"></a><a name="1.1"></a>
  - [1.1](#inports--sort) Split imports into 3 categories (stdlib, 3rdparty, internal imports), relative imports should be the last. Try to keep them sorted.
  
    ```python
    import datetime
    import logging

    from django.utils import timezone
    from django.utils.translation import ugettext_lazy as _
    from django.conf import settings
    from django_states.machine import StateMachine, StateDefinition, StateGroup, StateTransition
    from django_states.exceptions import TransitionValidationError

    from ourapp.notifications.handlers import NOTIFICATION_METHOD
    from ourapp.notifications import notifiers
    from ourapp.module_a.provider import Trusted
    from ourapp.activation.models import XOrder
    from ourapp.porting.utils import get_closest_wish_date
    from .models import Inport
    from .utils import add_inport_s3
    ```
  <a name="inports--modules"></a><a name="1.2"></a>
  - [1.2](#inports--modules) Use imports for packages and modules only, if it is possible.
  
    ```python
    import datetime  # never: from datetime import datetime or similar
    import logging  # never: from logging import getLogger
    import mock  # never from mock import Mock
    import random
    import uuid

    import bson  # never from bson import ObjectId
    from django.utils import timezone  # never: from django.utils.timezone import now
    ```
   <a name="inports--notfit"></a><a name="1.3"></a>
  - [1.3](#inports--notfit) If import exceeded line length limit, format it like that:
  
    ```python

    from ourapp.porting.tasks import (
        task_a,
        task_b,
        task_c,  # <- comma here
    )
    ```
   <a name="inports--quotes"></a><a name="1.4"></a>
  - [1.4](#inports--quotes) **Single quotes vs double quotes** Use double quotes around strings that are natural language messages and single quotes for small symbol-like strings, for example:
    ```python
    LIGHT_MESSAGES = {
        'english': "There are 3 lights.",
        'pirate':  "Arr! Thar be 3 lights."
    }
    def lights_message(language, number_of_lights):
        """Return a language-appropriate string reporting the light count."""
        return LIGHT_MESSAGES[language]

    def is_pirate(message):
        """Return True if the given message sounds piratical."""
        return re.search(r"(?i)(arr|avast|yohoho)!", message) is not None
    ```
        
    ref: http://stackoverflow.com/a/56190
        
        
  ## Indentation

  <a name="indentation--func"></a><a name="2.1"></a>
  - [2.1](#indentation--func) **function calls**
  
    ```python
    abc = qwe(dsadsa(product.to_dict(), cls=DecimalEncoder, mimetype='application/json')) # NO !

    abc = qwe(dsadsa(product.to_dict(), cls=DecimalEncoder,
              mimetype='application/json'))  # bad


    abc = qwe(dsadsa(product.to_dict(),
              cls=DecimalEncoder,
              mimetype='application/json'))  # bad


    abc = qwe(dsadsa(
        product.to_dict(),
        cls=DecimalEncoder,
        mimetype='application/json',  # <- comma here
    ))  # OK

    abc = qwe(
        dsadsa(
            product.to_dict(),
            cls=DecimalEncoder,
            mimetype='application/json',  # <- comma here
        )
    )  # could be too

    # especially when qwe has one aditional keyword parameter:

    abc = qwe(
        dsadsa(
            product.to_dict(),
            cls=DecimalEncoder,
            mimetype='application/json',  # <- comma here
        ),  # <- comma here
        dupa=2,  # <- comma here
    )  # OK
    ```
    
  <a name="indentation--dict1"></a><a name="2.2"></a>
  - [2.2](#indentation--dict1) **dict class**
  
    ```python 
    # bad
    abc = dict(abc=2, cde=5, mama='tata', tata='mama', raz=1, dwa=2)

    # bad
    abc = dict(abc=2, cde=5, mama='tata',
               tata='mama', raz=1, dwa=2)

    # bad  
    abc = dict(
      abc=2, cde=5, mama='tata',
      tata='mama', raz=1, dwa=2
    )

    # bad
    abc = dict(
      abc=2, cde=5, mama='tata',
      tata='mama', raz=1, dwa=2,
    )

    # good
    abc = dict(
      abc=2,
      cde=5,
      mama='tata',
      tata='mama',
      raz=1,
      dwa=2,  # <- comma here !
    )    
    ```
    
  <a name="indentation--dict2"></a><a name="2.3"></a>
  - [2.3](#indentation--dict2) **dict curly brackets**
  
    ```python
    # bad
    abc = {'abc': 2, 'cde': 5, 'mama': 'tata', 'tata': 'mama', 'raz': 1, 'dwa': 2}

    # bad
    abc = {'abc': 2, 'cde': 5, 'mama': 'tata',
           'tata': 'mama', 'raz': 1, 'dwa': 2}

    # bad    
    abc = {
        'abc': 2, 'cde': 5, 'mama': 'tata',
        'tata': 'mama', 'raz': 1, 'dwa': 2

    }

    # bad
    abc = {
        'abc': 2, 'cde': 5, 'mama': 'tata',
        'tata': 'mama', 'raz': 1, 'dwa': 2,
    }

    # good
    abc = {
        'abc': 2,
        'cde': 5,
        'mama': 'tata',
        'tata': 'mama',
        'raz': 1,
        'dwa': 2,  # <- comma here !
    }
    ```
    
  <a name="indentation--dictfunc"></a><a name="2.4"></a>
  - [2.4](#indentation--dictfunc) **func call and dicts**
  
    ```python
      abc = abc({
          'a': 1,
          'b': 3,
          'c': 4,  # <- comma here !
      })

      abc = abc(dict(
          a=1,
          b=2,
          c=3,  # <- comma here !
      ))

      nokaut_api_params = urllib.urlencode({
          'key': str(nokaut_key),
          'keyword': str(keyword),
          'format': 'xml',
          'method': 'nokaut.Product.getByKeyword',
          'sort_direction': 'price_asc',
          'returnType': 'full',
          'limit': '1',  # <- comma here !
      })
    ```
  
  <a name="indentation--listcomp"></a><a name="2.5"></a>
  - [2.5](#indentation--listcomp) **list comprehension**   
  
    ```python
      # bad
      data = [model_field
             for form_field, model_field in fields if form_field in form]

      # bad
      data = [model_field
             for form_field, model_field in fields if form_field in form
      ]

      # good
      data = [
          model_field
          for form_field, model_field in fields if form_field in form
      ]

      # good (if exceeding line length limit)
      data = [
          model_field
          for form_field, model_field in fields
          if form_field in form
      ]
    ```
        
  <a name="indentation--dictcomp"></a><a name="2.6"></a>
  - [2.6](#indentation--dictcomp) **dict comprehension**   

    ```python
      # bad
      data = {model_field: form[form_field].data
             for form_field, model_field in fields if form_field in form}

      # good
      data = {
          model_field: form[form_field].data
          for form_field, model_field in fields if form_field in form
      }

      # good (if exceeding line length limit)
      data = {
          model_field: form[form_field].data
          for form_field, model_field in fields
          if form_field in form
      }
    ```

  <a name="indentation--if"></a><a name="2.7"></a>
  - [2.7](#indentation--if) **if statement** if conditions are too long, store their result in variable with meaningful name

    ```python
    # bad
    if abc is None and b is True and c == 'mama' \
        and x != 1 or c in ('mama', 'tata') \
        or babcia.age > 100: # checks if thing should be done
        # do something big

    # bad
    if (abc is None and b is True and c == 'mama'
        and x != 1 or c in ('mama', 'tata')
        or babcia.age > 100): # checks if thing should be done
        # do something big

    # good
    should_be_done = (   # no comment needed !
        abc is None and
        b is True and
        c == 'mama'and
        x != 1 or
        c in ('mama', 'tata') or
        babcia.age > 100
    )

    if should_be_done:
        # do something big
    ```
