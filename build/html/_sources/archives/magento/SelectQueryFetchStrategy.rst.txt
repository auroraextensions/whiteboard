SelectQueryFetchStrategy
========================

* Published: 2023-11-16
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Related
-------

* `SelectQueryFetchStrategyInterface <SelectQueryFetchStrategyInterface>`_

Description
-----------

This is the implementation class for ``SelectQueryFetchStrategyInterface``.

Usage
-----

.. code-block:: php

    <?php

    declare(strict_types=1);

    namespace Vendor\Package\Model\ResourceModel\SomeEntity;

    use Magento\Framework\Data\Collection\EntityFactoryInterface;
    use Magento\Framework\Event\ManagerInterface;
    use Magento\Framework\DB\Adapter\AdapterInterface;
    use Magento\Framework\Model\ResourceModel\Db\AbstractDb;
    use Magento\Framework\Model\ResourceModel\Db\Collection\AbstractCollection;
    use Psr\Log\LoggerInterface;
    use Throwable;
    use UnexpectedValueException;
    use Vendor\Package\Api\SelectQueryFetchStrategyInterface;

    class Collection extends AbstractCollection
    {
        private const JSON_MAX_DEPTH = 4;

        /**
         * @param EntityFactoryInterface $entityFactory
         * @param LoggerInterface $logger
         * @param SelectQueryFetchStrategyInterface $fetchStrategy
         * @param ManagerInterface $eventManager
         * @param AdapterInterface|null $connection
         * @param AbstractDb|null $resource
         * @param mixed[] $data
         * @return void
         */
        public function __construct(
            EntityFactoryInterface $entityFactory,
            LoggerInterface $logger,
            SelectQueryFetchStrategyInterface $fetchStrategy,
            ManagerInterface $eventManager,
            ?AdapterInterface $connection = null,
            ?AbstractDb $resource = null,
            private array $data = []
        ) {
            parent::__construct(
                $entityFactory,
                $logger,
                $fetchStrategy,
                $eventManager,
                $connection,
                $resource
            );
        }

        /**
         * @return string|null
         */
        public function getSpecificColumnValue(): ?string
        {
            return $this->_fetchStrategy->fetchOne($this->getSelect());
        }
    }

Source
------

.. code-block:: php

    <?php

    declare(strict_types=1);

    namespace Vendor\Package\Model\Query;

    use Magento\Framework\App\CacheInterface;
    use Magento\Framework\DB\Select;
    use Magento\Framework\Serialize\SerializerInterface;
    use Psr\Log\LoggerInterface;
    use Vendor\Package\Api\SelectQueryFetchStrategyInterface;

    use function hash;
    use function implode;
    use function json_encode;
    use function json_last_error;
    use function json_last_error_msg;
    use function ksort;
    use function sprintf;

    use const JSON_ERROR_NONE;
    use const JSON_FORCE_OBJECT;
    use const SORT_STRING;

    class SelectQueryFetchStrategy implements SelectQueryFetchStrategyInterface
    {
        /**
         * @param LoggerInterface $logger
         * @param SerializerInterface $serializer
         * @param CacheInterface|null $cache
         * @param string[] $cacheTags
         * @param int $cacheTtl
         * @param string $algo
         * @return void
         */
        public function __construct(
            private readonly LoggerInterface $logger,
            private readonly SerializerInterface $serializer,
            private ?CacheInterface $cache = null,
            private readonly array $cacheTags = [],
            private readonly int $cacheTtl = 3600,
            private readonly string $algo = 'crc32b'
        ) {}

        /**
         * {@inheritdoc}
         */
        public function fetchAll(Select $select, array $bind = []): array
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return (array) $this->serializer->unserialize($val);
            }

            try {
                /** @var mixed[]|false $data */
                $data = $select->getConnection()->fetchAll($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (array) $data;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * {@inheritdoc}
         */
        public function fetchAssoc(Select $select, array $bind = []): array
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return (array) $this->serializer->unserialize($val);
            }

            try {
                /** @var mixed[]|false $data */
                $data = $select->getConnection()->fetchAssoc($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (array) $data;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * {@inheritdoc}
         */
        public function fetchCol(Select $select, array $bind = []): array
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return (array) $this->serializer->unserialize($val);
            }

            try {
                /** @var mixed[]|false $data */
                $data = $select->getConnection()->fetchCol($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (array) $data;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * {@inheritdoc}
         */
        public function fetchOne(Select $select, array $bind = []): ?string
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return $val;
            }

            try {
                /** @var string|false $data */
                $data = $select->getConnection()->fetchOne($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (string) $data ?: null;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * {@inheritdoc}
         */
        public function fetchPairs(Select $select, array $bind = []): array
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return (array) $this->serializer->unserialize($val);
            }

            try {
                /** @var mixed[]|false $data */
                $data = $select->getConnection()->fetchPairs($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (array) $data;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * {@inheritdoc}
         */
        public function fetchRow(Select $select, array $bind = []): array
        {
            /** @var string|null $val */
            /** @var string|null $key */
            [$val, $key] = $this->getCacheValue(
                $select,
                $bind,
                __METHOD__
            );

            if (!empty($val)) {
                return (array) $this->serializer->unserialize($val);
            }

            try {
                /** @var mixed[]|false $data */
                $data = $select->getConnection()->fetchRow($select, $bind);

                if ($data !== false) {
                    $this->cache?->save(
                        $this->serializer->serialize($data),
                        $key,
                        $this->cacheTags,
                        $this->cacheTtl
                    );
                }

                return (array) $data;
            } catch (Throwable $e) {
                $this->logger->error($e->getMessage());
                return [];
            }
        }

        /**
         * @param Select $select
         * @param mixed[] $bind
         * @param string $caller
         * @return mixed[]|null
         */
        private function getCacheValue(
            Select $select,
            array $bind,
            string $caller
        ): ?array {
            if ($this->cache === null) {
                return null;
            }

            if (!empty($bind)) {
                ksort($bind, SORT_STRING);
            }

            /** @var string $params */
            $params = json_encode(
                $bind,
                JSON_FORCE_OBJECT,
                self::JSON_MAX_DEPTH
            );

            if (json_last_error() !== JSON_ERROR_NONE) {
                throw new UnexpectedValueException(
                    sprintf(
                        'Unable to serialize bind parameters: "%s"',
                        json_last_error_msg()
                    )
                );
            }

            /** @var string $cacheKey */
            $cacheKey = sprintf(
                'sqfs-%s-%s',
                $caller,
                hash(
                    $this->algo,
                    implode('', [$select, $params])
                )
            );
            return [
                $this->cache?->load($cacheKey) ?: null,
                $cacheKey,
            ];
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
        <preference for="Vendor\Package\Api\SelectQueryFetchStrategyInterface" type="Vendor\Package\Model\Query\SelectQueryFetchStrategy"/>
    </config>
