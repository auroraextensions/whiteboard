libxstring v1.0.0
=================

* Published: 2020-03-03
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

.. note::

    If you would like to receive updates (like this one) by email, please consider
    subscribing to our quarterly newsletter. It is low volume, and includes details
    about upcoming releases, product updates, EOL announcements, and other related
    topics. You can subscribe `here <https://auroraextensions.com/>`_.

Links
-----

* `Repository <https://github.com/auroraextensions/libxstring>`_

Summary
-------

Parsing and manipulating strings is an integral part of building software, especially
software in the context of web-based applications. In PHP, frequent tasks can include:

* Trim whitespace from string
* Split string into array of substrings
* Join array of strings into single ``string`` value

These tasks are so inherently common that if you scrutinize any PHP framework of choice,
you're bound to find many instances of expressions like:

* ``trim($str, '/')``
* ``explode('_', $str)``
* ``implode('/', $arr)``

To be clear, there is absolutely nothing wrong with any of those expressions, fundamental
or otherwise. However, if you look closely, you will notice a pattern, which is that the
overwhelming majority of instances are trimming, splitting, joining, etc. the same maybe
four or so characters, like ``/``, ``\\``, ``-``, and ``_``. These tasks were frequent
enough that we desired character-specific functions for each of these use cases, so we
put together a compact library to achieve just that.

libxstring v1.0.0 is now available on GitHub.

Downloads
---------

* `auroraextensions_libxstring-1.0.0.zip <https://github.com/auroraextensions/libxstring/archive/1.0.0.zip>`_
* `auroraextensions_libxstring-1.0.0.tar.gz <https://github.com/auroraextensions/libxstring/archive/1.0.0.tar.gz>`_
