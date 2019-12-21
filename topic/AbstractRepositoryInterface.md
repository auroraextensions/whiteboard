# AbstractRepositoryInterface

---

_Published_: 2019-07-02

## Table of Contents

+ [Related](#related)
+ [Description](#description)
+ [Usage](#usage)
+ [Source](#source)
+ [Notes](#notes)

## Related

+ [AbstractRepository](AbstractRepository.md)
+ [AbstractRepositoryTrait](AbstractRepositoryTrait.md)

## Description

All repository interfaces share common method signatures. For example, any repository interface
will have a `getList` method that accepts an instance of `SearchCriteriaInterface`<sup>1</sup>
and returns an instance of `SearchResultsInterface`<sup>2</sup>, per the Magento repository design
pattern. As such, it makes sense to reduce duplication by creating an abstract repository interface
that isn't implemented directly, but is extended through entity-specific repository interfaces.

## Usage

```php
<?php
/**
 * EntityRepositoryInterface.php
 */
declare(strict_types=1);

namespace Vendor\Package\Api;

interface EntityRepositoryInterface extends AbstractRepositoryInterface
{
}
```

## Source

```php
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
```

## Notes

1. [`SearchCriteriaInterface`](https://github.com/magento/magento2/blob/2.3-develop/lib/internal/Magento/Framework/Api/SearchCriteriaInterface.php) (GitHub)
2. [`SearchResultsInterface`](https://github.com/magento/magento2/blob/2.3-develop/lib/internal/Magento/Framework/Api/SearchResultsInterface.php) (GitHub)
