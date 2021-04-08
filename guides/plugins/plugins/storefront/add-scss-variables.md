# Add SCSS variables

## Overview

To add SCSS variables to your plugin you can use a subscriber class to add custom SCSS variables.

## Prerequisites

You won't learn how to create a plugin in this guide, head over to our Plugin base guide to create
your first plugin.

{% page-ref page="./../plugin-base-guide.md" %}

## Setup a default value for a custom SCSS variable

Before you start adding your subscriber you should provide a fallback value for your custom SCSS variable in your plugin `base.scss`:

{% code title="<plugin root>/src/Resources/app/storefront/src/scss/base.scss" %}
```scss
// The value will be overwritten by the subscriber when the plugin is installed and activated
$sass-plugin-header-bg-color: #ffcc00 !default;

.header-main {
    background-color: $sass-plugin-header-bg-color;
}
```
{% endcode %}

## Theme variables subscriber

You can add a new subscriber according to the [Listening to events](../plugin-fundamentals/listening-to-events.md) guide.
In this example we name the subscriber `ThemeVariableSubscriber.php`. The subscriber listens to the `ThemeCompilerEnrichScssVariablesEvent`.

{% tabs %}
{% tab title="<plugin root>/src/Subscriber/ThemeVariablesSubscriber.php" %}
```php
<?php declare(strict_types=1);

namespace Swag\BasicExample\Subscriber;

use Shopware\Storefront\Event\ThemeCompilerEnrichScssVariablesEvent;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

class ThemeVariablesSubscriber implements EventSubscriberInterface
{
    public static function getSubscribedEvents(): array
    {
        return [
            ThemeCompilerEnrichScssVariablesEvent::class => 'onAddVariables'
        ];
    }

    public function onAddVariables(ThemeCompilerEnrichScssVariablesEvent $event)
    {
        // Will render: $sass-plugin-header-bg-color: "#59ccff";
        $event->addVariable('sass-plugin-header-bg-color', '#59ccff');
    }
}
```
{% endtab %}
{% tab title="<plugin root>/src/Resources/config/services.xml" %}
```xml
<?xml version="1.0" ?>

<container xmlns="http://symfony.com/schema/dic/services"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">
    
    <services>
        <service id="Swag\BasicExample\Subscriber\ThemeVariablesSubscriber">
            <tag name="kernel.event_subscriber"/>
        </service>
    </services>
</container>
```
{% endtab %}
{% endtabs %}

The `ThemeCompilerEnrichScssVariablesEvent` provides the `addVariable()` method which takes the following parameters:

* `$name:` (string): The name of the SCSS variable. The passed string will be exactly in your SCSS so please be careful with special characters. We recommend using kebab-case here. The variable prefix `$` will be added automatically. We also recommend prefixing your variable name with your plugin's or company's name to prevent naming conflicts.
* `$value:` (string): The value which should be assigned to the SCSS variable.
* `$sanitize` (bool - optional): Optional parameter to remove special characters from the variables value. The parameter will also add quotes around the variables value. In most cases quotes are not needed e.g. for color hex values. But there may be situations where you want to pass individual strings to your SCSS variable.

{% hint style="info" %}
Please note that plugins are not sales-channel specific. Your SCSS variables are directly added in the SCSS compilation process and will be globally available throughout all themes and storefront sales-channels. If you want to change a variables value for each sales-channel you should use plugin config fields and follow the next example.
{% endhint %}

## Plugin config values as SCSS variables

Inside your `ThemeVariablesSubscriber.php` you can also read values from the plugin configuration and assign those to a SCSS variable. This makes it also possible to have different values for each sales-channel. Depending on the selected sales-channel inside the plugin configuration in the administration.

First of all lets add a new plugin configuration field according to the [Plugin Configurations](../plugin-fundamentals/add-plugin-configuration.md):

{% code title="<plugin root>/src/Resources/config/config.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="https://raw.githubusercontent.com/shopware/platform/trunk/src/Core/System/SystemConfig/Schema/config.xsd">

    <card>
        <title>Example configuration</title>
        <input-field>
            <name>sassPluginHeaderBgColor</name>
            <label>Header background color</label>
        </input-field>
    </card>
</config>
```
{% endcode %}

To be able to read this config you have to add the `SystemConfigService` to your subscriber:

{% tabs %}
{% tab title="<plugin root>/src/Subscriber/ThemeVariablesSubscriber.php" %}
```php
<?php declare(strict_types=1);

namespace Swag\BasicExample\Subscriber;

use Shopware\Core\System\SystemConfig\SystemConfigService;
use Shopware\Storefront\Event\ThemeCompilerEnrichScssVariablesEvent;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

class ThemeVariablesSubscriber implements EventSubscriberInterface
{
    /**
     * @var SystemConfigService
     */
    protected $systemConfig;

    // add the `SystemConfigService` to your constructor
    public function __construct(SystemConfigService $systemConfig)
    {
        $this->systemConfig = $systemConfig;
    }

    public static function getSubscribedEvents(): array
    {
        return [
            ThemeCompilerEnrichScssVariablesEvent::class => 'onAddVariables'
        ];
    }

    public function onAddVariables(ThemeCompilerEnrichScssVariablesEvent $event)
    {
        /** @var string $configExampleField */
        $configPluginHeaderBgColor = $this->systemConfig->get('SwagBasicExample.config.sassPluginHeaderBgColor', $event->getSalesChannelId());

        // pass the value from `configPluginHeaderBgColor` to `addVariable`
        $event->addVariable('sass-plugin-header-bg-color', $configPluginHeaderBgColor);
    }
}
```
{% endtab %}
{% tab title="<plugin root>/src/Resources/config/services.xml" %}
```xml
<?xml version="1.0" ?>

<container xmlns="http://symfony.com/schema/dic/services"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">
    
    <services>
        <service id="Swag\BasicExample\Subscriber\ThemeVariablesSubscriber">
            <!-- add argument `SystemConfigService` -->
            <argument type="service" id="Shopware\Core\System\SystemConfig\SystemConfigService"/>
            <tag name="kernel.event_subscriber"/>
        </service>
    </services>
</container>
```
{% endtab %}
{% endtabs %}

* The `SystemConfigService` provides a `get()` method where you can access the configuration structure in the first parameter with a dot notation syntax like `SwagBasicExample.config.fieldName`. The second parameter is the sales-channel `id`. With this `id` the config fields can be accessed for each sales-channel.
* You can get the sales-channel id through the getter `getSalesChannelId()` of the `ThemeCompilerEnrichScssVariablesEvent`.
* Now your sass variables can have different values in each sales-channel.

### All config fields as SCSS variables

Adding config fields via `$event->addVariable()` individually for every field may be a bit cumbersome in some cases. You could also loop over all config fields and call `addVariable()` for each config field. But this depends on your use case.

{% code title="<plugin root>/src/Subscriber/ThemeVariablesSubscriber.php" %}
```php
<?php declare(strict_types=1);

namespace Swag\BasicExample\Subscriber;

// ...
use Symfony\Component\Serializer\NameConverter\CamelCaseToSnakeCaseNameConverter;

class ThemeVariablesSubscriber implements EventSubscriberInterface
{
    // ...

    public function onAddVariables(ThemeCompilerEnrichScssVariablesEvent $event)
    {
        $configFields = $this->systemConfig->get('SwagBasicExample.config', $event->getSalesChannelId());

        foreach($configFields as $key => $value) {
            // Convert `customVariableName` to `custom-variable-name`
            $variableName = str_replace('_', '-', (new CamelCaseToSnakeCaseNameConverter())->normalize($key));

            $event->addVariable($variableName, $value);
        }
    }
}
```
{% endcode %}

To avoid camelCase variable names when reading from the `config.xml` we recommend using the `CamelCaseToSnakeCaseNameConverter` to format the variable before adding it.
