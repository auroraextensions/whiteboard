SelectQueryFetchStrategyInterface
=================================

* Published: 2023-11-16
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Related
-------

* :doc:`SelectQueryFetchStrategy`

Description
-----------

An important aspect of working with collections is optimizing for performance.
This is especially true in the context of Magento, where it's quite common for
the underlying SQL to be complex and expensive. That's why Magento provides the
interface ``Magento\Framework\Data\Collection\Db\FetchStrategyInterface`` along
with two implementation classes:

1. ``Magento\Framework\Data\Collection\Db\FetchStrategy\Cache``
2. ``Magento\Framework\Data\Collection\Db\FetchStrategy\Query``

As their class names suggest, ``Cache`` caches the results of the ``SELECT``
query, whereas ``Query`` does not and will reconnect to the database each time.
In fact, ``Cache`` utilizes ``Query`` to execute the query when a cache entry
does not exist. This cache-driven approach can have substantial performance
benefits for complex queries and queries with large data sets, but it can also
mask unperformant queries that require optimization, and should be used with care.

``SelectQueryFetchStrategyInterface`` extends ``FetchStrategyInterface`` to provide
additional methods for other SQL fetch-related operations. Likewise, it can be used
for purposes beyond collections, where it's equally useful. The rationale behind
extending ``FetchStrategyInterface`` is to make it easier to use as a drop-in
replacement where instances of ``FetchStrategyInterface`` are being utilized.

Usage
-----

.. code-block:: xml

    <?xml version="1.0"?>
    <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
        <type name="Magento\Eav\Model\ResourceModel\Attribute\Collection">
            <arguments>
                <argument name="fetchStrategy" xsi:type="object">Vendor\Package\Api\SelectQueryFetchStrategyInterface</argument>
            </arguments>
        </type>
    </config>

Source
------

.. code-block:: php

    <?php

    declare(strict_types=1);

    namespace Vendor\Package\Api;

    use Magento\Framework\Data\Collection\Db\FetchStrategyInterface;
    use Magento\Framework\DB\Select;

    interface SelectQueryFetchStrategyInterface extends FetchStrategyInterface
    {
        /**
         * @param \Magento\Framework\DB\Select $select
         * @param mixed[] $bind
         * @return mixed[]
         */
        public function fetchAssoc(Select $select, array $bind = []): array;

        /**
         * @param \Magento\Framework\DB\Select $select
         * @param mixed[] $bind
         * @return mixed[]
         */
        public function fetchCol(Select $select, array $bind = []): array;

        /**
         * @param \Magento\Framework\DB\Select $select
         * @param mixed[] $bind
         * @return string|null
         */
        public function fetchOne(Select $select, array $bind = []): ?string;

        /**
         * @param \Magento\Framework\DB\Select $select
         * @param mixed[] $bind
         * @return mixed[]
         */
        public function fetchPairs(Select $select, array $bind = []): array;

        /**
         * @param \Magento\Framework\DB\Select $select
         * @param mixed[] $bind
         * @return mixed[]
         */
        public function fetchRow(Select $select, array $bind = []): array;
    }
