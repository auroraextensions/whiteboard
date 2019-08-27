# CsrfAwareActionTrait

## Table of Contents

+ [Related](#related)
+ [Description](#description)
+ [Usage](#usage)
+ [Source](#source)

## Related

+ [AuthTrait](AuthTrait.md)
+ [RedirectTrait](RedirectTrait.md)

## Description

Magento offers the `CsrfAwareActionInterface` interface, which is used to
validate requests against CSRF attacks. In the example below, we've created
a trait called `CsrfAwareActionTrait`, which implements this interface.

## Usage

```php
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
```

## Source

```php
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
```
