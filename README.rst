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

.. _AbstractRepository: source/archives/magento/AbstractRepository.rst
.. _AbstractRepositoryTrait: source/archives/magento/AbstractRepositoryTrait.rst
.. _AbstractRepositoryInterface: source/archives/magento/AbstractRepositoryInterface.rst
.. _AuthTrait: source/archives/magento/AuthTrait.rst
.. _CsrfAwareActionTrait: source/archives/magento/CsrfAwareActionTrait.rst
.. _DataObjectFactory: source/archives/magento/DataObjectFactory.rst
.. _DateTimeFactory: source/archives/magento/DateTimeFactory.rst
.. _EventManagerTrait: source/archives/magento/EventManagerTrait.rst
.. _ExceptionFactory: source/archives/magento/ExceptionFactory.rst
.. _ModifierPoolTrait: source/archives/magento/ModifierPoolTrait.rst
.. _RedirectTrait: source/archives/magento/RedirectTrait.rst
.. _Token: source/archives/magento/Token.rst
.. _VirtualSelect: source/archives/magento/VirtualSelect.rst
.. _VirtualGroupedSelect: source/archives/magento/VirtualGroupedSelect.rst

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
