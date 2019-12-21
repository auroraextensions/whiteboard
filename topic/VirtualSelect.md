# VirtualSelect

_Published_: 2019-07-25

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)
- [Source](#source)

## Related

- [DataObjectFactory](DataObjectFactory.md)

## Description

The use of dependency injection in Magento makes it especially easy to
work with data from `di.xml`. However, it is still so commonplace to
see hardcoded data in backend `source_model` classes, which amounts to
a bunch of duplicated classes containing disparate data. Unifying the
data in `di.xml` makes it easier to maintain and update throughout the
lifetime of the module.

An elegant approach to creating select `source_model` classes with populated
data is to create a generic class that implements `ArrayInterface`, and use
the power of the `<virtualType>` to set the options dynamically.

## Usage

```xml
<?xml version="1.0"?>
<!--
/**
 * system.xml
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Config:etc/system_file.xsd">
    <system>
        <tab id="vendor" translate="label" sortOrder="100">
            <label>Vendor, Inc.</label>
        </tab>
        <section id="package" translate="label" sortOrder="100"
                 showInDefault="1" showInWebsite="1" showInStore="1">
            <class>separator-top</class>
            <label>Package</label>
            <tab>vendor</tab>
            <group id="general" translate="label" type="text" sortOrder="10"
                   showInDefault="1" showInWebsite="1" showInStore="1">
                <label>General Settings</label>
                <field id="default_status" translate="label" type="select" sortOrder="10"
                       showInDefault="1" showInWebsite="1" showInStore="1">
                    <label>Default Status</label>
                    <!-- The <virtualType> class -->
                    <source_model>Vendor\Package\Model\Source\Select\Status</source_model>
                </field>
            </group>
        </section>
    </system>
</config>
```

## Source

```php
<?php
/**
 * Generic.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model\Source\Select;

use Magento\Framework\Option\ArrayInterface;

class Generic implements ArrayInterface
{
    /** @property array $options */
    protected $options = [];

    /**
     * @param array $data
     * @return void
     */
    public function __construct(array $data = [])
    {
        /* Inverts KV for array_walk. */
        $data = array_flip($data);

        array_walk(
            $data,
            [
                $this,
                'setOption'
            ]
        );
    }

    /**
     * @param int|string|null $value
     * @param int|string $key
     * @return void
     */
    protected function setOption($value, $key): void
    {
        $this->options[] = [
            'label' => __($key),
            'value' => $value,
        ];
    }

    /**
     * @return array
     */
    public function toOptionArray()
    {
        return $this->options;
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
    <virtualType name="Vendor\Package\Model\Source\Select\Status"
                 type="Vendor\Package\Model\Source\Select\Generic">
        <arguments>
            <argument name="data" xsi:type="array">
                <item name="pending" xsi:type="string">Pending</item>
                <item name="closed" xsi:type="string">Closed</item>
                <item name="open" xsi:type="string">Open</item>
                <item name="on_hold" xsi:type="string">On Hold</item>
            </argument>
        </arguments>
    </virtualType>
</config>
```
