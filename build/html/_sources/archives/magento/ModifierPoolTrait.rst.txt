.. contents:: :local:

ModifierPoolTrait
=================

*Published*: 2019-12-22

Description
===========

Several Magento UI components depend on data from data providers. Often times, we need to
conditionally modify data provided by a data provider, so data modifiers were introduced
to solve this problem. Traditionally, we would need to override a class in order to make
those modifications, but Magento provides an improved approach with data modifiers, which
accepts the respective metadata/data as an argument, and returns the modified version to
the data provider.

In the example below, we've created a trait called ``ModifierPoolTrait``, which can be used
by a data provider class to access the respective data modifier pool.

Usage
=====

.. code-block:: php

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

.. code-block:: php

    <?php
    /**
     * DataModifier.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Ui\DataModifier\Entity;

    use Magento\Ui\DataProvider\Modifier\ModifierInterface;

    class DataModifier implements ModifierInterface
    {
        /**
         * @param array $meta
         * @return array
         */
        public function modifyMeta(array $meta)
        {
            /* Perform whatever modifications you need to make to the metadata. */

            return $meta;
        }

        /**
         * @param array $data
         * @return array
         */
        public function modifyData(array $data)
        {
            /* Perform whatever modifications you need to make to the data. */

            return $data;
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
        <virtualType name="Vendor\Package\Ui\DataModifier\Entity\Pool" type="Magento\Ui\DataProvider\Modifier\Pool">
             <arguments>
                 <argument name="modifiers" xsi:type="array">
                     <item name="vendor_package_entity_data_modifier" xsi:type="array">
                         <item name="class" xsi:type="string">Vendor\Package\Ui\DataModifier\Entity\DataModifier</item>
                         <item name="sortOrder" xsi:type="number">10</item>
                     </item>
                 </argument>
             </arguments>
        </virtualType>

        <type name="Vendor\Package\Ui\DataProvider\Listing\Entity\DataProvider">
            <arguments>
                <argument name="modifierPool" xsi:type="object">Vendor\Package\Ui\DataModifier\Entity\Pool</argument>
            </arguments>
        </type>
    </config>
    ```

Source
======

.. code-block:: php

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
