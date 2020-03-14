ExceptionFactory
================

* Published: 2019-06-14
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Related
-------

* :doc:`DataObjectFactory`
* :doc:`DateTimeFactory`

Description
-----------

There are several benefits to using factories in Magento, namely flexible, stateless object
creation. As such, factories have become widely adopted and utilized in Magento 2, and can
be seen as one of the most significant architectural improvements over Magento 1. However,
it's noticeable that exception creation practices have yet to embrace factories.

In the example below, we introduce the ``ExceptionFactory`` class, which can be used to create
``Throwable`` [#ref1]_ exception types. The reasons for defining this class statically include:

1. Enforce ``$type`` argument to implement ``Throwable`` interface
2. Allow any ``Throwable`` exception type, instead of single exception type

.. note::

    The ``ExceptionFactory`` class will only create an exception type. It is the
    responsibility of the invoking method to throw the created exception instance.

Usage
-----

.. code-block:: php

    <?php
    ...

    /** @var EntityInterface $entity */
    $entity = $this->entityFactory->create();
    $this->entityResource->load(
        $entity,
        $id
    );

    if (!$entity->getId()) {
        /** @var NoSuchEntityException $exception */
        $exception = $this->exceptionFactory->create(
          NoSuchEntityException::class,
          __('No such entity found.')
        );

        throw $exception;
    }
    ...

Source
------

Below is an example ``ExceptionFactory`` that can be used in modules:

.. code-block:: php

    <?php
    /**
     * ExceptionFactory.php
     *
     * Factory for generating various exception types.
     * Requires ObjectManager for instance generation.
     *
     * @link https://devdocs.magento.com/guides/v2.3/extension-dev-guide/factories.html
     */
    declare(strict_types=1);

    namespace Vendor\Package\Exception;

    use Exception;
    use Magento\Framework\{
        ObjectManagerInterface,
        Phrase
    };
    use Throwable;

    class ExceptionFactory
    {
        /** @constant string BASE_TYPE */
        public const BASE_TYPE = Exception::class;

        /** @constant string DEFAULT_ERROR */
        public const DEFAULT_ERROR = 'An error was encountered.';

        /** @property ObjectManagerInterface $objectManager */
        protected $objectManager;

        /**
         * @param ObjectManagerInterface $objectManager
         * @return void
         */
        public function __construct(
            ObjectManagerInterface $objectManager
        ) {
            $this->objectManager = $objectManager;
        }

        /**
         * @param string|null $type
         * @param Phrase|null $message
         * @return mixed
         * @throws Exception
         */
        public function create(
            ?string $type = self::BASE_TYPE,
            ?Phrase $message = null
        ) {
            /** @var array $arguments */
            $arguments = [];

            /* If no message was given, set default message. */
            $message = $message ?? __(self::ERROR_DEFAULT);

            if (!is_subclass_of($type, Throwable::class)) {
                throw new Exception(
                    __(
                        'Invalid exception type %1 was given.',
                        $type
                    )->__toString()
                );
            }

            if ($type !== self::BASE_TYPE) {
                $arguments['message'] = $message->__toString();
            } else {
                $arguments['phrase'] = $message;
            }

            return $this->objectManager->create($type, $arguments);
        }
    }

Notes
-----

.. [#ref1] The `Throwable` interface is a PHP built-in interface and can only be implemented via extending `Exception`.
