# AuthTrait

## Table of Contents

+ [Related](#related)
+ [Description](#description)
+ [Usage](#usage)
+ [Source](#source)

## Related

+ [RedirectTrait](RedirectTrait.md)

## Description

Magento controllers often need to perform many of the same tasks, such as:

+ Check if user is logged in
+ Redirect user to different page
+ Set error messages

To simplify these tasks, it is helpful to compartmentalize tasks into compositional pieces
that can be reused. As such, traits make for the perfect vehicle to achieve such a goal.

In the example below, we've created a trait called `AuthTrait`, which can:

+ Check session to verify login status
+ Authenticate user if not logged in

## Usage

```php
<?php
/**
 * LoginPost.php
 */
declare(strict_types=1);

namespace Vendor\Package\Controller\Entity;

use Magento\Customer\{
    Api\AccountManagementInterface,
    Api\Data\CustomerInterface,
    Model\Session
};
use Magento\Framework\{
    App\Action\Action,
    App\Action\Context,
    App\Action\HttpPostActionInterface,
    App\CsrfAwareActionInterface,
    Data\Form\FormKey\Validator as FormKeyValidator,
    Stdlib\Cookie\PhpCookieManager,
    Stdlib\Cookie\CookieMetadataFactory
};
use Vendor\Package\Component\AuthTrait;

class LoginPost extends Action implements HttpPostActionInterface
{
    use AuthTrait;

    /** @property AccountManagementInterface $accountManagement */
    protected $accountManagement;

    /** @property PhpCookieManager $cookieManager */
    protected $cookieManager;

    /** @property CookieMetadataFactory $cookieMetadataFactory */
    protected $cookieMetadataFactory;

    /** @property Session $session */
    protected $session;

    /**
     * @param Context $context
     * @param AccountManagementInterface $accountManagement
     * @param CookieManager $cookieManager
     * @param CookieMetadataFactory $cookieMetadataFactory
     * @param FormKeyValidator $formKeyValidator
     * @param Session $session
     * @return void
     */
    public function __construct(
        Context $context,
        AccountManagementInterface $accountManagement,
        CookieManager $cookieManager,
        CookieMetadataFactory $cookieMetadataFactory,
        FormKeyValidator $formKeyValidator,
        Session $session
    ) {
        parent::__construct($context);
        $this->accountManagement = $accountManagement;
        $this->cookieManager = $cookieManager;
        $this->cookieMetadataFactory = $cookieMetadataFactory;
        $this->formKeyValidator = $formKeyValidator;
        $this->session = $session;
    }

    /**
     * @return Redirect
     */
    public function execute()
    {
        /** @var Magento\Framework\App\RequestInterface $request */
        $request = $this->getRequest();

        /** @var Redirect $resultRedirect */
        $resultRedirect = $this->resultRedirectFactory->create();

        if ($this->isAuth() || !$this->hasValidFormKey()) {
            $resultRedirect->setPath('*/*/');

            return $resultRedirect;
        }

        /** @var CustomerInterface|null $customer */
        $customer = $this->auth();

        if ($customer !== null) {
            /* Set cookies, set redirects, etc. */

            ...
        }

        return $resultRedirect;
    }
}
```

## Source

```php
<?php
/**
 * AuthTrait.php
 */
declare(strict_types=1);

namespace Vendor\Package\Component;

use Magento\Customer\Api\Data\CustomerInterface;
use Magento\Framework\{
    Exception\EmailNotConfirmedException,
    Exception\InvalidEmailOrPasswordException,
    Exception\UserLockedException
};

trait AuthTrait
{
    /**
     * Determine if user is authenticated.
     *
     * @return bool
     */
    public function isAuth(): bool
    {
        return (bool) $this->session->isLoggedIn();
    }

    /**
     * @return bool
     */
    public function hasValidFormKey(): bool
    {
        return (bool) $this->formKeyValidator->validate($this->getRequest());
    }

    /**
     * @return string|null
     */
    public function getUsername(): ?string
    {
        /** @var array $login */
        $login = $this->getRequest()->getPost('login');

        if (!empty($login)) {
            return !empty($login['username']) ? $login['username'] : null;
        }

        return null;
    }

    /**
     * @return string|null
     */
    public function getPassword(): ?string
    {
        /** @var array $login */
        $login = $this->getRequest()->getPost('login');

        if (!empty($login)) {
            return !empty($login['password']) ? $login['password'] : null;
        }

        return null;
    }

    /**
     * @return bool
     */
    public function isCredentialsGiven(): bool
    {
        /** @var string|null $username */
        $username = $this->getUsername();

        /** @var string|null $password */
        $password = $this->getPassword();

        return ($username !== null && $password !== null);
    }

    /**
     * @return CustomerInterface|null
     */
    public function auth(): ?CustomerInterface
    {
        try {
            /** @var CustomerInterface $customer */
            $customer = $this->accountManagement->authenticate(
                $this->getUsername(),
                $this->getPassword()
            );
            $this->session->setCustomerDataAsLoggedIn($customer);
            $this->sesssion->regenerateId();

            return $customer;
        } catch (InvalidEmailOrPasswordException $e) {
            /* Set error message, return value, etc. */
        } catch (UserLockedException $e) {
            /* Set error message, return value, etc. */
        } catch (EmailNotConfirmedException $e) {
            /* Set error message, return value, etc. */
        }

        return null;
    }
}
```
