# Add custom listing filters

## Overview

## Prerequisites

Before you starting reading this guide, make sure you got an own plugin installed to work with. If you need a starting 
point for that, see this guide:

{% page-ref page="./../plugin-base-guide.md" %}

## Adding new Filter

At first, you need to create a subscriber -  in this example we'll call it `ExampleListingSubscriber`. If you're not
sure on working with subscribers, please refer to the guide on working with events in Shopware:

{% page-ref page="./../plugin-fundamentals/listening-to-events.md" %}

New listing filters, e.g. for your product listing, can be registered via the event `\Shopware\Core\Content\Product\Events\ProductListingCollectFilterEvent` 
This event was introduced to enable every developer to specify the metadata for a filter. The handling, meaning if and 
how a filter is added, is done by Shopware's core:

```javascript
    public static function getSubscribedEvents()
    {
        return [
            ProductListingCollectFilterEvent::class => 'addFilter'
        ];
    }
```

After that, you can start to actually add your custom filters. Arguably an important step is to define your filter.
Therefore, you're able to use the `Filter` class, including the parameters below:

| Parameter | Description |
| --- | --- | 
| `name` | Unique name of the filter |
| `filtered` | Set this option to `true` if this filter is active |
| `aggregations` | Defines aggregations behind a filter. Sometimes a filter contains multiple aggregations like properties |
| `filter` | Sets the DAL filter which should be added to the criteria    |
| `values` | Defines the values which will be added as `currentFilter` to the result |
| `exclude` | Configure exclusions |

As a result, an example filter could look like this:

```php
$filter = new Filter(
    // name
    'manufacturer',
    
    // filtered
    !empty($ids),
    
    // aggregations
    [new EntityAggregation('manufacturer', 'product.manufacturerId', 'product_manufacturer')],
    
    // filter
    new EqualsAnyFilter('product.manufacturerId', $ids),
    
    // values
    $ids
);
```

So you get the existing filters by `ProductListingCollectFilterEvent`, define your new custom filter and add it to the
existing ones. Here is a complete example implementation, please note the comments for explanation:

{% code title="<plugin root>/src/Subscriber/ExampleListingSubscriber.php" %}
```php
class ExampleListingSubscriber implements EventSubscriberInterface
{
    // Register event
    public static function getSubscribedEvents()
    {
        return [
            ProductListingCollectFilterEvent::class => 'addFilter'
        ];
    }

    public function handleRequest(ProductListingCollectFilterEvent $event)
    {
        // Fetch existing filters
        $filters = $event->getFilters();

        // Get filter values
        $ids = $this->getManufacturerIds($request);

        $filter = new Filter(
            //unique name of the filter
            'manufacturer',
            
            // defines if this filter is active
            !empty($ids),
            
            // defines aggregations behind a filter. Sometimes a filter contains multiple aggregations like properties
            [new EntityAggregation('manufacturer', 'product.manufacturerId', 'product_manufacturer')],
            
            // defines the DAL filter which should be added to the criteria   
            new EqualsAnyFilter('product.manufacturerId', $ids),
            
            // defines the values which will be added as currentFilter to the result
            $ids
        );
        
        // Add your custom filter
        $filters->add($filter);
    }

    // Get manufacturer IDs to use as filter values later
    private function getManufacturerIds(Request $request): array
    {
        $ids = $request->query->get('manufacturer', '');
        $ids = explode('|', $ids);

        return array_filter($ids);
    }
}
```
{% endcode %}

## Next steps

Are you interested to add a custom sorting to your listing in the storefront, as well? Head over to the corresponding
guide to learn more about that:

{% page-ref page="./../storefront/add-custom-sorting-product-listing.md" %}
