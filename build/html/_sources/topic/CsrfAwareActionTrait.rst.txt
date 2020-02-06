.. contents:: Table of Contents
    :depth: 2

CsrfAwareActionTrait
====================

* *Published*: 2019-08-27
* *Author*: Nickolas Burr

Related
-------

* :doc:`AuthTrait`
* :doc:`RedirectTrait`

Description
-----------

Starting in v2.3.0, Magento provides the ``CsrfAwareActionInterface`` [#ref1]_
interface, which is used to validate requests against CSRF attacks. In most cases,
default validation is adequate, so the implementation is identical across the vast
majority of controllers.

In the example below, we've created a trait called ``CsrfAwareActionTrait``, which
effectively implements ``CsrfAwareActionInterface`` with default validation.

Usage
-----

.. code-block:: php

    <?php
    /**
     * LoginPost.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Controller\Entity;

    use Magento\Framework\{
        App\Action\Action,
        App\Action\Context,
        App\Action\HttpPostActionInterface,
        App\CsrfAwareActionInterface
    };
    use Vendor\Package\Component\CsrfAwareActionTrait;

    class LoginPost extends Action implements
        CsrfAwareActionInterface,
        HttpPostActionInterface
    {
        use CsrfAwareActionTrait;

        /**
         * @param Context $context
         * @return void
         */
        public function __construct(
            Context $context
        ) {
            parent::__construct($context);
        }

        /**
         * @return Redirect
         */
        public function execute()
        {
            ...
        }
    }

Source
------

.. code-block:: php

    <?php
    /**
     * CsrfAwareActionTrait.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Component;

    use Magento\Framework\{
        App\RequestInterface,
        App\Request\InvalidRequestException
    };

    trait CsrfAwareActionTrait
    {
        /**
         * @param RequestInterface $request
         * @return InvalidRequestException|null
         */
        public function createCsrfValidationException(
            RequestInterface $request
        ): ?InvalidRequestException
        {
            /** @var Redirect $resultRedirect */
            $resultRedirect = $this->resultRedirectFactory->create();
            $resultRedirect->setPath('*/*/');

            return new InvalidRequestException(
                $resultRedirect,
                [
                    __('Invalid Form Key. Please refresh the page.')
                ]
            );
        }

        /**
         * @param RequestInterface $request
         * @return bool|null
         */
        public function validateForCsrf(RequestInterface $request): ?bool
        {
            return null;
        }
    }

Notes
-----

.. [#ref1] `Magento\\\Framework\\\App\\\CsrfAwareActionInterface <https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/App/CsrfAwareActionInterface.php>`_
