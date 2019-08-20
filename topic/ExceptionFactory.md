# ExceptionFactory

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)
- [Source](#source)
- [Footnotes](#footnotes)

## Related

- [DataObjectFactory](DataObjectFactory.md)
- [DateTimeFactory](DateTimeFactory.md)

## Description

There are several benefits to using factories in Magento, namely flexible, stateless object
creation. As such, factories have become widely adopted and utilized in Magento 2, and can
be seen as one of the most significant architectural improvements over Magento 1. However,
it's noticeable that exception creation practices have yet to embrace factories.

In the example below, we introduce the `ExceptionFactory` class, which can be used to create
`Throwable` exception types. The reasons for defining this class statically include:

1. Enforce `$type` argument to implement [`Throwable`](https://www.php.net/manual/en/class.throwable.php)<sup><a href="#footnotes">1</a></sup> interface
2. Allow any `Throwable` exception type, instead of single exception type

Please note: The `ExceptionFactory` class is only creating an exception type. It is the
responsibility of the invoking method to throw the created exception instance.

## Usage

```php
...
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
                )->__toString()
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

## Footnotes

1. The `Throwable` interface is a PHP built-in interface and can only be implemented via extending `Exception`.
