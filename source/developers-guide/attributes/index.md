---
layout: default
title: Extending shopware tables
github_link: developers-guide/attributes/index.md
---

## Overview
For many plugins it is required to add own fields to a database table - for example in order to extend shopware's
user entity by a field "shoe size". This document will describe the current best practice for that.

## The attribute system
As shopware makes heavily use of Doctrine ORM for the backend and the API, it is not recommended to directly modify
any of the default tables like `s_user` or `s_articles`: Those changes will not be reflected in the ORM and might cause
issues in various situations.

Instead of that, there are so called "attribute" tables for all main entities of shopware: So for `s_user` there is
`s_user_attributes` and for `s_categories` there is `s_categories_attributes`. For those attribute tables, the models
are generated automatically, so that custom columns are also reflected in the ORM. Furthermore shopware automatically
links those models, so that e.g. `Shopware\Models\Customer\Customer` always has a `Shopware\Models\Attribute\Customer`
associated. So even if you are not encouraged to modify the `Customer` model directly, you are able to modify the
associated `Attribute\Customer` model and bring you "shoe size" into play.

## Using it
Creating / removing attributes usually happens during install or uninstall time. Three methods are available:

 * `Shopware()->Models()->addAttribute`: Extend a table by a column
 * `Shopware()->Models()->generateAttributeModels`: Generate the attribute model for Doctrine ORM
 * `Shopware()->Models()->removeAttribute`: Remove a column from a attribute table

The simple example below will extend the table `s_user_attribute` by a column `swag_customer_preferences_size` and
generated the corresponding attributes:

```
class Shopware_Plugins_Backend_SwagCustomerPreferences_Bootstrap extends Shopware_Components_Plugin_Bootstrap
{
    public function getVersion()
    {
        return '1.0.0';
    }

    public function getLabel()
    {
        return 'Customer preferences';
    }

    public function install()
    {
        Shopware()->Models()->addAttribute(
            's_user_attributes',
            'swag',
            'customer_preferences_size',
            'int(11)',
            true,
            null
        );

        Shopware()->Models()->generateAttributeModels(array(
            's_user_attributes'
        ));

        return true;
    }

    public function uninstall()
    {
        Shopware()->Models()->removeAttribute(
            's_user_attributes',
            'swag',
            'customer_preferences_size'
        );

        Shopware()->Models()->generateAttributeModels(array(
            's_user_attributes'
        ));

        return true;
    }

}
```

The `addAttribute` method takes the following parameters:

 * `$table`: The attribute table to be extended, e.g. `s_user_attributes`
 * `$prefix`: You company's developer prefix, e.g. `swag`. This will avoid name collisions with other developers
 * `$column`: The column name, e.g. `customer_preferences_size`
 * `$type`: MySQL type of the column, e.g. `INT(11)`
 * `$nullable` (default: `true`): Whether or not this column requires a value. Be aware, that setting this value to
 `false` might lead to problems, if your customer disabled your plugin: The column will remain in the database,
 as your plugin is not active any more, no logic will be executed to fill the column and errors might occur.
 So either clean up the column in your plugin's `enable` / `disable` method or avoid creating non-nullable columnss
 entirely.
 * `$default` (default: `null`):  Default value of your column. As explained in the section above, it is not recommended
 to rely on this option when working with non-nullable columns.


After installing the plugin, the new attribute can be accessed like this:

```
Shopware()->Models()->getRepository('Shopware\Models\Customer\Customer')
    ->find(1)
    ->getAttribute()
    ->setSwagCustomerPreferencesSize(6)
```

This will find the customer with ID `1`, access the attribute model and set the shoe size to 6. Be aware, that
the getters and setters are automatically created from the developer prefix (`swag` in this case) and the attribute name
(`customer_preferences_size`). Furthermore underscores are converted to caps.

## TO_MANY relations
Shopware's attribute system is ideal for so called `TO_ONE` relations: *One* row in an `*_attributes` tables always relates
to exact *one* row in the corresponding entity table. If you need to have any kind of `TO_MANY` relations (e.g. one
customer can have multiple shoe sizes or many customers share the exact same shoe size), this cannot be modelled using
attributes (except serialisation is an option). In those cases it is recommended to create own tables with own Doctrine
ORM models.

The downside of this approach is the fact, that shopware's native models will not have a relation to your custom models
(the `Customer` does not have a notion of your `ShoeSize` model). But as you are still able to create an association
from your model to shopware's native models, this is not a major issue in most cases (your `ShoeSize` model does have
a notion of the corresponding `Customer`(s)). 