.. contents:: Table of Contents
    :depth: 2

EventManagerTrait
=================

* *Published*: 2019-12-21
* *Author*: Nickolas Burr

Description
-----------

Magento provides an event-driven subsystem that allows modules to dispatch and observe
arbitrary types of events. This is useful for a variety of reasons, such as:

* Performing actions upon occurrence of specific events
* Receiving important data sent by an event dispatcher
* Logging state during time of event dispatch

In the example below, we've created a trait called ``EventManagerTrait``. By mixing in the
``EventManagerTrait`` trait, a class can easily dispatch events with data by invoking the
``dispatchEvent`` method.

Usage
-----

.. code-block:: php

    <?php
    /**
     * Entity.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Model;

    use Magento\Framework\Event\ManagerInterface as EventManagerInterface;
    use Vendor\Package\Component\Event\EventManagerTrait;

    class Entity
    {
        /**
         * @property EventManagerInterface $eventManager
         * @method void dispatchEvent(string $event, array $data)
         */
        use EventManagerTrait;

        /**
         * @param EventManagerInterface $eventManager
         * @return void
         */
        public function __construct(
            EventManagerInterface $eventManager
        ) {
            $this->eventManager = $eventManager;
        }

        /**
         * @return void
         */
        public function save()
        {
            $this->dispatchEvent(
                'package_entity_save_before',
                [
                    'entity' => $entity,
                    'status' => $status,
                ]
            );

            ...

            $this->dispatchEvent(
                'package_entity_save_after',
                [
                    'entity' => $entity,
                    'status' => $status,
                ]
            );
        }
    }

Source
------

.. code-block:: php

    <?php
    /**
     * EventManagerTrait.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Component\Event;

    trait EventManagerTrait
    {
        /** @property Magento\Framework\Event\ManagerInterface $eventManager */
        private $eventManager;

        /**
         * @param string $event
         * @param array $data
         * @return void
         */
        private function dispatchEvent(string $event, array $data = []): void
        {
            $this->eventManager
                ->dispatch($event, $data);
        }
    }
