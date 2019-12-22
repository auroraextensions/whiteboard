# ModifierPoolTrait

_Published_: 2019-12-22

## Table of Contents

+ [Description](#description)
+ [Usage](#usage)
+ [Source](#source)

## Description

Several Magento UI components depend on data from data providers. Often times, we need to
conditionally modify data provided by a data provider, so data modifiers were introduced
to solve this problem. Traditionally, we would need to override a class in order to make
those modifications, but Magento provides an improved approach with data modifiers, which
accepts the respective metadata/data as an argument, and returns the modified version to
the data provider.

In the example below, we've created a trait called `ModifierPoolTrait`, which can be used
by a data provider class to access the respective data modifier pool.

## Usage

```php
<?php
/**
 * DataProvider.php
 */
declare(strict_types=1);

namespace Vendor\Package\Ui\DataProvider\Listing\Entity;

use Countable;
use Magento\Framework\View\Element\UiComponent\DataProvider\DataProviderInterface;
use Magento\Ui\{
    DataProvider\AbstractDataProvider,
    DataProvider\Modifier\ModifierInterface,
    DataProvider\Modifier\PoolInterface
};
use Vendor\Package\Component\Ui\DataModifier\ModifierPoolTrait;

class DataProvider extends AbstractDataProvider implements
    Countable,
    DataProviderInterface
{
    /**
     * @property PoolInterface $modifierPool
     * @method PoolInterface getModifierPool()
     * @method ModifierInterface[] getModifiers()
     */
    use ModifierPoolTrait;

    /** @var array $loadedData */
    protected $loadedData = [];

    /**
     * @param string $name
     * @param string $primaryFieldName
     * @param string $requestFieldName
     * @param array $meta
     * @param array $data
     * @param PoolInterface $modifierPool
     * @return void
     */
    public function __construct(
        $name,
        $primaryFieldName,
        $requestFieldName,
        array $meta = [],
        array $data = [],
        PoolInterface $modifierPool
    ) {
        parent::__construct(
            $name,
            $primaryFieldName,
            $requestFieldName,
            $meta,
            $data
        );
        $this->modifierPool = $modifierPool;
    }

    /**
     * @return array
     */
    public function getMeta(): array
    {
        /** @var array $meta */
        $meta = parent::getMeta();

        /** @var ModifierInterface[] $modifiers */
        $modifiers = $this->getModifiers();

        /** @var ModifierInterface $modifier */
        foreach ($modifiers as $modifier) {
            $meta = $modifier->modifyMeta($meta);
        }

        return $meta;
    }

    /**
     * @return array
     */
    public function getData(): array
    {
        if (!empty($this->loadedData)) {
            return $this->loadedData;
        }

        /** @var EntityInterface[] $items */
        $items = $this->getCollection()->getItems();

        /** @var EntityInterface $entity */
        foreach ($items as $entity) {
            $this->loadedData[$entity->getId()] = $entity->getData();
        }

        /** @var ModifierInterface[] $modifiers */
        $modifiers = $this->getModifiers();

        /** @var ModifierInterface $modifier */
        foreach ($modifiers as $modifier) {
            $this->loadedData = $modifier->modifyData($this->loadedData);
        }

        return $this->loadedData;
    }
}
```

## Source

```php
<?php
/**
 * ModifierPoolTrait.php
 */
declare(strict_types=1);

namespace Vendor\Package\Component\Ui\DataModifier;

use Magento\Ui\{
    DataProvider\Modifier\ModifierInterface,
    DataProvider\Modifier\PoolInterface
};

trait ModifierPoolTrait
{
    /** @property PoolInterface $modifierPool */
    private $modifierPool;

    /**
     * @return PoolInterface
     */
    public function getModifierPool(): PoolInterface
    {
        return $this->modifierPool;
    }

    /**
     * @return ModifierInterface[]
     */
    public function getModifiers(): array
    {
        return $this->getModifierPool()
            ->getModifiersInstances();
    }
}
```
