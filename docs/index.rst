.. _index:

****************************************
Pylons Project Documentation Style Guide
****************************************

.. meta::
   :description: This chapter is the style guide used for documentation of all Pylons Project projects.
   :keywords: Style Guide, Documentation


.. _dsg-introduction:

Introduction
============

This document is aimed at authors of and contributors to documentation for projects under the Pylons Project.
This document describes style and reStructuredText syntax used in project documentation.
We provide examples, including reStructuredText code and its rendered output, for both visual and technical reference.

The source code of this guide is located in its `project repository on GitHub <https://github.com/Pylons/docs-style-guide>`_.

For Python coding style guidelines, see `Coding Style <https://pylonsproject.org/community-coding-style-standards.html#coding-style>`_.

We have adopted the convention from :RFC:`2119` for key words.

    The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in :RFC:`2119`.

Other conventions throughout this guide are adopted and adapted from `Documenting Python <https://devguide.python.org/documenting/>`_, `Plone Documentation Style Guide <https://docs.plone.org/about/contributing/documentation_styleguide.html>`_, and `Write The Docs <http://www.writethedocs.org/>`_.


.. toctree::
   :maxdepth: 2
   :caption: Contents:


.. _dsg-contributing:

Contributing to a project's documentation
-----------------------------------------

All projects under the Pylons Projects follow the guidelines established at `How to Contribute <https://pylonsproject.org/community-how-to-contribute.html>`_.

When submitting a pull request for the first time in a project, sign its ``CONTRIBUTORS.txt`` and commit it along with your pull request.

Contributors to documentation should be familiar with `reStructuredText <http://docutils.sourceforge.net/rst.html>`_ (reST) for writing documentation.
Most projects use `Sphinx <http://www.sphinx-doc.org/en/master/>`_ to build documentation from reST source files, and `Read The Docs <https://readthedocs.org/>`_ (RTD) for publishing them on the web.
Experience with Sphinx and RTD may be helpful, but not required.


.. _dsg_testing-documentation:

Testing Documentation
---------------------

Before submitting a pull request, documentation should be tested locally.
Ultimately testing of documentation must be done before merging a pull request.
This is typically done through a project's integration with Travis CI or Appveyor.

*   Use Sphinx's ``make html`` command to build the HTML output of the documentation without errors or warnings.
    Some projects use ``tox -e docs`` or just ``tox`` to invoke Sphinx's ``make html``.
    Most other build utilities like `restview <https://pypi.org/project/restview/>`_ and `readme_renderer <https://pypi.org/project/readme_renderer/>`_ test only a single file and do not support cross-references between files.
*   If documentation has doctests and uses :ref:`dsg-sphinx-extension-doctest`, then run Sphinx's ``make doctest`` command.
*   Optionally use Sphinx's ``make linkcheck`` command to verify that links are valid and to avoid bit rot.
    It is acceptable to ignore broken links in a project's change log and history.
    Narrative and API documentation should occasionally have its links checked.
*   Optionally use Sphinx's ``make epub`` and ``make latexpdf`` or ``make xelatex`` commands to build epub or PDF output of the documentation.

This project's `repository <https://github.com/Pylons/docs-style-guide>`_ has an example of how to configure Sphinx, tox, and Travis to build documentation.


.. _dsg-documentation-structure:

Documentation structure
=======================

This section describes the structure of documentation and its files.


.. _dsg-location:

Location
--------

Documentation source files should be contained in a folder named ``docs`` located at the root of the project.
Images and other static assets should be located in ``docs/_static``.

reST directives must refer to files either relative to the source file or absolute from the top source directory.
For example, in ``docs/narr/source.rst``, you could refer to a file in a different directory as either:

.. code-block:: rst

    .. include:: ../diff-dir/diff-source.rst

or:

.. code-block:: rst

    .. include:: /diff-dir/diff-source.rst.


.. _dsg-file-naming:

File naming
-----------

reST source files and static assets should have names that consist only of lowercase letters (``a-z``), numbers (``0-9``), periods (``.``), and hyphens (``-``) instead of underscores (``_``).
Files must start a letter.
reST source files should have the extension of ``.rst``.

Image files may be any format, but must have standard file extensions that consist of three letters (``.gif``, ``.jpg``, ``.png``, ``.svg``).
``.gif`` and ``.svg`` are not currently supported by PDF builders in Sphinx.
However you can use an asterisk (``*``) as a :index:`wildcard extension` instead of the actual file extension.
This allows the Sphinx builder to automatically select the correct image format for the desired output.
For example:

    .. code-block:: rst

        .. image:: ../_static/pyramid_request_processing.*

will select the image ``pyramid_request_processing.svg`` for the HTML documentation builder, and ``pyramid_request_processing.png`` for the PDF builder.
See the related `Stack Overflow post <https://stackoverflow.com/questions/6473660/using-sphinx-docs-how-can-i-specify-png-image-formats-for-html-builds-and-pdf-im/6486713#6486713>`_.


.. _dsg-index:

Index
-----

Documentation must have an index file whose purpose is to serve as a home page for the documentation, including references to all other pages in the documentation.
The index file should be named ``index.rst``.
Each section, or a subdirectory of reST files such as a tutorial, must contain an ``index.rst`` file.

The index should contain at least a :ref:`dsg-table-of-contents` through the `toctree <http://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#toctree-directive>`_ directive.

The index should include a reference to both a search page and a general index page, which are automatically generated by Sphinx.
See below for an example.

.. code-block:: rst

    * :ref:`genindex`
    * :ref:`search`

* :ref:`genindex`
* :ref:`search`


.. _dsg-glossary:

Glossary
--------

Documentation may have a glossary file.
If present, it must be named ``glossary.rst``.

This file defines terms used throughout the documentation.
Its content must begin with the directive ``glossary``.
An optional ``sorted`` argument should be used to sort the terms alphabetically when rendered, making it easier for the user to find a given term.
Without the argument ``sorted``, terms will appear in the order of the ``glossary`` source file.

Example:

.. code-block:: rst

    .. glossary::
        :sorted:

        voom
            Theoretically, the sound a parrot makes when four-thousand volts of electricity pass through it.

        pining
            What the Norwegien Blue does when it misses its homeland, for example, pining for the fjords.

The above code renders as follows.

.. glossary::
    :sorted:

    voom
        Theoretically, the sound a parrot makes when four-thousand volts of electricity pass through it.

    pining
        What the Norwegian Blue does when it misses its homeland, for example, pining for the fjords.


References to glossary terms appear as follows.

.. code-block:: rst

    :term:`voom`

:term:`voom`

Note it is hyperlinked, and when clicked it will take the user to the term in the Glossary and highlight the term.


.. _dsg-change-history:

Change history
--------------

Either a reference to a change history log file in the project or its inclusion in the documentation should be present.
Change history should include feature additions, deprecations, bug fixes, release dates and versions, and other significant changes to the project.


.. _dsg-file-encoding:

File encoding
-------------

All documentation source files must be in UTF-8 encoding to allow special characters, including em-dash (``—``) and tree diagram characters.


.. _dsg-general-guidance:

General guidance
================

This section describes the general voice, tone, and style that documentation should follow.
It also includes things for authors to consider when writing to your audience.


Accessibility
-------------

Consider that your audience includes people who fall into the following groups:

- People who do not use English as their first language (English language rules are insanely complex).  According to our web statistics for docs.pylonsproject.org, about 36% of all readers of documentation under the Pylons Project do not use English as their first language.  And only about 32% of all visitors are from the United States, United Kingdom, Canada, Australia, New Zealand, and Ireland.
- Visually impaired readers (a comma makes a huge difference to a screen reader, adding a "breath", much like a musical breath symbol).
- Readers who don't have college-level reading comprehension.
- Folks with reading disabilities.


Voice
-----

It is acceptable to address the reader as "you".
This helps make documentation, especially tutorials, more approachable.
"You" is also less formal than "the user".


Avoid sentence run-ons
----------------------

Instead of using long sentences, consider breaking them into multiple shorter sentences.
Long complicated sentences are more difficult to understand than shorter, clearer sentences.
Consider that your audience is not familiar with your content.
That is why they are reading your documentation.


Gender
------

Except for speaking for oneself, the author should avoid using pronouns that identify a specific gender.
Neutral gender pronouns "they", "them", "their", "theirs", "it", and "its" are preferred.
Never use the hideous and clumsy "he/she".


Style
-----

Avoid hype and marketing.

Avoid words that can frustrate or discourage the reader if they are not able to complete or understand a concept.
Such words include "simple", "just", "very", "easy", and their derivations.
For example, "Simply run the command ``foo bar``, and you're up and running!" will frustrate a user who has neither installed the requirements for ``foo`` nor configured it to execute ``bar``.


English Syntax
--------------

Use proper spelling, grammar, and punctuation.
`English Language & Usage <https://english.stackexchange.com/>`_ is a good resource.

Never use "and/or".
If you cannot figure out whether you should use "and" or "or", and are tempted to use the lazy "and/or", then write the sentence so it is clear.

Avoid abbreviations.
Spell out words.

Avoid "etc.", "e.g.", and "i.e.".
They do not translate well or read well by screen readers for the visually impaired.
It is lazy, and sounds pretentious.
Writers seldom get the usage or punctuation right.
Instead spell it out to their meanings of "and so on", "for example", and "in other words".


.. _dsg-page-structure:

Documentation page structure
============================

Each page should contain in order the following.

#.  The main heading. This will be visible in the table of contents.

    .. code-block:: rst

        ================
        The main heading
        ================

#.  Meta tag information.
    The "meta" directive is used to specify HTML metadata stored in HTML META tags.
    Metadata is used to describe and classify web pages in the World Wide Web in a form that is easy for search engines to extract and collate.

    .. code-block:: rst

        .. meta::
           :description: This chapter describes how to edit, update, and build the Pyramid documentation.
           :keywords: Pyramid, Style Guide

    The above code renders as follows.

    .. code-block:: xml

        <meta content="This chapter describes how to edit, update, and build the Pyramid documentation." name="description" />
        <meta content="Pyramid, Style Guide" name="keywords" />

#.  Introduction paragraph.

    .. code-block:: rst

        Introduction
        ------------

        This chapter is an introduction.

#.  Finally the content of the document page, consisting of reST elements such as headings, paragraphs, tables, and so on.


.. _dsg-documentation-content:

Documentation page content
==========================

This section describes the content of documentation source files.


.. _dsg-use-of-whitespace:

Use of whitespace
-----------------

Narrative documentation is not code, and should therefore not adhere to :pep:`8` standards or other line length conventions.
There is no maximum line length.
Lines should be the length of one sentence, without hard wraps.
Lines should not end with white space.

When a paragraph contains more than one sentence, its sentences should be written one per line.
This makes it easier to edit and find differences when updating documentation.

For example:

.. code-block:: rst

    This is the first sentence in a paragraph.
    This is the second sentence in a paragraph.

    This is a new paragraph.

Which renders as follows.

    This is the first sentence in a paragraph.
    This is the second sentence in a paragraph.

    This is a new paragraph.

All indentation should be 4 spaces.
Do not use tabs to indent.

Each section should have two trailing blank lines to separate it from the previous section. A section heading should have a blank line following its underline and before its content.

A line feed or carriage return should be used at the end of a file.

All other whitespace is defined by reST syntax.


.. _dsg-table-of-contents:

Table of contents
-----------------

``toctree`` entries should exclude the file extension.
For example:

.. code-block:: rst

    .. toctree::
        :maxdepth: 2

        narr/introduction
        narr/install
        narr/firstapp


.. _dsg-headings:

Headings
--------

Chapter titles, sections, sub-sections, sub-sub-sections, and sub-sub-sub-sections within a chapter (usually a single reST file) are indicated with markup and are displayed as headings at various levels.
Conventions are adopted from `Documenting Python, Sections <https://devguide.python.org/documenting/#sections>`_.

Headings are rendered in HTML as follows:

====================  ============================  ========
   Heading Level      underline/overline character    HTML
====================  ============================  ========
Title                 ``*``                         ``<h1>``
Sections              ``=``                         ``<h2>``
Sub-sections          ``-``                         ``<h3>``
Sub-sub-sections      ``^``                         ``<h4>``
Sub-sub-sub-sections  ``"``                         ``<h5>``
====================  ============================  ========

The chapter title of this document, *Documentation Style Guide*, has the following reST code.

.. code-block:: rst

    *************************
    Documentation Style Guide
    *************************

Chapter titles should be over- and underlined with asterisks (``*``).

The heading for this section, *Documentation content*, has the following reST code:

.. code-block:: rst

    Documentation content
    =====================

The heading for this sub-section, *Headings*, has the following reST code:

.. code-block:: rst

    Headings
    --------

Sub-sub-section, and sub-sub-sub-section headings are shown as follows.

Heading Level 4
^^^^^^^^^^^^^^^

This is a sub-sub-section.

.. code-block:: rst

    Heading Level 4
    ^^^^^^^^^^^^^^^

Heading Level 5
"""""""""""""""

This is a sub-sub-sub-section.

.. code-block:: rst

    Heading Level 5
    """""""""""""""


.. _dsg-paragraphs:

Paragraphs
----------

A paragraph of text looks exactly like this paragraph.
Paragraphs must be separated by two line feeds.


.. _dsg-links:

Links to websites
-----------------

It is recommended to use inline links to keep together the context or link label with the URL.
Avoid the use of targets and links at the end of the page, because the separation makes it difficult to update and translate.
Here is an example of inline links, our preferred method.

.. code-block:: rst

    `TryPyramid <https://trypyramid.com>`_

The above code renders as follows.

`TryPyramid <https://trypyramid.com>`_

.. seealso:: See also :ref:`dsg-cross-reference-links` for generating links throughout the entire documentation.


.. _dsg-cross-reference-links:

Cross-reference links
---------------------

To create cross-references to a document, arbitrary location, object, or other items, use variations of the following syntax.

*   ``:role:`target``` creates a link to the item named ``target`` of the type indicated by ``role``, with the link's text as the title of the target.
    ``target`` may need to be disambiguated between documentation sets linked through intersphinx, in which case the syntax would be ``deform:overview``.
*   ``:role:`~target``` displays the link as only the last component of the target.
*   ``:role:`title <target>``` creates a custom title, instead of the default title of the target.


.. _dsg-cross-referencing-documents:

Cross-referencing documents
^^^^^^^^^^^^^^^^^^^^^^^^^^^

To link to pages within this documentation:

.. code-block:: rst

    :doc:`glossary`

The above code renders as follows.

:doc:`glossary`


.. _dsg-cross-referencing-arbitrary-locations:

Cross-referencing arbitrary locations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To support cross-referencing to arbitrary locations in any document and between documentation sets via intersphinx, the standard reST labels are used.
For this to work, label names must be unique throughout the entire documentation including externally linked intersphinx references.
There are two ways in which you can refer to labels, if they are placed directly before a section title, a figure, or table with a caption, or at any other location.
This section has a label with the syntax ``.. _label_name:`` followed by the section title.

.. code-block:: rst

    .. _dsg-cross-referencing-arbitrary-locations:

    Cross-referencing arbitrary locations
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To generate a link to that section with its title, use the following syntax.

.. code-block:: rst

    :ref:`dsg-cross-referencing-arbitrary-locations`

The above code renders as follows.

:ref:`dsg-cross-referencing-arbitrary-locations`

The same syntax works for figures and tables with captions.

For labels that are not placed as mentioned, the link must be given an explicit title, such as ``:ref:`title <label>```.

.. seealso:: See also the Sphinx documentation, :ref:`sphinx:rst-inline-markup`.


.. _dsg-cross-referencing-python:

Python modules, classes, methods, and functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All of the following are clickable links to Python modules, classes, methods, and functions.

Python module names use the ``mod`` directive, with the module name as the argument.

.. code-block:: rst

    :mod:`python:string`

The above code renders as follows.

:mod:`python:string`

Python class names use the ``class`` directive, with the class name as the argument.

.. code-block:: rst

    :class:`str`

The above code renders as follows.

:class:`str`

Python method names use the ``meth`` directive, with the method name as the argument.

.. code-block:: rst

    :meth:`python:str.capitalize`

The above code renders as follows.

:meth:`python:str.capitalize`

Python function names use the ``func`` directive, with the function name as the argument.

.. code-block:: rst

    :func:`pyramid.renderers.render_to_response`

The above code renders as follows.

:func:`pyramid.renderers.render_to_response`

Sometimes we show only the last segment of a Python object's name, which displays as follows.

.. code-block:: rst

    :func:`~pyramid.renderers.render_to_response`

The above code renders as follows.

:func:`~pyramid.renderers.render_to_response`


.. _dsg-peps:

PEPs
^^^^

Use the directive ``:pep:`` to refer to PEPs and link to them.

.. code-block:: rst

    :pep:`8`

The above code renders as follows.

:pep:`8`

.. _dsg-rfcs:

RFCs
^^^^

Use the directive ``:rfc:`` to refer to RFCs and link to them.

.. code-block:: rst

    :rfc:`2119`

The above code renders as follows.

:rfc:`2119`


.. _dsg-lists:

Lists
-----

All list items should have their text indented to start in the fifth column.
This makes it easier for editors to maintain uniform indentation, especially for nested and definition lists.

Bulleted lists use syntax and display as follows.
It is recommended to use asterisks (``*``) for each list item because they stand out better than hyphens (``-``) in the reST source.

.. code-block:: rst

    *   This is an item in a bulleted list.
    *   This is another item in a bulleted list.

        The second item supports paragraph markup.

*   This is an item in a bulleted list.
*   This is another item in a bulleted list.

    The second item supports paragraph markup.

Numbered lists use syntax and display as follows.
Numbered lists should use a number sign followed by a period (``#.``) and will be numbered automatically.
Avoid manually numbering lists, instead allowing Sphinx to do the numbering for you.

.. code-block:: rst

    #.  This is an item in a numbered list.
    #.  This is another item in a numbered list.

#.  This is an item in a numbered list.
#.  This is another item in a numbered list.

Nested lists use syntax and display as follows.
The appearance of nested lists can be created by separating the child lists from their parent list by blank lines, and indenting four spaces.

.. code-block:: rst

    #.  This is a list item in the parent list.
    #.  This is another list item in the parent list.

        #.  This is a list item in the child list.
        #.  This is another list item in the child list.

    #.  This is one more list item in the parent list.

#.  This is a list item in the parent list.
#.  This is another list item in the parent list.

    #.  This is a list item in the child list.
    #.  This is another list item in the child list.

#.  This is one more list item in the parent list.

Definition lists use syntax and display as follows.

.. code-block:: rst

    term
        Definition of the term is indented.

        A definition may have paragraphs and other reST markup.

    next term
        Next term definition.

term
    Definition of the term is indented.

    A definition may have paragraphs and other reST markup.

next term
    Next term definition.


.. _dsg-images:

Images
------

To display images, use the ``image`` directive.

.. code-block:: rst

    .. image:: /_static/pyramid_request_processing.*

Will render in HTML as the following.

.. image:: /_static/pyramid_request_processing.*


.. _dsg-source-code:

Source code
-----------

Source code may be displayed in blocks or inline.

Code blocks
^^^^^^^^^^^

Blocks of code should use syntax highlighting and specify the language, and may use line numbering or emphasis.
Avoid the use of two colons (``::``) at the end of a line, followed by a blank line, then code, because this may result in code parsing warnings (or worse, silent failures) and incorrect source code highlighting.
Always indent the source code in a code block.
Code blocks use the directive ``code-block``.
Its syntax is the following, where the Pygments `lexer <http://pygments.org/docs/lexers/>`_ is the name of the language, with options indented on the subsequent lines.

.. code-block:: rst

    .. code-block:: lexer
        :option:

.. seealso:: See also the Sphinx documentation for :ref:`sphinx:code-examples`.


.. _dsg-syntax-highlighting:

Syntax highlighting examples
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Python:

.. code-block:: rst

    .. code-block:: python

        if "foo" == "bar":
            # This is Python code
            pass

Renders as:

.. code-block:: python

    if "foo" == "bar":
        # This is Python code
        pass

Shell commands should have no prefix character.
They get in the way of copy-pasting commands, and often don't look right with syntax highlighting.

Bash:

.. code-block:: rst

    .. code-block:: bash

        $VENV/bin/pip install -e .

Renders as:

.. code-block:: bash

    $VENV/bin/pip install -e .

Windows:

.. code-block:: rst

    .. code-block:: doscon

        %VENV%\Scripts\pserve development.ini

Renders as:

.. code-block:: doscon

    %VENV%\Scripts\pserve development.ini

XML:

.. code-block:: rst

    .. code-block:: xml

        <somesnippet>Some XML</somesnippet>

Renders as:

.. code-block:: xml

    <somesnippet>Some XML</somesnippet>

cfg:

.. code-block:: rst

    .. code-block:: cfg

        [some-part]
        # A random part in the buildout
        recipe = collective.recipe.foo
        option = value

Renders as:

.. code-block:: cfg

    [some-part]
    # A random part in the buildout
    recipe = collective.recipe.foo
    option = value

ini:

.. code-block:: rst

    .. code-block:: ini

        [nosetests]
        match=^test
        where=pyramid
        nocapture=1

Renders as:

.. code-block:: ini

    [nosetests]
    match=^test
    where=pyramid
    nocapture=1

Interactive Python:

.. code-block:: rst

    .. code-block:: pycon

        >>> class Foo:
        ...     bar = 100
        ...
        >>> f = Foo()
        >>> f.bar
        100
        >>> f.bar / 0
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        ZeroDivisionError: integer division or modulo by zero

Renders as:

.. code-block:: pycon

    >>> class Foo:
    ...     bar = 100
    ...
    >>> f = Foo()
    >>> f.bar
    100
    >>> f.bar / 0
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ZeroDivisionError: integer division or modulo by zero


.. _dsg-long-commands:

Displaying long commands
^^^^^^^^^^^^^^^^^^^^^^^^

When a command that should be typed on one line is too long to fit on the displayed width of a page, the backslash character (``\``) is used to indicate that the subsequent printed line should be part of the command:

.. code-block:: rst

    .. code-block:: bash

        $ $VENV/bin/py.test tutorial/tests.py --cov-report term-missing \
            --cov=tutorial -q

Renders as:

.. code-block:: bash

    $ $VENV/bin/py.test tutorial/tests.py --cov-report term-missing \
        --cov=tutorial -q


.. _dsg-code-block-options:

Code block options
^^^^^^^^^^^^^^^^^^

To emphasize lines, we give the appearance that a highlighting pen has been used on the code using the option ``emphasize-lines``.
The argument passed to ``emphasize-lines`` must be a comma-separated list of either single or ranges of line numbers.

.. code-block:: rst

    .. code-block:: python
        :emphasize-lines: 1,3

        if "foo" == "bar":
            # This is Python code
            pass

Renders as:

.. code-block:: python
    :emphasize-lines: 1,3

    if "foo" == "bar":
        # This is Python code
        pass

A code block with line numbers.

.. code-block:: rst

    .. code-block:: python
        :linenos:

        if "foo" == "bar":
            # This is Python code
            pass

Renders as:

.. code-block:: python
    :linenos:

    if "foo" == "bar":
        # This is Python code
        pass

Code blocks may be given a caption.
They may also be given a ``name`` option, providing an implicit target name that can be referenced by using ``ref`` (see :ref:`dsg-cross-referencing-arbitrary-locations`).

.. code-block:: rst

    .. code-block:: python
        :caption: sample.py
        :name: sample-py-dsg

        if "foo" == "bar":
            # This is Python code
            pass

Renders as:

.. code-block:: python
    :caption: sample.py
    :name: sample-py-dsg

    if "foo" == "bar":
        # This is Python code
        pass


To specify the starting number to use for line numbering, use the ``lineno-start`` directive.

.. code-block:: rst

    .. code-block:: python
        :lineno-start: 2

        if "foo" == "bar":
            # This is Python code
            pass

Renders as:

.. code-block:: python
    :lineno-start: 2

    if "foo" == "bar":
        # This is Python code
        pass

.. _dsg-includes:

Include code blocks from external files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Longer displays of verbatim text may be included by storing the example text in an external file containing only plain text or code.
The file may be included using the ``literalinclude`` directive.
The file name follows the conventions of :ref:`dsg-file-naming`.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python

The above code renders as follows.

.. literalinclude:: src/helloworld.py
    :language: python

Like code blocks, ``literalinclude`` supports the following options.

*   ``language`` to select a language for syntax highlighting
*   ``linenos`` to switch on line numbers
*   ``lineno-start`` to specify the starting number to use for line numbering
*   ``emphasize-lines`` to emphasize particular lines

Additionally literal includes support the following options:

*   ``pyobject`` to select a Python object to display
*   ``lines`` to select lines to display
*   ``lineno-match`` to match line numbers with the selected lines from the ``lines`` option


.. _dsg-linenos-and-lineno-start:

``linenos`` and ``lineno-start`` issues
"""""""""""""""""""""""""""""""""""""""

Avoid the use of ``linenos`` and ``lineno-start`` with literal includes.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python
        :linenos:
        :lineno-start: 11
        :emphasize-lines: 1,6-7,9-

The above code renders as follows.
Note that ``lineno-start`` and ``emphasize-lines`` do not align.
The former displays numbering starting from the *arbitrarily provided value*, whereas the latter emphasizes the line numbers of the *source file*.

.. literalinclude:: src/helloworld.py
    :language: python
    :linenos:
    :lineno-start: 11
    :emphasize-lines: 1,6-7,9-


.. _dsg-pyobject:

``pyobject``
""""""""""""

``literalinclude`` also supports including only parts of a file.

If the source code is a Python module, you can select a class, function, or method to include using the ``pyobject`` option.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python
        :pyobject: hello_world

The above code renders as follows.
It returns the function ``hello_world`` in the source file.

.. literalinclude:: src/helloworld.py
    :language: python
    :pyobject: hello_world


.. _dsg-start-after-and-end-before:

``start-after`` and ``end-before``
""""""""""""""""""""""""""""""""""

Another way to control which part of the file is included is to use the ``start-after`` and ``end-before`` options (or only one of them).
If ``start-after`` is given as a string option, only lines that follow the first line containing that string are included.
If ``end-before`` is given as a string option, only lines that precede the first lines containing that string are included.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python
        :start-after: from pyramid.response import Response
        :end-before: if __name__ == '__main__':

The above code renders as follows.

.. literalinclude:: src/helloworld.py
    :language: python
    :start-after: from pyramid.response import Response
    :end-before: if __name__ == '__main__':


.. _dsg-lines:

``lines``
"""""""""

You can specify exactly which lines to include by giving a ``lines`` option.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python
        :lines: 6-7

The above code renders as follows.

.. literalinclude:: src/helloworld.py
    :language: python
    :lines: 6-7


.. _dsg-lineno-match:

``lineno-match``
""""""""""""""""

When specifying particular parts of a file to display, it can be useful to display exactly which lines are being presented.
This can be done using the ``lineno-match`` option.

.. code-block:: rst

    .. literalinclude:: src/helloworld.py
        :language: python
        :lines: 6-7
        :lineno-match:

The above code renders as follows.

.. literalinclude:: src/helloworld.py
    :language: python
    :lines: 6-7
    :lineno-match:

.. _dsg-recommendations:

Recommendations
"""""""""""""""

Out of all the ways to include parts of a file, ``pyobject`` is the most preferred option because if you change your code and add or remove lines, you don't need to adjust line numbering, whereas with ``lines`` you would have to adjust.
``start-after`` and ``end-before`` are less desirable because they depend on source code not changing.
Alternatively you can insert comments into your source code to act as the delimiters, but that just adds comments that have nothing to do with the functionality of your code.

Above all with includes, if you use line numbering, it's much preferred to use ``lineno-match`` over ``linenos`` with ``lineno-start`` because it "just works" without thinking and with less markup.


.. _dsg-parsed-literals:

Parsed literals
"""""""""""""""

Parsed literals are used to render, for example, a specific version number of the application in code blocks.
Use the directive ``parsed-literal``.
Note that syntax highlighting is not supported and code is rendered as plain text.

.. code-block:: rst

    .. parsed-literal::

        $VENV/bin/pip install "docs-style-guide==\ |release|\ "

The above code renders as follows.

.. parsed-literal::

    $VENV/bin/pip install "docs-style-guide==\ |release|\ "


.. _dsg-inline-code:

Inline code
"""""""""""

Inline code is surrounded by double backtick marks.

.. code-block:: rst

    Install requirements for building documentation: ``pip install -e ".[docs]"``

Renders as:

Install requirements for building documentation: ``pip install -e ".[docs]"``


.. _dsg-feature-versioning:

Feature versioning
------------------

Three directives designate the version in which something is added, changed, or deprecated in the project.

The first argument is the version.
An optional second argument must appear upon a subsequent line, without blank lines in between, and indented.


.. _dsg-version-added:

Version added
^^^^^^^^^^^^^

To indicate the version in which a feature is added to a project, use the ``versionadded`` directive.
If the feature is an entire module, then the directive should be placed at the top of the module section before any prose.

.. code-block:: rst

    .. versionadded:: 1.1
        :func:`pyramid.paster.bootstrap`

Renders as:

.. versionadded:: 1.1
    :func:`pyramid.paster.bootstrap`


.. _dsg-version-changed:

Version changed
^^^^^^^^^^^^^^^

To indicate the version in which a feature is changed in a project, use the ``versionchanged`` directive.
Its arguments are the same as ``versionadded``.

.. code-block:: rst

    .. versionchanged:: 1.8
        Added the ability for ``bootstrap`` to cleanup automatically via the ``with`` statement.

Renders as:

.. versionchanged:: 1.8
    Added the ability for ``bootstrap`` to cleanup automatically via the ``with`` statement.


.. _dsg-deprecated:

Deprecated
^^^^^^^^^^

Similar to ``versionchanged``, ``deprecated`` describes when the feature was deprecated.
An explanation may be given to inform the reader what should be used instead.

.. code-block:: rst

    .. deprecated:: 1.7
        Use the ``require_csrf`` option or read :ref:`auto_csrf_checking` instead to have :class:`pyramid.exceptions.BadCSRFToken` exceptions raised.

Renders as:

.. deprecated:: 1.7
    Use the ``require_csrf`` option or read :ref:`auto_csrf_checking` instead to have :class:`pyramid.exceptions.BadCSRFToken` exceptions raised.


.. _dsg-admonitions:

Admonitions
-----------

Admonitions are intended to bring special attention to the reader.


.. _dsg-danger:

Danger
^^^^^^

Danger represents critical information related to a topic or concept, and should recommend to the user "don't do this dangerous thing".
Use the ``danger`` or ``error`` directive.

.. code-block:: rst

    .. danger::

        This is danger or an error.

The above code renders as follows.

.. danger::

    This is danger or an error.

.. todo: The style for ``danger`` and ``error`` has not yet been created.


.. _dsg-warnings:

Warnings
^^^^^^^^

Warnings represent limitations and advice related to a topic or concept.


.. code-block:: rst

    .. warning::

        This is a warning.

The above code renders as follows.

.. warning::

    This is a warning.


.. _dsg-notes:

Notes
^^^^^

Notes represent additional information related to a topic or concept.

.. code-block:: rst

    .. note::

        This is a note.

The above code renders as follows.

.. note::

    This is a note.


.. _dsg-see-also:

See also
^^^^^^^^

"See also" messages refer to topics that are related to the current topic, but have a narrative tone to them instead of merely a link without explanation.

.. code-block:: rst

    .. seealso::

        See :ref:`Quick Tutorial section on Requirements <qtut_requirements>`.

The above code renders as follows.

.. seealso::

    See :ref:`Quick Tutorial section on Requirements <qtut_requirements>`.


.. _dsg-todo:

Todo
^^^^

Todo items designate tasks that require further work.

.. todo: The todo style is not yet implemented and needs further work.

.. code-block:: rst

    .. todo::

        This is a todo item.

This renders as the following.

.. todo::

    This is a todo item.

To display a list of all todos in the entire documentation, use the ``todolist`` directive.

.. code-block:: rst

    .. todolist::

In this document, the above renders as the following.

.. todolist::


.. _dsg-tables:

Tables
------

Two forms of tables are supported, `simple <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#simple-tables>`_ and `grid <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#grid-tables>`_.

Simple tables require less markup but have fewer features and some constraints compared to grid tables.
The right-most column in simple tables is unbound to the length of the underline in the column header.

.. code-block:: rst

    =====  =====
    col 1  col 2
    =====  =====
    1      Second column of row 1.
    2      Second column of row 2.
           Second line of paragraph.
    3      * Second column of row 3.

           * Second item in bullet
             list (row 3, column 2).
    \      Row 4; column 1 will be empty.
    =====  =====

Renders as:

=====  =====
col 1  col 2
=====  =====
1      Second column of row 1.
2      Second column of row 2.
       Second line of paragraph.
3      * Second column of row 3.

       * Second item in bullet
         list (row 3, column 2).
\      Row 4; column 1 will be empty.
=====  =====

Grid tables have much more cumbersome markup, although Emacs' table mode may lessen the tedium.

.. code-block:: rst

    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+----------+
    | body row 2             | Cells may span columns.          |
    +------------------------+------------+---------------------+
    | body row 3             | Cells may  | * Table cells       |
    +------------------------+ span rows. | * contain           |
    | body row 4             |            | * body elements.    |
    +------------------------+------------+---------------------+

The above code renders as follows.

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | Cells may span columns.          |
+------------------------+------------+---------------------+
| body row 3             | Cells may  | * Table cells       |
+------------------------+ span rows. | * contain           |
| body row 4             |            | * body elements.    |
+------------------------+------------+---------------------+


.. _dsg-topic:

Topic
-----

A topic is similar to a block quote with a title, or a self-contained section with no subsections.
A topic indicates a self-contained idea that is separate from the flow of the document.
Topics may occur anywhere a section or transition may occur.
For example:

.. code-block:: rst

    .. topic:: Topic Title

        Subsequent indented lines comprise
        the body of the topic, and are
        interpreted as body elements.

renders as:

.. topic:: Topic Title

    Subsequent indented lines comprise
    the body of the topic, and are
    interpreted as body elements.


.. _dsg-comments:

Comments
--------

Comments may appear in reST source, but they are not rendered to the output.
Comments are intended for documentation authors.

.. code-block:: rst

    .. This is a comment.

.. This is a comment.

.. seealso:: See also comments in :ref:`dsg-source-code`.


.. _dsg-font-styles:

Font styles
===========


.. _dsg-italics:

Italics
-------

.. code-block:: rst

    This *word* is italicized.

The above code renders as follows.

This *word* is italicized.


.. _dsg-strong:

Strong
------

.. code-block:: rst

    This **word** is in bold text.

The above code renders as follows.

This **word** is in bold text.


.. _dsg-keyboard-symbols:

Keyboard symbols
----------------

.. code-block:: rst

    Press :kbd:`Ctrl-C` (or :kbd:`Ctrl-Break` on Windows) to exit the server.

The above code renders as follows.

Press :kbd:`Ctrl-C` (or :kbd:`Ctrl-Break` on Windows) to exit the server.


.. _dsg-grammar-spelling-and-capitalization:

Grammar, spelling, and capitalization
=====================================

Use any commercial or free professional style guide in general.
Use a spell- and grammar-checker.
The following table lists the preferred grammar, spelling, and capitalization of words and phrases for frequently used items in documentation.

========================  =====
Preferred                 Avoid
========================  =====
add-on                    addon
ASCII                     ascii
and so on                 etc.
for example               e.g.
GitHub                    Github, github
in other words            i.e.
JavaScript                Javascript, javascript, js
JSON                      json
nginx                     Nginx
plug-in                   plugin
Pyramid                   pyramid
Python                    python
reST or reStructuredText  rst, restructuredtext
select                    check, tick (checkbox)
Setuptools                setuptools
such as                   like
Unicode                   unicode
Unix                      UNIX, unix
URL                       url
verify                    be sure
========================  =====


.. _dsg-sphinx-extensions:

Sphinx extensions
=================

We use several Sphinx extensions to add features to our documentation.
Extensions need to be enabled and configured in ``docs/conf.py`` before they can be used.


.. _dsg-sphinx-extension-autodoc:

:mod:`sphinx.ext.autodoc`
-------------------------

API documentation uses the Sphinx extension :mod:`sphinx.ext.autodoc` to include documentation from docstrings.

See the source of any documentation within the ``docs/api/`` directory for conventions and usage, as well as the Sphinx extension's :mod:`documentation <sphinx.ext.autodoc>`.
Live examples can be found in Pyramid's :ref:`api_documentation`.


.. _dsg-sphinx-extension-doctest:

:mod:`sphinx.ext.doctest`
-------------------------

:mod:`sphinx.ext.doctest` allows you to test code snippets in the documentation in a natural way.
It works by collecting specially-marked up code blocks and running them as doctest tests.
We have only a few tests in our Pyramid documentation which can be found in ``narr/sessions.rst`` and ``narr/hooks.rst``.
The following is an example.

.. code-block:: rst

    .. testsetup:: group1

        a = 1
        b = 2

    .. doctest:: group1

        >>> a + b
        3

And the rendered result.

.. testsetup:: group1

    a = 1
    b = 2

.. doctest:: group1

    >>> a + b
    3

When we run doctests, the output would be similar to the following.

.. code-block:: bash

    tox -e doctest

    # ...

    Document: index
    ---------------
    1 items passed all tests:
       1 tests in group1
    1 tests in 1 items.
    1 passed and 0 failed.
    Test passed.

    Doctest summary
    ===============
        1 test
        0 failures in tests
        0 failures in setup code
        0 failures in cleanup code
    build succeeded.

    Testing of doctests in the sources finished, look at the results in ../.tox/doctest/doctest/output.txt.


.. _dsg-sphinx-extension-intersphinx:

:mod:`sphinx.ext.intersphinx`
-----------------------------

:mod:`sphinx.ext.intersphinx` generates links to the documentation of objects in other projects.


.. _dsg-sphinx-extension-todo:

:mod:`sphinx.ext.todo`
----------------------

:mod:`sphinx.ext.todo` adds support for todo items.


.. _dsg-sphinx-extension-viewcode:

:mod:`sphinx.ext.viewcode`
--------------------------

:mod:`sphinx.ext.viewcode` looks at your Python object descriptions and tries to find the source files where the objects are contained.
When found, a separate HTML page will be output for each module with a highlighted version of the source code, and a link will be added to all object descriptions that leads to the source code of the described object.
A link back from the source to the description will also be inserted.
Live examples can be found in Pyramid's :ref:`api_documentation`.


.. _dsg-sphinx-extension-repoze-sphinx-autointerface:

`repoze.sphinx.autointerface <https://pypi.org/project/repoze.sphinx.autointerface/>`_
-----------------------------------------------------------------------------------------

`repoze.sphinx.autointerface <https://pypi.org/project/repoze.sphinx.autointerface/>`_ auto-generates API docs from Zope interfaces.


.. _dsg-script-documentation:

Script documentation
--------------------

We currently use `sphinxcontrib-autoprogram <https://pythonhosted.org/sphinxcontrib-autoprogram/>`_ to generate program output of Pyramid's :ref:`pscripts_documentation`.


.. _indices-and-tables:

Index, Glossary, and Search
===========================

* :ref:`genindex`
* :ref:`glossary`
* :ref:`search`

.. toctree::
   :hidden:

   glossary
