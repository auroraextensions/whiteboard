Tokenize User Authentication v1.2.0
===================================

* Published: 2020-02-06
* Author: Nickolas Burr

.. note::

    If you would like to receive updates (like this one) in the form of an email, please
    consider subscribing to our quarterly newsletter. It is, by definition, low volume,
    and we include details about upcoming releases, product updates, EOL announcements,
    and other related topics. You can sign up `here <https://auroraextensions.com/>`_.

Summary
-------

Tokenize User Authentication v1.2.0 is set to release early next week, and it's
packed full of improvements and new features, including support for multi-factor
authentication, which we discuss below in `MFA`_.

Highlights
^^^^^^^^^^

* Magento 2.3.4 support
* Deprecated all Helper classes
* Improved token validation framework
* Multi-factor authentication support

MFA
^^^

Over the past several years, multi-factor authentication has become the gold standard
for reducing unauthorized user account access. Relying on password-based authentication
alone is simply not adequate anymore, yet so many merchants and agencies have not yet
adopted measures to incorporate MFA into their user authentication workflows.

Prior to v1.2.0, Tokenize User Authentication was not considered an MFA extension, as
there was only a single (albeit external) point of authentication. After thoughtful
consideration, we decided to add support for MFA, as we believe it is an important
feature that substantially improves account security and provides real value to both
merchants and agencies.

In v1.2.0, you have the option to enable/disable MFA for administrators and customers.
When enabled, administrators/customers will be required to create a numeric PIN when
they register their accounts, and will need to provide their PIN each time they attempt
to access their account.

Changelog
---------

[1.2.0] ~ 2020-02-07
^^^^^^^^^^^^^^^^^^^^

Added
*****

* Add PinInterface
* Add PinSearchResultsInterface
* Add PinRepositoryInterface
* Add db_schema.xml
* Add AbstractRepositoryTrait component trait
* Add AbstractCollectionInterface
* Add PinCollectionInterface
* Add PinResourceInterface
* Add PinRepository model
* Add AbstractResourceTrait component trait
* Add AbstractCollectionTrait component trait
* Add AbstractPinTrait component trait
* Add Customer Pin model
* Add Customer Pin resource model
* Add Customer Pin collection model
* Add TokenCollectionInterface component interface
* Add TokenResourceInterface
* Add DataContainerInterface component interface
* Add DataContainerTrait component trait
* Add ModuleConfigInterface component interface
* Add ModuleConfig model
* Add adminhtml_pin_validate controller
* Add adminhtml_pin_validate XML layout
* Add adminhtml_pin_validate template
* Add adminhtml_pin_validate view model
* Add adminhtml default XML layout
* Add adminhtml normalize.css
* Add adminhtml_pin_validatePost controller
* Add adminhtml_pin_create controller
* Add adminhtml_pin_create XML layout
* Add adminhtml_pin_create view model
* Add adminhtml_pin_createPost controller
* Add User PIN entity model
* Add User PIN resource model
* Add User PIN collection model
* Add User resource model Pin::getEntityIdColumn() method
* Add Customer resource model Pin::getEntityIdColumn() method
* Add PinValidatorInterface component interface
* Add TokenValidatorInterface component interface
* Add InvalidPinException
* Add User PIN validator model
* Add User token validator model
* Add ModuleConfig::getAdminExpirationPeriod() method
* Add ModuleConfig::isCustomerPinRequired() method
* Add area-specific <preference> for PIN, token validator interfaces
* Add PIN classes to action whitelist in Customer Router class
* Add customer_pin_create controller
* Add customer_pin_create XML layout
* Add customer_pin_create template
* Add customer_pin_create view model
* Add RedirectTrait component trait
* Add PIN requirement check to customer_token_validate controller
* Add customer_pin_createPost controller
* Add ModuleConfig::getConfigValue() method
* Add TokenValidatorInterface::validate() method signature
* Add PinValidatorInterface::validate() method signature
* Add Customer token validator model
* Add Customer PIN validator model
* Add ModuleConfig::getCustomerExpirationPeriod() method
* Add customer_pin_validate controller
* Add customer_pin_validate XML layout
* Add customer_pin_validate template
* Add customer_pin_validate view model
* Add customer_pin_validatePost controller
* Add $params argument to RedirectTrait::getRedirectToPath() method

Changed
*******

* Move TokenRepository into Repository/ model directory
* Change TokenRepositoryInterface::save() return type
* Replace User config helper in adminhtml Token validation controller
* Rename processUnauthenticatedLoginRequest() method(s)

Fixed
*****

* Fix missing redirect URL params in customer_token_validate controller
* Improve exception handling in backend authentication plugin

Deprecated
**********

* Deprecate Shared\\\ModuleComponentInterface
* Deprecate Plugin\\\Backend\\\Authentication plugin
* Deprecate all helper classes:
    * Helper\\\Action
    * Helper\\\Customer
    * Helper\\\Data
    * Helper\\\Dict
    * Helper\\\Email\\\AbstractTransport
    * Helper\\\Email\\\Transport\\\Customer
    * Helper\\\Email\\\Transport\\\User
    * Helper\\\Input\\\Sanitizer
    * Helper\\\Input\\\Validator
    * Helper\\\State\\\Manager
    * Helper\\\Token
    * Helper\\\User

Removed
*******

* Remove "Forgot Password" link from admin login page
* Remove Model\\\Token\\\ResourceModel\\\Token\\\CollectionInterface
* Remove Model\\\Token\\\ResourceModel\\\TokenInterface
