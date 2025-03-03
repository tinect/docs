# Field inheritance

## Overview

In this guide you'll learn how to create inherited fields for your entities.
Field inheritance allows you to tell Shopware which fields should inherit values from a parent entity.

## Prerequisites

This guide is built upon the [Plugin Base Guide](../../plugin-base-guide.md), but any plugin will work here. Just note that all examples are using the plugin mentioned above.

You also should have a look at our [Adding custom complex data](./add-custom-complex-data.md) guide, since this guide is built upon it.

## Inherit a field

To start using inheritance, we have to update our definition and database.

1. Make inheritable fields nullable in the database
2. Add the `ParentFkField`, `ParentAssociationField`, `ChildrenAssociationField` in your definition
3. Enable inheritance by overwriting `isInheritanceAware()`
4. Flag fields as inheritable
5. Add getters and setters to the entity class

### Make fields nullable

The first thing we need to do is to make all our fields that we want to make inheritable nullable in our migration.
If you lack knowledge about migrations, have a look at our [database migrations](../../plugin-fundamentals/database-migrations.md) guide.

```sql
ALTER TABLE `swag_example` MODIFY `description` VARCHAR(255) NULL;
```

To avoid creating the column incorrectly, you can simply use the `Shopware\Core\Framework\Migration\InheritanceUpdaterTrait` in your migrations for new fields:

{% code title="<plugin root>/src/Migration/Migration1615363012AddInheritanceColumnToExample.php" %}
```php
<?php declare(strict_types=1);

namespace Swag\BasicExample\Migration;

use Doctrine\DBAL\Connection;
use Shopware\Core\Framework\Migration\InheritanceUpdaterTrait;
use Shopware\Core\Framework\Migration\MigrationStep;

class Migration1615363012AddInheritanceColumnToExample extends MigrationStep
{
    use InheritanceUpdaterTrait;

    public function getCreationTimestamp(): int
    {
        return 1615363012;
    }

    public function update(Connection $connection): void
    {
        $this->updateInheritance($connection, 'swag_example', 'example_field');
    }

    public function updateDestructive(Connection $connection): void
    {
    }
}
```
{% endcode %}

### Add the ParentFkField and the associations

After we've made all our fields nullable, we still need to add the following fields to our definition:
`Shopware\Core\Framework\DataAbstractionLayer\Field\ParentFkField`, `Shopware\Core\Framework\DataAbstractionLayer\Field\ParentAssociationField` 
and `Shopware\Core\Framework\DataAbstractionLayer\Field\ChildrenAssociationField`.
- `ParentFkField`: Is the foreign key, that references the parent's id. 
- `ParentAssociationField`: Field that the DAL knows where to load the parent association from.
- `ChildrenAssociationField`: Field that the DAL knows where to load the children association from.
  
In default, it points to a `parent_id` column in the database.
All these fields must refer to our definition by using `self::class`. The `ParentAssociationField` has as its second parameter the referenceField, which in our case is `id`.
Below you can find an example of how it should then look.

{% code title="<plugin root>/src/Core/Content/Example/ExampleDefinition.php" %}
```php
protected function defineFields(): FieldCollection
{
    return new FieldCollection([
        ...
        
        new ParentFkField(self::class),
        new ParentAssociationField(self::class, 'id'),
        new ChildrenAssociationField(self::class),
        
        ...
    ]);
}
```
{% endcode %}

### Allow inheritance

Now we need to enable inheritance by overriding the `isInheritanceAware` method in our definition, which must then return `true`.

{% code title="<plugin root>/src/Core/Content/Example/ExampleDefinition.php" %}
```php
public function isInheritanceAware(): bool
{
    return true;
}
```
{% endcode %}

### Flag fields as inheritable

After we've enabled inheritance for our definition, we need to add the`Shopware\Core\Framework\DataAbstractionLayer\Field\Flag\Inherited` 
flag to all the fields in our definition that should be inherited.

{% code title="<plugin root>/src/Core/Content/Example/ExampleDefinition.php" %}
```php
protected function defineFields(): FieldCollection
{
    return new FieldCollection([
        (new IdField('id', 'id'))->addFlags(new Required(), new PrimaryKey()),
        
        new ParentFkField(self::class),
        new ParentAssociationField(self::class, 'id'),
        new ChildrenAssociationField(self::class),

        (new StringField('name', 'name'))->addFlags(new Inherited()),
        (new StringField('description', 'description'))->addFlags(new Inherited()),
        (new BoolField('active', 'active'))->addFlags(new Inherited()),
    ]);
}
```
{% endcode %}

### Add getters and setters to the entity class

The last thing we need to do is add our new fields to our entity class.

{% code title="<plugin root>/src/Core/Content/Example/ExampleEntity.php" %}
```php
<?php declare(strict_types=1);

namespace Swag\BasicExample\Core\Content\Example;

use Shopware\Core\Framework\DataAbstractionLayer\Entity;
use Shopware\Core\Framework\DataAbstractionLayer\EntityIdTrait;

class ExampleEntity extends Entity
{
    ...

    /**
     * @var self|null
     */
    protected $parent;

    /**
     * @var string|null
     */
    protected $parentId;

    /**
     * @var ExampleCollection|null
     */
    protected $children;

    ...

    public function getParent(): ?ExampleEntity
    {
        return $this->parent;
    }

    public function setParent(ExampleEntity $parent): void
    {
        $this->parent = $parent;
    }

    public function getParentId(): ?string
    {
        return $this->parentId;
    }

    public function setParentId(?string $parentId): void
    {
        $this->parentId = $parentId;
    }

    public function getChildren(): ?ExampleCollection
    {
        return $this->children;
    }

    public function setChildren(ExampleCollection $children): void
    {
        $this->children = $children;
    }
}
```
{% endcode %}

## Translations

This concept also supports translations. Given a parent/child entity with an
inherited language (de-CH *inherits from* de-DE), the inheritance system will try to look up 
the values in following order:

1. Child (de-CH)
2. Child (de-DE)
3. Parent (de-CH)
4. Parent (de-DE)

If an inheritance is not found, the next translation in the chain above will be used.

### Enable translation inheritance

Assuming our definition is already aware of inheritance, we have to update 
our definition and add the `Inherited` flag to our translated fields and
the translation association.

```php
(new TranslatedField('name'))->addFlags(new Inherited()),
(new TranslationsAssociationField(ExampleTranslationDefinition::class))->addFlags(new Inherited()),
```