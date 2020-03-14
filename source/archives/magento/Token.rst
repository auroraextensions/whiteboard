Token
=====

* Published: 2019-08-21
* Author: Nickolas Burr

.. contents:: Table of Contents
    :local:

Description
===========

Magento provides the ``Magento\Framework\Math\Random`` [#ref1]_ class for
generating random data. This class is particularly useful when you need things
like tokens, nonces, and salts, and is used in several areas of the framework.
However, we'd prefer to have an entirely static class that provides the same
functionality, which we can do with PHP builtins.

In the example below, the ``Token`` class provides two ``static`` methods:

+ ``generate``
+ ``isHex``

The ``generate`` method utilizes ``random_bytes`` [#ref2]_ for random sequence
generation, and the ``isHex`` method verifies the given sequence contains only
hexidecimal characters.

Usage
=====

.. code-block:: php

    <?php
    ...
    /** @var string $token */
    $token = Token::generate();
    ...

Source
======

.. code-block:: php

    <?php
    /**
     * Token.php
     */
    declare(strict_types=1);

    namespace Vendor\Package\Model\Security;

    class Token
    {
        /** @constant string REGEX */
        public const REGEX = '/[^a-f0-9]/';

        /**
         * @param int $length
         * @return string
         */
        public static function generate(int $length = 32): string
        {
            return bin2hex(random_bytes($length));
        }

        /**
         * @param string $token
         * @return bool
         */
        public static function isHex(string $token): bool
        {
          return !preg_match(self::REGEX, $token);
        }
    }

Notes
=====

.. |generator| replace:: ``Magento\Framework\Math\Random``
.. _generator: https://github.com/magento/magento2/blob/2.3/lib/internal/Magento/Framework/Math/Random.php

.. |random_bytes| replace:: ``random_bytes``
.. _random_bytes: https://www.php.net/manual/en/function.random-bytes.php

.. [#ref1] |generator|_
.. [#ref2] |random_bytes|_
