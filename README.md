# Adding php 8.1 enum support to Laravel migrations

[![Latest Version on Packagist](https://img.shields.io/packagist/v/zbundy/laravel-enums.svg?style=flat-square)](https://packagist.org/packages/zbundy/laravel-enums)
![Test](https://github.com/zbundy/laravel-enums/workflows/Test/badge.svg)
![Check & fix styling](https://github.com/zbundy/laravel-enums/workflows/Check%20&%20fix%20styling/badge.svg)
[![Total Downloads](https://img.shields.io/packagist/dt/zbundy/laravel-enums.svg?style=flat-square)](https://packagist.org/packages/zbundy/laravel-enums)

This package adds enum support to migrations that has arrived with the release of [PHP 8.1](https://www.php.net/releases/8.1/en.php#enumerations).

## Installation

You can install the package via composer:

```bash
composer require zbundy/laravel-enums
```

## Usage

Let's say that you have the following `UserStatus` enum:

```php
<?php

namespace App\Enums;

enum UserStatus: string
{
    case ACTIVE = 'active';
    case SUSPENDED = 'suspended';
    case BANNED = 'banned';
}
```

Then in your user migration you can create a `status` column and pass that fully qualified enum:

```php
<?php

use App\Enums\UserStatus;
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class SetupDatabase extends Migration
{
    public function up()
    {
        Schema::create("users", function (Blueprint $table) {
            $table->id();

            $table->enum("status", UserStatus::class);

            $table->string("username");
            $table->string("password");

            $table->rememberToken();
            $table->timestamps();
        });
    }
}
```

You may also set a default value by creating a `DEFAULT` constant in your enum that is assigned one enum case:

```php
<?php

namespace App\Enums;

enum UserStatus: int
{
    case ACTIVE = 1;
    case SUSPENDED = 2;
    case BANNED = 3;

    public const DEFAULT = self::ACTIVE;
}
```

This is equivalent to doing the following:

```php
<?php

use Zbundy\LaravelEnums;
use App\Enums\UserStatus;
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class SetupDatabase extends Migration
{
    public function up()
    {
        Schema::create("users", function (Blueprint $table) {
            $table->id();

            $table
                ->enum("status", UserStatus::class)
                ->default(UserStatus::ACTIVE);

            $table->string("username");
            $table->string("password");

            $table->rememberToken();
            $table->timestamps();
        });
    }
}
```

## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information about recent changes.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details regarding contributions.

## Security

If you discover any security related issues, please use the [issue tracker](../../issues).

## Credits

-   [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
