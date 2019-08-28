# AbstractRepository

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)
- [Source](#source)

## Related

- [AbstractRepositoryInterface](AbstractRepositoryInterface.md)
- [AbstractRepositoryTrait](AbstractRepositoryTrait.md)

## Description

Just as `AbstractRepositoryInterface` can be extended by entity-specific repository
interfaces, `AbstractRepository` can be extended by entity-specific repository models.
While this introduces tighter coupling through inheritance, the tradeoff is to reduce
duplication common to repository models, and the impact can be minimized by keeping
`AbstractRepository` small and lightweight, providing only the methods and properties
needed for _all_ repository models.

## Usage

```php
<?php
/**
 * EntityRepository.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model\RepositoryModel;

use Vendor\Package\Api\EntityRepositoryInterface;

class EntityRepository extends AbstractRepository implements EntityRepositoryInterface
{
}
```

## Source

```php
<?php
/**
 * AbstractRepository.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model\RepositoryModel;

use Magento\Framework\{
    Api\SearchCriteriaInterface,
    Api\SearchResultsInterface,
    Api\Search\FilterGroup,
    Api\SortOrder,
    Exception\NoSuchEntityException
};
use Vendor\Package\Api\AbstractRepositoryInterface;

abstract class AbstractRepository implements AbstractRepositoryInterface
{
    /** @property mixed $collectionFactory */
    protected $collectionFactory;

    /** @property mixed $searchResultsFactory */
    protected $searchResultsFactory;

    /**
     * @param mixed $collectionFactory
     * @param mixed $searchResultsFactory
     * @return void
     */
    public function __construct(
        $collectionFactory,
        $searchResultsFactory
    ) {
        $this->collectionFactory = $collectionFactory;
        $this->searchResultsFactory = $searchResultsFactory;
    }

    /**
     * @param FilterGroup $filterGroup
     * @param mixed $collection
     * @return void
     */
    public function addFilterGroupToCollection(
        FilterGroup $filterGroup,
        $collection
    ): void
    {
        /** @var array $fields */
        $fields = [];

        /** @var array $params */
        $params = [];

        foreach ($filterGroup->getFilters() as $filter) {
            /** @var string $param */
            $param = $filter->getConditionType() ?: 'eq';

            /** @var string $field */
            $field = $filter->getField();

            /** @var mixed $value */
            $value = $filter->getValue();

            $fields[] = $field;
            $params[] = [
                $param => $value,
            ];
        }

        $collection->addFieldToFilter($fields, $params);
    }

    /**
     * @param string $direction
     * @return string
     */
    public function getDirection(
        string $direction = SortOrder::SORT_DESC
    ): string
    {
        return $direction === SortOrder::SORT_ASC
            ? SortOrder::SORT_ASC
            : SortOrder::SORT_DESC;
    }

    /**
     * @param SearchCriteriaInterface $criteria
     * @return SearchResultsInterface
     */
    public function getList(SearchCriteriaInterface $criteria): SearchResultsInterface
    {
        /** @var AbstractCollectionInterface $collection */
        $collection = $this->collectionFactory->create();

        foreach ($criteria->getFilterGroups() as $group) {
            $this->addFilterGroupToCollection($group, $collection);
        }

        foreach ((array) $criteria->getSortOrders() as $sortOrder) {
            /** @var string $field */
            $field = $sortOrder->getField();

            $collection->addOrder(
                $field,
                $this->getDirection($sortOrder->getDirection())
            );
        }

        $collection->setCurPage($criteria->getCurrentPage());
        $collection->setPageSize($criteria->getPageSize());
        $collection->load();

        /** @var SearchResultsInterface $results */
        $results = $this->searchResultsFactory->create();
        $results->setSearchCriteria($criteria);

        /** @var array $items */
        $items = [];

        foreach ($collection as $item) {
            $items[] = $item;
        }

        $results->setItems($items);
        $results->setTotalCount($collection->getSize());

        return $results;
    }
}
```
