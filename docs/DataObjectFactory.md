# DataObjectFactory

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)

## Related

- [ExceptionFactory](ExceptionFactory.md)

## Description

The `DataObject` class plays an integral role in Magento as the data container for
CRUD operations. Likewise, there are circumstances where you need to interact with
data injected via `di.xml`, but don't want to introduce tighter coupling caused by
extending the `DataObject` class. This is where a `DataObjectFactory` can come in
handy. It provides a compositional approach to interacting with injected data.

In the usage example below, we have a `$container` property that will store our
injected data in a `DataObject`. By using this approach, you not only have looser
coupling, but you can control access to the data with the appropriate visibility
settings for `$container` and `getContainer`.

## Usage

```php
<?php
/**
 * Geography.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model;

use Magento\Framework\{
    DataObject,
    DataObject\Factory as DataObjectFactory
};

class Geography
{
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
        $this->container = $dataObjectFactory->create($data);
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
    public function getCountries(): array
    {
        return $this->getContainer()->getData('countries') ?? [];
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
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Vendor\Package\Model\Geography">
        <arguments>
            <argument name="data" xsi:type="array">
                <item name="countries" xsi:type="array">
                    <item name="us" xsi:type="string">US</item>
                    <item name="england" xsi:type="string">England</item>
                    <item name="france" xsi:type="string">France</item>
                    <item name="italy" xsi:type="string">Italy</item>
                </item>
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
