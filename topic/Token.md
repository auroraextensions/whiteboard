# Token

## Table of Contents

- [Description](#description)
- [Usage](#usage)
- [Source](#source)

## Description

Magento provides the `Magento\Framework\Math\Random` class for generating
random data. This is particularly useful when you need things like access
tokens, nonces, and salts.

In the example below, the `Token` class provides two `static` methods:

+ `generate`
+ `isHex`

The `generate` method utilizes `random_bytes` for sequence generation, and
the `isHex` method verifies the given sequence contains only hexidecimal
characters.

## Usage

```php
...
/** @var string $token */
$token = Token::generate();
...
```

## Source

```php
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
    public function generate(int $length = 32): string
    {
        return bin2hex(random_bytes($length));
    }

    /**
     * @param string $token
     * @return bool
     */
    public function isHex(string $token): bool
    {
      return !preg_match(self::REGEX, $token);
    }
}
```
