![](https://banners.beyondco.de/Laravel%20API%20Response%20Helpers.png?theme=light&packageManager=composer+require&packageName=f9webltd%2Flaravel-api-response-helpers&pattern=brickWall&style=style_1&description=Generate+consistent+API+responses+for+your+Laravel+application&md=1&showWatermark=0&fontSize=100px&images=code)

[![run-tests](https://img.shields.io/github/workflow/status/f9webltd/laravel-api-response-helpers/run-tests?style=flat-square)](https://github.com/f9webltd/laravel-api-response-helpers/actions/workflows/run-tests.yml)
[![Packagist Version](https://img.shields.io/packagist/v/f9webltd/laravel-api-response-helpers?style=flat-square)](https://packagist.org/packages/f9webltd/laravel-api-response-helpers)
[![Packagist PHP Version](
https://img.shields.io/packagist/php-v/f9webltd/laravel-api-response-helpers?style=flat-square)](https://packagist.org/packages/f9webltd/laravel-api-response-helpers)
[![Packagist License](https://img.shields.io/packagist/l/f9webltd/laravel-api-response-helpers?style=flat-square)](https://packagist.org/packages/f9webltd/laravel-api-response-helpers)


# Laravel API Response Helpers

A simple package allowing for consistent API responses throughout your Laravel application.

## Requirements

- PHP `^7.4 | ^8.0`
- Laravel 6, 7 and 8

## Installation / Usage

`composer require f9webltd/laravel-api-response-helpers`


Simply reference the required trait within your controller:

```php
<?php

namespace App\Http\Api\Controllers;

use F9Web\ApiResponseHelpers;
use Illuminate\Http\JsonResponse;

class OrdersController
{
    use ApiResponseHelpers;

    public function index(): JsonResponse
    {
        return $this->respondWithSuccess();
    }
}
```

Optionally, the trait could be imported within a base controller.

## Available methods

#### `respondNotFound(string|Exception $message, ?string $key = 'error')`

Returns a `404` HTTP status code, an exception object can optionally be passed.

#### `respondWithSuccess(array|Arrayable|JsonSerializable|null $contents = [])`

Returns a `200` HTTP status code

#### `respondOk(string $message)`

Returns a `200` HTTP status code

#### `respondUnAuthenticated(?string $message = null)`

Returns a `401` HTTP status code

#### `respondForbidden(?string $message = null)`

Returns a `403` HTTP status code

#### `respondError(?string $message = null)`

Returns a `400` HTTP status code

#### `respondCreated(array|Arrayable|JsonSerializable|null $data = [])`

Returns a `201` HTTP status code, with response optional data

#### `respondNoContent(array|Arrayable|JsonSerializable|null $data = [])`

Returns a `204` HTTP status code, with optional response data. Strictly speaking, the response body should be empty. However, functionality to optionally return data was added to handle legacy projects. Within your own projects, you can simply call the method, omitting parameters, to generate a correct `204` response i.e. `return $this->respondNoContent()` 

## Additional Data Types

In addition to a plain PHP `array`, the following data types can be passed to relevant methods:

- Objects implementing the Laravel `Illuminate\Contracts\Support\Arrayable` contract
- Objects implementing the native PHP `JsonSerializable` contract

This allows a variety of object types to be passed and converted automatically.

Some example objects that can be passed (there are a lot more):

- `Illuminate\Support\Collection` (i.e. [Collections](https://laravel.com/docs/8.x/collections))
- `Illuminate\Database\Eloquent`  (i.e. [Eloquent collections](https://laravel.com/docs/8.x/eloquent-collections))
- `Illuminate\Http\Resources\Json\JsonResource` (i.e. [Laravel API resources](https://laravel.com/docs/8.x/eloquent-resources))
- `Illuminate\Foundation\Http\FormRequest`

## Motivation

Ensure consistent JSON API responses throughout an application. The motivation was primarily based on a very old inherited Laravel project. The project contained a plethora of methods/structures used to return an error:

- `response()->json(['error' => $error], 400)`
- `response()->json(['data' => ['error' => $error], 400)`
- `response()->json(['message' => $error], Response::HTTP_BAD_REQUEST)`
- `response()->json([$error], 400)`
- etc.

I wanted to add a simple trait that kept this consistent, in this case:

`$this->respondError('Ouch')`

This package is intended to be used **alongside** Laravel's  [API resources](https://laravel.com/docs/8.x/eloquent-resources) and in no way replaces them:

```php
$this->respondCreated(PostResource::make($post));
```

## Contribution

Any ideas are welcome. Feel free to submit any issues or pull requests.

## Testing

`composer test`

## Security

If you discover any security related issues, please email rob@f9web.co.uk instead of using the issue tracker.

## Credits

- [Rob Allport](https://github.com/ultrono) for [F9 Web Ltd.](https://www.f9web.co.uk)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.

