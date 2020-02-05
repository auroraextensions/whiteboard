Archive of whiteboard topics, mostly about Magento, PHP, OOP, design patterns, etc.

.. toctree::
    :maxdepth: 2
    :titlesonly:

Use Cases
=========

Magento
-------

* Create ``select`` source model with data from ``di.xml``
* Create categorized ``select`` source model with data from ``di.xml``
* Generate token for user authentication
* Generate nonce for reset link
* Update an entry timestamp via ``DateTime`` factory
* Deduplicate redirect logic between inheritance-based controllers
* Implement CSRF-aware request validation in controllers
* Dispatch ``before``, ``after`` events while saving an entity
* Modify metadata/data via data modifiers in an entity data provider

Topics
======

Magento
-------

Controllers
^^^^^^^^^^^

* :doc:`topic/AuthTrait`
* :doc:`topic/CsrfAwareActionTrait`
* :doc:`topic/RedirectTrait`

Cryptography
^^^^^^^^^^^^

* :doc:`topic/Token`

Dependency Injection
^^^^^^^^^^^^^^^^^^^^

* :doc:`topic/DataObjectFactory`
* :doc:`topic/DateTimeFactory`
* :doc:`topic/VirtualGroupedSelect`
* :doc:`topic/VirtualSelect`

Events
^^^^^^

* :doc:`topic/EventManagerTrait`

Factories
^^^^^^^^^

* :doc:`topic/DataObjectFactory`
* :doc:`topic/DateTimeFactory`
* :doc:`topic/ExceptionFactory`

Redirects
^^^^^^^^^

* :doc:`topic/RedirectTrait`

Repositories
^^^^^^^^^^^^

* :doc:`topic/AbstractRepository`
* :doc:`topic/AbstractRepositoryTrait`
* :doc:`topic/AbstractRepositoryInterface`

Security
^^^^^^^^

* :doc:`topic/AuthTrait`
* :doc:`topic/CsrfAwareActionTrait`
* :doc:`topic/Token`

UI Components
^^^^^^^^^^^^^

* :doc:`topic/ModifierPoolTrait`
