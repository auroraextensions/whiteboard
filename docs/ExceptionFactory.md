# ExceptionFactory

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)
- [Source](#source)

## Related

- [DataObjectFactory](DataObjectFactory.md)

## Description

There are many benefits to using factories in Magento 2, such as flexible object
creation. As such, factories have become widely adopted and utilized in Magento 2,
and can be seen as one of the most significant improvements over Magento 1. However,
it's noticeable that exception creation practices have yet to embrace factories.

## Usage

The `ExceptionFactory` is only for creating, not throwing, an exception. For example:

```php
...
$entity = $this->entityFactory->create();
$this->entityResource->load(
    $entity,
    $id
);

if (!$entity->getId()) {
    /** @var mixed $exception */
    $exception = $this->exceptionFactory->create(
      NoSuchEntityException::class,
      __('No such entity found.')
    );

    throw $exception;
}
...
```

## Source

Below is an example `ExceptionFactory` that could be used in modules:

```php
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

use Magento\Framework\{
    ObjectManagerInterface,
    Phrase,
    PhraseFactory
};

class ExceptionFactory
{
    /** @constant string BASE_TYPE */
    const BASE_TYPE = \Exception::class;

    /** @constant string DEFAULT_ERROR */
    const DEFAULT_ERROR = 'An error was encountered.';

    /** @property ObjectManagerInterface $objectManager */
    protected $objectManager;

    /** @property PhraseFactory $phraseFactory */
    protected $phraseFactory;

    /**
     * @param ObjectManagerInterface $objectManager
     * @param PhraseFactory $phraseFactory
     * @return void
     */
    public function __construct(
        ObjectManagerInterface $objectManager,
        PhraseFactory $phraseFactory
    ) {
        $this->objectManager = $objectManager;
        $this->phraseFactory = $phraseFactory;
    }

    /**
     * Create exception from type given.
     *
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

        if ($type !== self::BASE_TYPE && !is_subclass_of($type, self::BASE_TYPE)) {
            throw new \Exception(
                __(
                    'Invalid exception class type %1 was given.',
                    $type
                )
            );
        }

        /* If no message was given, set default message. */
        $message = $message ?? $this->phraseFactory->create(
            [
                'text' => self::DEFAULT_ERROR,
            ]
        );

        if (!is_subclass_of($type, self::BASE_TYPE)) {
            $arguments['message'] = $message->__toString();
        } else {
            $arguments['phrase'] = $message;
        }

        return $this->objectManager->create($type, $arguments);
    }
}
```
