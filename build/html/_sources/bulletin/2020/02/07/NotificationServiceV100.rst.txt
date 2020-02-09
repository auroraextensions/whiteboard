Notification Service v1.0.0
===========================

* Published: 2020-02-07
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

* `Repository <https://github.com/auroraextensions/notificationservice>`_
* `Documentation <https://docs.auroraextensions.com/magento/extensions/2.x/notificationservice/latest/>`_
* `CHANGELOG.txt <https://docs.auroraextensions.com/magento/extensions/2.x/notificationservice/CHANGELOG.txt>`_

Summary
-------

Magento provides a backend notification system that lacks powerful features, like auto-publish
and discovery. Consequently, many extension vendors don't effectively utilize it for important
announcements, like API-breaking releases, EOL notices, or deprecation of specific extension
features, which can lead to transparency issues.

To address this issue, wouldn't it be nice if you could just add, say, an XML file to your module,
which contains the notifications you want to publish, and the rest is handled by the notification
system? It would remove the need for notification-specific models and data patches, and would not
require setup upgrade to run in order for new notifications to publish.

*Bottom line*: It should be dead simple to use.

That was exactly what we wanted, so we built it. Now, instead of bloating our modules with one-off
scripts and extraneous classes, we just add a ``notifications.xml`` file to our module, update it
as needed, and we no longer have to stress about backend notifications.

Notification Service v1.0.0 is now available on GitHub.

Downloads
---------

* `auroraextensions_notificationservice-1.0.0.zip <https://github.com/auroraextensions/notificationservice/archive/1.0.0.zip>`_
* `auroraextensions_notificationservice-1.0.0.tar.gz <https://github.com/auroraextensions/notificationservice/archive/1.0.0.tar.gz>`_
