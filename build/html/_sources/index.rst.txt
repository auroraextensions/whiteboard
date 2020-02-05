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

* :doc:`AuthTrait <topic/AuthTrait>`_
* :doc:`CsrfAwareActionTrait <topic/CsrfAwareActionTrait>`_
* :doc:`RedirectTrait <topic/RedirectTrait>`_

Cryptography
^^^^^^^^^^^^

* :doc:`Token <topic/Token>`_

Dependency Injection
^^^^^^^^^^^^^^^^^^^^

* :doc:`DataObjectFactory <topic/DataObjectFactory>`_
* :doc:`DateTimeFactory <topic/DateTimeFactory>`_
* :doc:`VirtualGroupedSelect <topic/VirtualGroupedSelect>`_
* :doc:`VirtualSelect <topic/VirtualSelect>`_

Events
^^^^^^

* :doc:`EventManagerTrait <topic/EventManagerTrait>`_

Factories
^^^^^^^^^

* :doc:`DataObjectFactory <topic/DataObjectFactory>`_
* :doc:`DateTimeFactory <topic/DateTimeFactory>`_
* :doc:`ExceptionFactory <topic/ExceptionFactory>`_

Redirects
^^^^^^^^^

* :doc:`RedirectTrait <topic/RedirectTrait>`_

Repositories
^^^^^^^^^^^^

* :doc:`AbstractRepository <topic/AbstractRepository>`_
* :doc:`AbstractRepositoryTrait <topic/AbstractRepositoryTrait>`_
* :doc:`AbstractRepositoryInterface <topic/AbstractRepositoryInterface>`_

Security
^^^^^^^^

* :doc:`AuthTrait <topic/AuthTrait>`_
* :doc:`CsrfAwareActionTrait <topic/CsrfAwareActionTrait>`_
* :doc:`Token <topic/Token>`_

UI Components
^^^^^^^^^^^^^

* :doc:`ModifierPoolTrait <topic/ModifierPoolTrait>`_
