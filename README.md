# Laravel Scout Elasticsearch Driver

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)

This package makes is the [Elasticsearch](https://www.elastic.co/products/elasticsearch) driver for Laravel Scout.

## Contents

- [Installation](#installation)
- [Usage](#usage)
- [Credits](#credits)
- [License](#license)

## Installation

You can install the package via composer:

``` bash
composer require Crasmedia/laravel-scout-elastic
```

You must add the Scout service provider and the package service provider in your app.php config:

```php
// config/app.php
'providers' => [
    ...
    Laravel\Scout\ScoutServiceProvider::class,
    ...
    ScoutEngines\Elasticsearch\ElasticsearchProvider::class,
],
```

### Setting up Elasticsearch configuration
You must have a Elasticsearch server up and running with the index you want to use created

If you need help with this please refer to the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

After you've published the Laravel Scout package configuration:

```php
// config/scout.php
// Set your driver to elasticsearch
    'driver' => env('SCOUT_DRIVER', 'elasticsearch'),

...
    'elasticsearch' => [
        'index' => env('ELASTICSEARCH_INDEX', 'laravel'),
        'hosts' => [
            env('ELASTICSEARCH_HOST', 'http://localhost'),
        ],
    ],
...
```

## Usage

Now you can use Laravel Scout as described in the [official documentation](https://laravel.com/docs/5.3/scout)

## Custom modifications
You can create/delete your own indexes and put custom mappings on it
```php
$index = config('scout.elasticsearch.index');

// Check if index exists
if($model->searchableUsing()->exists($index))
{
    // Delete index if exists
    $model->searchableUsing()->deleteIndex($index);
}

// Create new index
$model->searchableUsing()->createIndex($index);

// Put custom mapping
$mappings = $model->mapping();
$model->searchableUsing()->putMapping($index, $model->getTable(), $mappings);
```

For using the custom mapping just add a mapping function to your model
```php
public function mapping()
{
    $mapping = [
        'title' => [
            'type' => 'keyword'
        ]
    ];

    return $mapping;
}
```
## Boost
If you wanna boost some fields, just add a boosts variable to your model
```php
protected $boosts = [
    'title' => 2
];
```
[More information on elastic site](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-match-query.html)

## Sort
Just use the eloquent order by function
```php
Model::search('')->orderBy('title', 'asc')->get()
```

## Credits

- [Erick Tamayo](https://github.com/ericktamayo)
- [Rutger Daling](https://github.com/roetker)
- [All Contributors](../../contributors)

## License

The MIT License (MIT).
