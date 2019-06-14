# ExceptionFactory

There are many benefits to using factories in Magento 2, such as flexible object
creation. As such, factories have become widely adopted and utilized in Magento 2,
and can be seen as one of the most significant improvements over Magento 1. However,
it's noticeable that exception creation practices have yet to embrace factories.

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
    )
    {
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
                    self::ERROR_INVALID_EXCEPTION_TYPE,
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
