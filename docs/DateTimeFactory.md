# DateTimeFactory

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)

## Related

- [DataObjectFactory](DataObjectFactory.md)
- [ExceptionFactory](ExceptionFactory.md)

## Description

The `DateTime` class is a built-in PHP class used extensively throughout Magento.
In fact, Magento has several wrapper classes to assist with creating `DateTime`
objects, as well as formatting subsequent date/time values. Such classes include:

- `Magento\Framework\Stdlib\DateTime`
- `Magento\Framework\Stdlib\DateTime\DateTime`
- `Magento\Framework\Stdlib\DateTime\DateTimeFormatter`

In the example below, we use `DateTimeFactory` to create `DateTime` objects, which
we'll use to modify the `updated_at` timestamp of an entry when a user makes an edit.

## Usage

```php
<?php
/**
 * EditPost.php
 */
declare(strict_types=1);

namespace Vendor\Package\Controller\Entity;

use DateTime;
use DateTimeFactory;
use Magento\Framework\{
    App\Action\Action,
    App\Action\Context,
    App\Action\HttpPostActionInterface,
    App\RequestInterface,
    Data\Form\FormKey\Validator as FormKeyValidator,
    Exception\LocalizedException,
    Exception\NoSuchEntityException,
    Stdlib\DateTime as StdlibDateTime
};
use Vendor\Package\{
    Api\EntityRepositoryInterface,
    Api\Data\EntityInterface
};

class EditPost extends Action implements HttpPostActionInterface
{
    /** @property DateTimeFactory $dateTimeFactory */
    protected $dateTimeFactory;

    /** @property EntityRepositoryInterface $entityRepository */
    protected $entityRepository;

    /** @property FormKeyValidator $formKeyValidator */
    protected $formKeyValidator;

    /**
     * @param Context $context
     * @param DateTimeFactory $dateTimeFactory
     * @param EntityRepositoryInterface $entityRepository
     * @param FormKeyValidator $formKeyValidator
     * @return void
     */
    public function __construct(
        Context $context,
        DateTimeFactory $dateTimeFactory,
        EntityRepositoryInterface $entityRepository,
        FormKeyValidator $formKeyValidator
    ) {
        parent::__construct($context);
        $this->dateTimeFactory = $dateTimeFactory;
        $this->entityRepository = $entityRepository;
        $this->formKeyValidator = $formKeyValidator;
    }

    /**
     * @return Magento\Framework\Controller\Result\Redirect
     */
    public function execute()
    {
        /** @var RequestInterface $request */
        $request = $this->getRequest();

        /** @var Redirect $resultRedirect */
        $resultRedirect = $this->resultRedirectFactory->create();

        if (!$request->isPost() || !$this->formKeyValidator->validate($request)) {
            $resultRedirect->setPath('*/*/edit');

            return $resultRedirect;
        }

        /** @var int|string|null $entityId */
        $entityId = $request->getParam('entity_id');
        $entityId = $entityId !== null && is_numeric($entityId)
            ? (int) $entityId
            : null;

        if ($entityId !== null) {
            try {
                /** @var EntityInterface $entity */
                $entity = $this->entityRepository->getById($entityId);

                $this->entityRepository->save(
                    $entity->setUpdatedAt($this->dateTimeFactory->create())
                );
            } catch (NoSuchEntityException $e) {
                $this->messageManager->addErrorMessage($e->getMessage());
            } catch (LocalizedException $e) {
                $this->messageManager->addErrorMessage($e->getMessage());
            }
        }

        $resultRedirect->setPath('*/*/index');

        return $resultRedirect;
    }
}
```
