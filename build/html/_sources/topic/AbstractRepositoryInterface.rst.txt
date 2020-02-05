.. contents:: :local:

AbstractRepositoryInterface
===========================

*Published*: 2019-07-02

Related
=======

* `AbstractRepository <AbstractRepository>`_
* `AbstractRepositoryTrait <AbstractRepositoryTrait>`_

Description
===========

All repository interfaces share common method signatures. For example, any repository interface
will have a ``getList`` method that accepts an instance of ``SearchCriteriaInterface`` [#ref1]_
and returns an instance of ``SearchResultsInterface`` [#ref2]_, per the Magento repository design
pattern. As such, it makes sense to reduce duplication by creating an abstract repository interface
that isn't implemented directly, but is extended through entity-specific repository interfaces.

Usage
=====

.. code-block:: php

    <?php
    /**
     * EntityRepositoryInterface.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Api;

    interface EntityRepositoryInterface extends AbstractRepositoryInterface
    {
    }

Source
======

.. code-block:: php

    <?php
    /**
     * AbstractRepositoryInterface.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Api;

    use Magento\Framework\{
        Api\SearchCriteriaInterface,
        Api\SearchResultsInterface,
        Api\Search\FilterGroup
    };

    interface AbstractRepositoryInterface
    {
        /**
         * @param FilterGroup $group
         * @param mixed $collection
         * @return void
         */
        public function addFilterGroupToCollection(
            FilterGroup $group,
            $collection
        ): void;

        /**
         * @param string $direction
         * @return string
         */
        public function getDirection(string $direction): string;

        /**
         * @param SearchCriteriaInterface $criteria
         * @return SearchResultsInterface
         */
        public function getList(SearchCriteriaInterface $criteria): SearchResultsInterface;
    }

Notes
=====

.. [#ref1] `Magento\\Framework\\Api\\SearchCriteriaInterface <https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/Api/SearchCriteriaInterface.php>`_
.. [#ref2] `Magento\\Framework\\Api\\SearchResultsInterface <https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/Api/SearchResultsInterface.php>`_
