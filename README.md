# Sylius Brand Plugin

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE)
[![Build Status][ico-travis]][link-travis]
[![Coverage Status][ico-scrutinizer]][link-scrutinizer]
[![Quality Score][ico-code-quality]][link-code-quality]

## Installation

### Step 1: Download the plugin

Open a command console, enter your project directory and execute the following command to download the latest stable version of this bundle:

```bash
$ composer require loevgaard/sylius-brand-plugin
```

This command requires you to have Composer installed globally, as explained in the [installation chapter](https://getcomposer.org/doc/00-intro.md) of the Composer documentation.


### Step 2: Enable the plugin

Then, enable the plugin by adding it to the list of registered plugins/bundles
in the `app/AppKernel.php` file of your project:

```php
<?php
// app/AppKernel.php

use Sylius\Bundle\CoreBundle\Application\Kernel;

final class AppKernel extends Kernel
{
    public function registerBundles(): array
    {
        return array_merge(parent::registerBundles(), [
            // ...
            new \Loevgaard\SyliusBrandPlugin\LoevgaardSyliusBrandPlugin(),
            // ...
        ]);
    }
    
    // ...
}
```

### Step 3: Configure the plugin

```yaml
# app/config/config.yml

imports:
    # ...
    - { resource: "@LoevgaardSyliusBrandPlugin/Resources/config/config.yml" }
```

```yaml
# app/config/routing.yml

loevgaard_sylius_brand:
    resource: "@LoevgaardSyliusBrandPlugin/Resources/config/routing.yml"
```

```yaml
# src/AppBundle/Resources/config/doctrine/Product.orm.yml

AppBundle\Entity\Product:
    type: entity
    table: sylius_product
    manyToOne:
        brand:
            targetEntity: Loevgaard\SyliusBrandPlugin\Entity\Brand
            joinColumn:
                name: brand_id
                referencedColumnName: id
```

### Step 4: Import product trait

```php
<?php declare(strict_types = 1);

namespace AppBundle\Entity;

use Loevgaard\SyliusBrandPlugin\Entity\ProductTrait as ProductBrandTrait;
use Sylius\Component\Core\Model\Product as BaseProduct;

class Product extends BaseProduct
{
    use ProductBrandTrait;
    
    // ...
}
```

### Step 5: Update your database schema
```bash
$ php bin/console doctrine:schema:update --force
```

or use [Doctrine Migrations](https://symfony.com/doc/master/bundles/DoctrineMigrationsBundle/index.html).

### Step 6: Add form widget to twig template
You need to override the template displaying the product form and add a `form_row` statement with the brand. In the example below I have added it below the channels widget.

```twig
{# app/Resources/SyliusAdminBundle/views/Product/Tab/_details.html.twig #}

{# ... #}

<div class="column">
    {{ form_row(form.channels) }}

    {{ form_row(form.brand) }}

    {% if product.simple %}
        <h4 class="ui dividing header">{{ 'sylius.ui.pricing'|trans }}</h4>
        {{ form_row(form.variant.channelPricings, {'label': false}) }}
    {% endif %}
</div>

{# ... #}
```

## Usage

TODO

[ico-version]: https://img.shields.io/packagist/v/loevgaard/sylius-brand-plugin.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/loevgaard/SyliusBrandPlugin/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/loevgaard/SyliusBrandPlugin.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/loevgaard/SyliusBrandPlugin.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/loevgaard/sylius-brand-plugin
[link-travis]: https://travis-ci.org/loevgaard/SyliusBrandPlugin
[link-scrutinizer]: https://scrutinizer-ci.com/g/loevgaard/SyliusBrandPlugin/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/loevgaard/SyliusBrandPlugin