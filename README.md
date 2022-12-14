# Data Transfer Object

Fast and light Data Transfer Object (DTO) with nested objects and attribute change control, 
without checking scalar types, use PHP type casting to check them.

[![License](https://img.shields.io/github/license/laxity7/dto.svg)](https://github.com/laxity7/dto/blob/master/LICENSE)
[![Latest Stable Version](https://img.shields.io/packagist/v/laxity7/dto.svg)](https://packagist.org/packages/laxity7/dto)
[![Total Downloads](https://img.shields.io/packagist/dt/laxity7/dto.svg)](https://packagist.org/packages/laxity7/dto)

You can control whether the DTO can be mutable, but by default, it is immutable.
To be able to change, use public properties or public setters.

This package supports php: 7.4+, 8.*

## Install

Install via composer

```shell
composer require laxity7/dto
```

Or you may add dependency manually in composer.json: `"laxity7/dto": "*"`

## How to use

```php
class FooDto extends BaseDTO
 {
     public int $id;
     public string $name;
     public array $data; // just array
     /* @var BarDto[] */
     public array $bars; // array of objects BarDto
     public BarDto $bar;
     /* @var BarDto */
     public $baz;
     protected string $time;

     // setter can be protected
     // setter has higher priority than field
     public function setName(string $name): void
     {
         $this->name = 'Foo' . $name;
     }

     public function getTime(): string
     {
         return $this->time;
     }
 }

 class BarDto extends BaseDTO
 {
     public int $id;
 }

 $fooDto = new FooDto([
     'id'   => 1,
     'name' => 'Bar', // FooBar
     'bars' => [ // array of objects
         ['id' => 1], // transforms into an object BarDto
         ['id' => 2], // transforms into an object BarDto
     ],
     'bar'  => ['id' => 3], // transforms into an object BarDto
     'baz'  => new BarDto(['id' => 4]), // just set object
     'time' => '05:59',
 ]);

 // Get all attributes
 $arr = $fooDto->toArray();
 // Serialize to json (also serializes all included DTOs)
 $json = json_encode($fooDto);
```
