RedirectTrait
=============

* Published: 2019-08-21
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Related
-------

* `AuthTrait <AuthTrait>`_
* `CsrfAwareActionTrait <CsrfAwareActionTrait>`_

Description
-----------

In Magento, controllers inherit from ``Magento\Framework\App\Action\Action`` [#ref1]_.
This provides access to several methods (i.e. ``getRequest``, ``getResponse``) which are
essential for tasks like processing request parameters and setting response status. As
such, all controllers will inherit the ``$resultRedirectFactory`` [#ref2]_ property,
which, unsurprisingly, is needed to create redirects.

In the example below, we've created a trait called ``RedirectTrait``, which we can import
in our controllers. There are several advantages to this approach, such as:

1. Reduced duplication
2. Improved durability
3. Smaller ``execute`` methods
4. Single source of truth

Usage
-----

.. code-block:: php

    <?php
    /**
     * CreatePost.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Controller\Entity;

    use Magento\Framework\{
        App\Action\Action,
        App\Action\Context,
        App\Action\HttpPostActionInterface,
        App\CsrfAwareActionInterface,
        Data\Form\FormKey\Validator as FormKeyValidator
    };
    use Vendor\Package\Component\RedirectTrait;

    class CreatePost extends Action implements
        CsrfAwareActionInterface,
        HttpPostActionInterface
    {
        use RedirectTrait;

        /** @property FormKeyValidator $formKeyValidator */
        protected $formKeyValidator;

        /**
         * @param Context $context
         * @param FormKeyValidator $formKeyValidator
         * @return void
         */
        public function __construct(
            Context $context,
            FormKeyValidator $formKeyValidator
        ) {
            parent::__construct($context);
            $this->formKeyValidator = $formKeyValidator;
        }

        /**
         * @return Redirect
         */
        public function execute()
        {
            /** @var Magento\Framework\App\RequestInterface $request */
            $request = $this->getRequest();

            if (!$request->isPost() || !$this->formKeyValidator->validate($request)) {
                return $this->getRedirectToPath('*/*/create');
            }

            ...
        }
    }

Source
------

.. code-block:: php

    <?php
    /**
     * RedirectTrait.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Component;

    use Magento\Framework\{
        App\Action\AbstractAction,
        Controller\Result\Redirect
    };

    trait RedirectTrait
    {
        /**
         * @return Redirect
         */
        public function getRedirect(): Redirect
        {
            return $this->resultRedirectFactory->create();
        }

        /**
         * @param string $path
         * @return Redirect
         */
        public function getRedirectToPath(string $path = '*'): Redirect
        {
            /** @var Redirect $redirect */
            $redirect = $this->getRedirect();
            $redirect->setPath($path);

            return $redirect;
        }

        /**
         * @param string $url
         * @return Redirect
         */
        public function getRedirectToUrl(string $url = '*'): Redirect
        {
            /** @var Redirect $redirect */
            $redirect = $this->getRedirect();
            $redirect->setUrl($url);

            return $redirect;
        }
    }

Notes
-----

.. |link1| replace:: ``Magento\Framework\App\Action\Action``
.. |link2| replace:: ``$resultRedirectFactory``
.. _link1: https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/App/Action/Action.php
.. _link2: https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/App/Action/AbstractAction.php#L28

.. [#ref1] |link1|_
.. [#ref2] |link2|_
