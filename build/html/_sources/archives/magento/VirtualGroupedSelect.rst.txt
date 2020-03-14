VirtualGroupedSelect
====================

* Published: 2019-07-25
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Related
=======

* `DataObjectFactory <DataObjectFactory>`_
* `VirtualSelect <VirtualSelect>`_

Description
===========

The `VirtualSelect <VirtualSelect>`_ example illustrates how to create a ``select``,
``multiselect`` source model with data populated from ``di.xml``. That works well for
flat arrays, but doesn't work for multidimensional arrays.

In order to populate a ``select`` or ``multiselect`` in the same fashion, but with a
multidimensional array, we'll need to adjust the ``setOption`` method to recurse
through the given subarrays and create the desired ``optgroup`` set.

Usage
=====

.. code-block:: xml

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
                        <label>Product Types</label>
                        <!-- The <virtualType> class -->
                        <source_model>Vendor\Package\Model\Source\Select\Grouped\ProductTypes</source_model>
                    </field>
                </group>
            </section>
        </system>
    </config>

Source
======

.. code-block:: php

    <?php
    /**
     * Generic.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Model\Source\Select\Grouped;

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
            array_walk(
                $data,
                [
                    $this,
                    'setOption'
                ]
            );
        }

        /**
         * @param array $options
         * @return array
         */
        protected function getOptGroup(array $options): array
        {
            /** @var array $optgroup */
            $optgroup = [];

            foreach ($options as $key => $value) {
                if (is_array($value)) {
                    $optgroup[] = $this->getOptGroup($value);
                } else {
                  $optgroup[] = [
                      'label' => $value,
                      'value' => $key,
                  ];
                }
            }

            return $optgroup;
        }

        /**
         * @param int|string|null $value
         * @param int|string $key
         * @return void
         */
        protected function setOption($value, $key): void
        {
            if (is_array($value)) {
                $value = $this->getOptGroup($value);
            }

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

.. code-block:: xml

    <?xml version="1.0"?>
    <!--
    /**
     * di.xml
     */
    -->
    <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
        <virtualType name="Vendor\Package\Model\Source\Select\Grouped\ProductTypes"
                     type="Vendor\Package\Model\Source\Select\Grouped\Generic">
            <arguments>
                <argument name="data" xsi:type="array">
                    <item name="shirts" xsi:type="array">
                        <item name="tshirt" xsi:type="string">T-shirt</item>
                        <item name="long_sleeve" xsi:type="string">Long Sleeve Shirt</item>
                    </item>
                    <item name="pants" xsi:type="array">
                        <item name="slacks" xsi:type="string">Slacks</item>
                        <item name="track_pants" xsi:type="string">Track Pants</item>
                    </item>
                    <item name="shoes" xsi:type="array">
                        <item name="leather" xsi:type="array">
                            <item name="loafers" xsi:type="string">Loafers</item>
                        </item>
                    </item>
                </argument>
            </arguments>
        </virtualType>
    </config>
