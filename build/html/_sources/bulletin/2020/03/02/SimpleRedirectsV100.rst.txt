Simple Redirects v1.0.0
=======================

* Published: 2020-03-02
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

* `Repository <https://github.com/auroraextensions/simpleredirects>`_
* `Documentation <https://docs.auroraextensions.com/magento/extensions/2.x/simpleredirects/latest/>`_
* `CHANGELOG.txt <https://docs.auroraextensions.com/magento/extensions/2.x/simpleredirects/CHANGELOG.txt>`_

Summary
-------

Magento has fairly limited flexibility with respect to URL redirect management. The URL rewrite module
``Magento_UrlRewrite``, as well as related modules ``Magento_CatalogUrlRewrite`` and ``Magento_CmsUrlRewrite``,
provide the ability to redirect and rewrite URLs for products, categories, and CMS pages, which is extremely useful
for SEO purposes. However, there's a lack of functionality for simple rule-based redirects, which can be especially
helpful when transitioning to Magento from another platform like Shopify or BigCommerce.

To bridge this gap, we're releasing Simple Redirects, our rule-based URL redirect management module for Magento.
We're hopeful Simple Redirects will cover the vast majority of URL redirect scenarios and that it'll be useful
to the wider Magento community. Key features include:

1. Rule types:
    a. Path
    b. Query Parameter
    c. Host
2. Match types:
    a. Exact match
    b. Not exact match
    c. Contains
    d. Does not contain
    e. Matches regex
    f. Does not match regex
3. Redirect types:
    a. 301
    b. 302
4. Priority levels 1-10

Simple Redirects v1.0.0 is now available on GitHub.

Downloads
---------

* `auroraextensions_simpleredirects-1.0.0.zip <https://github.com/auroraextensions/simpleredirects/archive/1.0.0.zip>`_
* `auroraextensions_simpleredirects-1.0.0.tar.gz <https://github.com/auroraextensions/simpleredirects/archive/1.0.0.tar.gz>`_
