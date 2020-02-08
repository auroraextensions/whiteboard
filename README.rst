Archive of whiteboard topics, mostly about Magento, PHP, OOP, design patterns, etc.

.. contents:: :local:

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

.. _AbstractRepository: source/topic/AbstractRepository.rst
.. _AbstractRepositoryTrait: source/topic/AbstractRepositoryTrait.rst
.. _AbstractRepositoryInterface: source/topic/AbstractRepositoryInterface.rst
.. _AuthTrait: source/topic/AuthTrait.rst
.. _CsrfAwareActionTrait: source/topic/CsrfAwareActionTrait.rst
.. _DataObjectFactory: source/topic/DataObjectFactory.rst
.. _DateTimeFactory: source/topic/DateTimeFactory.rst
.. _EventManagerTrait: source/topic/EventManagerTrait.rst
.. _ExceptionFactory: source/topic/ExceptionFactory.rst
.. _ModifierPoolTrait: source/topic/ModifierPoolTrait.rst
.. _RedirectTrait: source/topic/RedirectTrait.rst
.. _Token: source/topic/Token.rst
.. _VirtualSelect: source/topic/VirtualSelect.rst
.. _VirtualGroupedSelect: source/topic/VirtualGroupedSelect.rst

Controllers
^^^^^^^^^^^

* `AuthTrait`_
* `CsrfAwareActionTrait`_
* `RedirectTrait`_

Cryptography
^^^^^^^^^^^^

* `Token`_

Dependency Injection
^^^^^^^^^^^^^^^^^^^^

* `DataObjectFactory`_
* `DateTimeFactory`_
* `VirtualSelect`_
* `VirtualGroupedSelect`_

Events
^^^^^^

* `EventManagerTrait`_

Factories
^^^^^^^^^

* `DataObjectFactory`_
* `DateTimeFactory`_
* `ExceptionFactory`_

Redirects
^^^^^^^^^

* `RedirectTrait`_

Repositories
^^^^^^^^^^^^

* `AbstractRepository`_
* `AbstractRepositoryTrait`_
* `AbstractRepositoryInterface`_

Security
^^^^^^^^

* `AuthTrait`_
* `CsrfAwareActionTrait`_
* `Token`_

UI Components
^^^^^^^^^^^^^

* `ModifierPoolTrait`_
