# DataObjectFactory

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)

## Related

- [ExceptionFactory](ExceptionFactory.md)

## Description

The `DataObject` class plays an integral role in Magento as the data container for
CRUD operations. Likewise, there are circumstances where you need the capabilities
of `DataObject`, but don't want tighter coupling introduced with inheritance. This
is where a `DataObjectFactory` comes in handy. It provides a means of injecting
data via `di.xml`, without introducing coupling from inheritance.

## Usage

```php
<?php
/**
 * Cities.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model;

use Magento\Framework\{
    DataObject,
    DataObjectFactory
};

class Cities
{
    /** @property DataObjectFactory $dataObjectFactory */
    protected $dataObjectFactory;

    /** @property DataObject $container */
    protected $container;

    /**
     * @param DataObjectFactory $dataObjectFactory
     * @param array $data
     * @return void
     */
    public function __construct(
        DataObjectFactory $dataObjectFactory,
        array $data = []
    ) {
        $this->dataObjectFactory = $dataObjectFactory;
        $this->container = $this->dataObjectFactory->create($data);
    }

    /**
     * @return DataObject|null
     */
    protected function getContainer(): ?DataObject
    {
        return $this->container;
    }

    /**
     * @return array
     */
    public function getCities(): array
    {
        return $this->getContainer()->getData('cities') ?? [];
    }
}
```

```xml
<?xml version="1.0"?>
<!--
/**
 * di.xml
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Vendor\Package\Model\Entity">
        <arguments>
            <argument name="data" xsi:type="array">
                <item name="cities" xsi:type="array">
                    <item name="new_york" xsi:type="string">New York</item>
                    <item name="london" xsi:type="string">London</item>
                    <item name="paris" xsi:type="string">Paris</item>
                    <item name="rome" xsi:type="string">Rome</item>
                </item>
            </argument>
        </arguments>
    </type>
</config>
```
