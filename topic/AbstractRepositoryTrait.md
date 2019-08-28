# AbstractRepositoryTrait

## Table of Contents

- [Related](#related)
- [Description](#description)
- [Usage](#usage)
- [Source](#source)

## Related

- [AbstractRepositoryInterface](AbstractRepositoryInterface.md)
- [AbstractRepository](AbstractRepository.md)

## Description

In constract to `AbstractRepository`, which is an `abstract` class that implements
`AbstractRepositoryInterface`, you could make use of PHP traits to achieve a similar
result. With regards to `AbstractRepository`, this approach provides looser coupling
by eliminating inheritance and utilizing a trait as the vehicle for satisfying the
service contract enforced by `AbstractRepositoryInterface`.

## Usage

```php
<?php
/**
 * EntityRepository.php
 */
declare(strict_types=1);

namespace Vendor\Package\Model\RepositoryModel;

use Vendor\Package\Api\EntityRepositoryInterface;

class EntityRepository implements EntityRepositoryInterface
{
    use AbstractRepositoryTrait;

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

    ...
}
```

## Source

```php
<?php
/**
 * AbstractRepositoryTrait.php
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

trait AbstractRepositoryTrait
{
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
