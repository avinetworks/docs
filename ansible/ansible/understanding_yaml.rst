############################
Understanding YAML
############################

This page will explain the structure of YAML (Yet syntax which is the syntax for Ansible playbooks. Ansible chose YAML because of it's ease of use and readability. It also maintans the same ability JSON does to display objects.

.. contents::
  :local:

***********
YAML Basics
***********

Indentation
===========

YAML uses a fixed indentation scheme. Each level consists of 2 spaces, do not use tabs.

Key-Value Pairs
===============

In YAML we display dictionaries as key-value pairs. Also know as hashes. In YAML, dictionary keys are represented by strings terminated by a colon. Following a space the value is specified. For example:

.. code-block:: yaml

  key: value

In python it would convert to, which just so happens to be the same as a json dictionary.

.. code-block:: python

  { 'key': 'value'}

Nesting Dictionaries
====================

To nest dictionaries in YAML we would use the following syntax.

.. code-block:: yaml

  first_level:
    second_level_key: second_level_value

In python this would be recognized as:

.. code-block:: python

  {
  "first_level": {
    "second_level_key": "second_level_value"
    }
  }

Arrays
======

In YAML we also can create arrays. Arrays are basically lists of items. Arrays can be a list of dictionaries or single items. Each item in a list is prefixed with a ``-`` and then a space.

.. code-block:: yaml

  - item1
  - item2
  - item3

This is a simple list of 3 items.

Often times in YAML we will display an array as a value of a key.

.. code-block:: yaml

  items:
    - item1
    - item2
    - item3

Which in python would be seen as

.. code-block:: python

  { "items": ["item1", "item2", "item3"] }

Handling Newlines
=================

Sometimes we will want to make our YAML values span multiple lines, or maybe shorten a really long line into a smaller one. We can do that in two ways.

The symbol ``|`` will maintain your newlines in the value.

.. code-block:: yaml

  include_newlines: |
    I definitely needed some
    new lines in this output

Will render as:

.. code-block:: none

  I definitely needed some
  new lines in this output

The symbol ``>`` will ignore newlines and move all lines into a single line of text.

.. code-block:: yaml

  ignore_newlines: >
    I am really
    starting to enjoy
    using YAML

Will render as:

.. code-block:: none

  I am really starting to enjoy using YAML
