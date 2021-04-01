# Using a modal window

## Overview

This guide explains how you can use a modal window in your plugin.

## Prerequisites

This guide requires you to already have a basic plugin running. If you don't know how to do this in the first place, have a look at our [Plugin base guide](../plugin-base-guide.md).

While this is not mandatory, having read the guide about [adding custom javascript](./add-custom-javascript.md) plugins beforehand might help you understand this guide a bit further.

## Create a modal window

To create a modal window you can use the `PseudoModalUtil` in your plugin.

Like in the [adding custom javascript](./add-custom-javascript.md) we load our JavaScript plugin by creating `index.html.twig` template in the `<plugin root>/src/Resources/views/storefront/page/content/` folder.
Inside this template, extend from the `@Storefront/storefront/page/content/index.html.twig` and overwrite the `base_main_inner` block. After the parent content of the blog, add a template tag that has the `data-example-plugin` attribute.

{% code title="<plugin root>/src/Resources/views/storefront/page/content/index.html.twig" %}
```text
{% sw_extends '@Storefront/storefront/page/content/index.html.twig' %}

{% block base_main_inner %}
    {{ parent() }}

    <template data-example-plugin></template>
{% endblock %}
```
{% endcode %}

Now we need to register the plugin which should create a modal in the `PluginManager`. To achieve that you can add the following code to the `main.js` file.

{% code title="<plugin root>/src/Resources/app/storefront/src/main.js" %}
```javascript
 // Import all necessary Storefront plugins
 import ExamplePlugin from './example-plugin/example-plugin.plugin';

 // Register your plugin via the existing PluginManager
 const PluginManager = window.PluginManager;
 PluginManager.register('ExamplePlugin', ExamplePlugin, '[data-example-plugin]');
```
{% endcode %}

Now let's get started with the modal window. First we have to import the `PseudoModalUtil` class in our plugin.

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        // ...
    }
}
```
{% endcode %}

Now we create a new modal instance using `new PseudoModalUtil()` and assign to a property of our plugin for later usage.
We also call the method `open()` to make it visible. 

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        this.openModal();
    }
    
    openModal() {
        // create a new modal instance
        this.modal = new PseudoModalUtil();
        
        // open the modal window and make it visible
        this.modal.open();
    }
}
```
{% endcode %}

To see your changes you have to build the storefront. Use the `./psh.phar storefront:build` command and reload your storefront.
You can see now a blank modal which contains `undefined`. This is because we have not added any content to show inside the modal.

The constructor method of `PseudoModalUtil()` expects some HTML `content` to display. To keep this guide simple, we are only including sample code here.
Of course, the content can also be generated via an API and inserted via AJAX requests.

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        // declaring some basic content
        const content = `
            <div class="js-pseudo-modal-template">
                <div class="js-pseudo-modal-template-title-element">Modal title</div>
                <div class="js-pseudo-modal-template-content-element">Modal content</div>
            </div>
        `;
        
        this.openModal(content);
    }
    
    openModal(content) {
        // create a new modal instance
        this.modal = new PseudoModalUtil(content);
        
        // open the modal window and make it visible
        this.modal.open();
    }
}
```
{% endcode %}

## Closing the modal

The `PseudoModalUtil` class also provide a `close()` method. As with opening the modal by calling `this.modal.open()`, you can simply close the modal with `this.modal.close()`.

## Callback when opening a modal

The `open()` method of the `PseudoModalUtil` class supports a callback function as an argument. So, if you need to perform some action when your modal opens you can implement a callback like this:

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        // declaring some basic content
        const content = `
            <div class="js-pseudo-modal-template">
                <div class="js-pseudo-modal-template-title-element">Modal title</div>
                <div class="js-pseudo-modal-template-content-element">Modal content</div>
            </div>
        `;
        
        this.openModal(content);
    }
    
    openModal(content) {
        // create a new modal instance
        this.modal = new PseudoModalUtil(content);
        
        // open the modal window and fire a callback function
        this.modal.open(this.onOpenModal.bind(this));
    }
    
    onOpenModal() {
        console.log('the modal is opened');
    }
}
```
{% endcode %}

## Updating the modal content

To update the content of a modal, `PseudoModalUtil` provides a method `updateContent()` to which you can pass the updated template string. The method also accepts a callback function as a second argument, which is called after the content has been updated.
Here is an example how to use it:

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        // declaring some basic content
        const content = `
            <div class="js-pseudo-modal-template">
                <div class="js-pseudo-modal-template-title-element">Modal title</div>
                <div class="js-pseudo-modal-template-content-element">Modal content</div>
            </div>
        `;
        
        this.openModal(content);
        
        // ... do some stuff

        const updatedContent = `
            <div class="js-pseudo-modal-template">
                <div class="js-pseudo-modal-template-title-element">Modal title</div>
                <div class="js-pseudo-modal-template-content-element">Updated content</div>
            </div>
        `;
        
        this.modal.updateModal(updatedContent, this.onUpdateModal.bind(this));
    }
    
    openModal(content) {
        // create a new modal instance
        this.modal = new PseudoModalUtil(content);
        
        // open the modal window and fire a callback function
        this.modal.open(this.onOpenModal.bind(this));
    }
    
    onOpenModal() {
        console.log('the modal is opened');
    }

    onUpdateModal() {
        console.log('the modal was updated');
    }
    
}
```
{% endcode %}

## Customize the modal appearance

The constructor method of `PseudoModalUtil` provides optional configuration. If you don't need backdrop of the modal for example just turn it off by instantiate the modal like this

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
import Plugin from 'src/plugin-system/plugin.class';
import PseudoModalUtil from 'src/utility/modal-extension/pseudo-modal.util';

export default class ExamplePlugin extends Plugin {
    init() {
        // declaring some basic content
        const content = `
            <div class="js-pseudo-modal-template">
                <div class="js-pseudo-modal-template-title-element">Modal title</div>
                <div class="js-pseudo-modal-template-content-element">Modal content</div>
            </div>
        `;
        
        this.openModal(content);
    }
    
    openModal(content) {
        // disable backdrop
        const useBackrop = false;
        
        // create a new modal instance
        this.modal = new PseudoModalUtil(content, useBackrop);
        
        // open the modal window and make it visible
        this.modal.open();
    }
}
```
{% endcode %}

As you can see in the sample code we are using the `js-pseudo-modal-template-title-element` class to style the title text of the modal.
It also tells the `PseudoModalUtil` class that the content of the `div` holds the title text.
Furthermore there are 2 more css selectors `js-pseudo-modal-template` and `js-pseudo-modal-template-content-element` to define structure of the template string.

If want to customize your modal by using different style classes, your can do that by overriding the defaults while instantiating `PseudoModalUtil`.

Here is a example which shows how to override the CSS class names.

{% code title="<plugin root>/src/Resources/app/storefront/src/example-plugin/example-plugin.plugin.js" %}
```javascript
// ...
export default class ExamplePlugin extends Plugin {
    init() {
        // ...
    }

    openModal(content) {
        // enable backdrop
        const useBackrop = true;

        // create a new modal instance
        this.modal = new PseudoModalUtil(
            content,
            useBackrop,
            'custom-js-pseudo-modal-template',
            'custom-js-pseudo-modal-template-content-element',
            'custom-js-pseudo-modal-template-title-element'
        );

        // open the modal window and make it visible
        this.modal.open();
    }
}
```
{% endcode %}