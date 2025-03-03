# Jest unit tests in Shopware's administration 

## Overview

You should write a unit test for every functional change. It should guarantee that your written code works and that a third developer can't break the functionality with their code.

With a good test coverage we can have the confidence to deploy a stable software without needing to manually test the software in its entirety. This little guide will guide you how to write unit tests for the administration in Shopware 6.

We are using [Jest](https://jestjs.io) as our testing framework. It's a solid foundation and widely used by many developers. Before you are reading this guide you have to make sure you understand the basics of unit tests and how Jest works.

## Video

Did you know that there's a video available to this topic? Please take a look:

<!-- markdown-link-check-disable -->
{% embed url="https://www.youtube.com/watch?v=nWUBK3fjwVg" %}
<!-- markdown-link-check-enable -->

## Prerequisites

This tutorial will have a strong focus on how unit tests should be written when it comes to components in the administration. So please make sure you already know what a unit test is and why we are doing it. Furthermore, you should know what components tests are and what we want to achieve with them.
You can find a good source for best practices in this Github repository:

<!-- markdown-link-check-disable -->
{% embed url="https://github.com/goldbergyoni/javascript-testing-best-practices" %}
<!-- markdown-link-check-enable -->

In addition, you need a running Shopware 6 installation. Your repository used for that should be based on development template, as we need to use some scripts provided by it.

## Folder structure

The test folder structure should match the source folder structure. You add a test for a file in the same path as the source path. You see: When creating the file, the name should also be the same as the component has with an additional .spec before the file extension `.js`. The `.spec` suffix is a well-known naming convention of frontend testing files.

```bash
Resources
  `-- app
    `-- <environment>
      `-- test
        `-- app
          `-- component
            `-- base
              `-- sw-alert.spec.js
```

Please note that in this example, `<environment>` is a placeholder for the environment you are working in. In your context, that should be `administration`.

## Testing services and ES modules

Services and isolated ECMAScript modules are well testable because you can import them directly without mocking or stubbing dependencies. A service can be used isolated and therefore is easy to test.

Let's have a look at an example:

{% code title="sanitizer.helper.spec.js" %}
```javascript
import Sanitizer from 'src/core/helper/sanitizer.helper';

describe('core/helper/sanitizer.helper.js', () => {
    it('should sanitize the html', () => {
        expect(Sanitizer.sanitize('<A/hREf="j%0aavas%09cript%0a:%09con%0afirm%0d``">z'))
            .toBe('<a href="j%0aavas%09cript%0a:%09con%0afirm%0d``">z</a>');
    });

    it('should remove script functions from dom elements', () => {
        expect(Sanitizer.sanitize('<details open ontoggle=confirm()>'))
            .toBe('<details open=""></details>');
    });

    it('should remove script functions completely', () => {
        expect(Sanitizer.sanitize(`<script y="><">/*<script* */prompt()</script`))
            .toBe('');
    });

    it('should sanitize js in links', () => {
        expect(Sanitizer.sanitize('<a href=javas&#99;ript:alert(1)>click'))
            .toBe('<a>click</a>');
    });

    // ...more tests 
});
```
{% endcode %}

You see, you are able to write the test the same way you're used to, writing Jest unit tests in general.

## Write tests for components

After setting up your component test, you need to write your tests. A good way to write them is to test input and output. The most common tests are:

* set Vue Props and check if component looks correctly
* interact with the DOM and check if the desired behaviour is happening

However, when it comes to writing component tests for Shopware's administration, there are some further steps to go.
We will take a look at them in the following paragraphs.

## Setup for testing Vue components

We are using the [Vue Test Utils](https://vue-test-utils.vuejs.org) for easier testing of Vue components. If you don't have experience with testing Vue components it is useful to read some basic guides on this topic. The main part of testing components is similar in Shopware 6.

However, there are some important differences. We can't test components that easily like in other Vue projects because we are supporting template inheritance and extendability for third party developers. This causes overhead which we need to bear in mind.

We are using a global object as an interface for the whole administration. Every component gets registered to this object, e.g. `Shopware.Component.register()`. Therefore, we have access to Component with the `Shopware.Component.build()` method. This creates a native Vue component with a working template. Every override and extension from another components are resolved in the built component.

### Executing tests

Before you are using the commands make sure that you installed all dependencies for your administration. If you haven't done this already, then you can do it running the following PSH command: `./psh.phar administration:install-dependencies`

In order to run jest unit tests of the administration, you can use the psh commands provided by our development template. 

{% hint style="info" %} This only applies to the Shopware provided Administration! If you use unit tests in your plugin, you might need to write your own scripts for that.
{% endhint %}

This command executes all unit tests and shows you the complete code coverage.  
`./psh.phar administration:unit`

This command executes only unit tests of changed files. It automatically restarts if a file gets saved. This should be used during the development of unit tests.  
`./psh.phar administration:unit-watch`

## First example: Testing sw-multi-select component

For better understanding how to write component tests for Shopware 6 let's write a test. In our example we are using the component `sw-multi-select`.

When you want to mount your component it needs to be imported first:

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript
import 'src/app/component/form/select/base/sw-multi-select';
```
{% endcode %}

You see that we import the `sw-multi-select` without saving the return value. This blackbox import only executes code. However, this is important because this registers the component to the Shopware object:

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript
Shopware.Component.register('sw-multi-select', {
    // The vue component
});
```
{% endcode %}

### Mounting components

In the next step we can mount our Vue component which we get from the global Shopware object:

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript

import 'src/app/component/form/select/base/sw-multi-select';

shallowMount(Shopware.Component.build('sw-multi-select'));
```
{% endcode %}

When we’re testing our vue.js components, we need a way to mount and render the component. Therefore, we use the following methods:

* `mount()`: Creates a Wrapper that contains the mounted and rendered Vue component.
* `shallowMount()`: Like mount, it creates a Wrapper that contains the mounted and rendered Vue component, 

  but with stubbed child components.

This way, we create a new `wrapper` before each test. The `build` method resolves the twig template and returns a vue component.

### Test structure

Now you can test the component like any other component. Let's try to write our first test:

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript

import { shallowMount } from '@vue/test-utils';
import 'src/app/component/form/select/base/sw-multi-select';

describe('components/sw-multi-select', () => {
    let wrapper;

    beforeEach(() => {
        wrapper = shallowMount(Shopware.Component.build('sw-multi-select'));
    });

    afterEach(() => {
        wrapper.destroy();
    });

    it('should be a Vue.js component', () => {
        expect(wrapper.vm).toBeTruthy();
    });
});
```
{% endcode %}

This contains our component. In our first test we only check if the wrapper is a Vue instance.

### Running the test

Now lets start the watcher to see if the test works. You can do this with our PSH command `./psh.phar administration:unit-watch`. You should see a result like this: `Test Suites: 1 passed, 1 total`. You should also see several warnings like this:

* `[Vue warn]: Missing required prop: "options"`
* `[Vue warn]: Missing required prop: "value"`
* `[Vue warn]: Unknown custom element: <sw-select-base> - did you register the component correctly? ...`

The first two warnings are solved easily by providing the required props to our shallowMount:

```javascript
wrapper = shallowMount(Shopware.Component.build('sw-multi-select'), {
    props: {
        options: [],
        value: ''
    }
});
```

Now you should only see the last warning with an unknown custom element. The reason for this is that most components contain other components. In our case the `sw-multi-select` needs the `sw-select-base` component. Now we have several solutions to solve this. The two most common ways are stubbing or using the component.

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript
import 'src/app/component/form/select/base/sw-select-base';

wrapper = shallowMount(Shopware.Component.build('sw-multi-select'), {
    props: {
        options: [],
        value: ''
    },
    stubs: {
        'sw-select-base': Shopware.Component.build('sw-select-base'),
    }
});
```
{% endcode %}

You need to choose which way is needed: Many tests do not need the real component, but in our case we need the real implementation. You will see that if we import another component that they can create also warnings. Let's look at the code that solve all warnings, then we should have a code like this:

{% code title="test/app/component/form/select/base/sw-multi-select.spec.js" %}
```javascript
import { shallowMount } from '@vue/test-utils';
import 'src/app/component/form/select/base/sw-multi-select';
import 'src/app/component/form/select/base/sw-select-base';
import 'src/app/component/form/field-base/sw-block-field';
import 'src/app/component/form/field-base/sw-base-field';
import 'src/app/component/form/field-base/sw-field-error';
import 'src/app/component/form/select/base/sw-select-selection-list';
import 'src/app/component/form/select/base/sw-select-result-list';
import 'src/app/component/utils/sw-popover';
import 'src/app/component/form/select/base/sw-select-result';
import 'src/app/component/base/sw-highlight-text';
import 'src/app/component/base/sw-label';
import 'src/app/component/base/sw-button';

describe('components/sw-multi-select', () => {
    let wrapper;

    beforeEach(() => {
        wrapper = shallowMount(Shopware.Component.build('sw-multi-select'), {
            props: {
                options: [],
                value: ''
            },
            stubs: {
                'sw-select-base': Shopware.Component.build('sw-select-base'),
                'sw-block-field': Shopware.Component.build('sw-block-field'),
                'sw-base-field': Shopware.Component.build('sw-base-field'),
                'sw-icon': '<div></div>',
                'sw-field-error': Shopware.Component.build('sw-field-error'),
                'sw-select-selection-list': Shopware.Component.build('sw-select-selection-list'),
                'sw-select-result-list': Shopware.Component.build('sw-select-result-list'),
                'sw-popover': Shopware.Component.build('sw-popover'),
                'sw-select-result': Shopware.Component.build('sw-select-result'),
                'sw-highlight-text': Shopware.Component.build('sw-highlight-text'),
                'sw-label': Shopware.Component.build('sw-label'),
                'sw-button': Shopware.Component.build('sw-button')
            }
        });
    });

    afterEach(() => {
        wrapper.destroy();
    });

    it('should be a Vue.js component', () => {
        expect(wrapper.vm).toBeTruthy();
    });
});
```
{% endcode %}

## Second example: Testing of message inside the sw-alert component

Of course, the complexity and structure of your test depends on what you are trying to achieve with your component. Here is one little example concerning the component `sw-alert`: Actually, he task of an alert is displaying a message for the user in most cases. So in this example, let's write a test for this text located in a slot. You can find this example in the linked video above as well.

We will start with an already written test similar to the first example:

```javascript
import { shallowMount } from '@vue/test-utils';
import 'src/app/component/base/sw-alert';

describe('components/base/sw-alert', () => {
    it('should be a Vue.js component', () => {
        const wrapper = shallowMount(Shopware.Component.build('sw-alert'), {
            stubs: ['sw-icon']
        });

        // Assert if our component is a vue instance = mountes correctly
        expect(wrapper.vm).toBeTruthy();
    });
});
```

There we'll add another test case for testing the alert's message:

```javascript
import { shallowMount } from '@vue/test-utils';
import 'src/app/component/base/sw-alert';

describe('components/base/sw-alert', () => {
    it('should be a Vue.js component', () => {
        // see above
    });

    it('should render the message inside the default slot', () => {
        // New
    });
});
```

### Mounting components

You can set the content of a slot during component mount. See the paragraph "Mounting components" in the first example for details.

```javascript
    it('should render the message inside the default slot', () => {
        const wrapper = shallowMount(Shopware.Component.build('sw-alert'), {
            slots: {
                default: 'My custom message'
            }
        });
    });
```

Afterwards you can make an assertion that the text passed to the slot will be rendered inside the desired element. In this case we search in the wrapper for the element with the selector `.sw-alert__message` and check if the text is there:

```javascript
    it('should render the message inside the default slot', () => {
        const wrapper = shallowMount(Shopware.Component.build('sw-alert'), {
            slots: {
                default: 'My custom message'
            }
        });
        expect(wrapper.find('.sw-alert__message').text()).toBe('My custom message');
    });
```

## Stubbing your component

Vue Test Utils has some advanced features for stubbing components. A stub is actually when you replace an existing implementation of a custom component with a dummy component doing nothing at all, actually. This is necessary for the component to function independently, in an isolated way.

Components in Shopware might also depend on other dependencies like `$tc`, directives or injections. This way, the setup of your test may get more complex. When you are building components then you need to mock their dependencies as well.

### Using mocks

A common warning can occur if you didn't mock functions needed in your component. For example, please look at the warning below:

> \[Vue warn\]: Error in render: "TypeError: $tc is not a function"

The solution is using mocks while mounting your component. In this case, your mock here is a simple function returning the translation path.

```javascript
shallowMount(Shopware.Component.build('your-component'), {
    mocks: {
        $tc: key => key // your mock (here a simple function returning the translation path)
    }
});
```

### Stubbing directives

When working with components of Shopware's Administration, you might stumble upon the following warning:

> \[Vue warn\]: Failed to resolve directive: popover

If that happens, you need to use [localVue](https://vue-test-utils.vuejs.org/api/#createlocalvue) to provide the directive mock. `createLocalVue` returns a Vue class for you to add components, mixins and install plugins without polluting the global Vue class. In our context, it looks like this:

```javascript
import { shallowMount, createLocalVue } from '@vue/test-utils';

const localVue = createLocalVue();
localVue.directive('popover', {}); // add directive mock to localVue

shallowMount(Shopware.Component.build('your-component'), {
    localVue
});
```

### Stubbing injections

Another common warning is the one below:

> \[Vue warn\]: Injection "repositoryFactory" not found

The solution is stubbing this injection. In detail, you need to provide the injected data, services and so on: That means you need to mock the return values for your test's methods used, e.g. `create` and `search` - Providing the data exactly as needed by the component for running your test case and what the injection actually is. It can be valid to set the injection name equal to an empty object just to mute it when it's not really needed for your test.

```javascript
provide: {
  repositoryFactory: {}
}
```

Please note that the repository has a `create` method which creates the repository itself with all its functionalities and then there is another `create` for actually creating an entity. So in order to stub the `repositoryfactory` in realistic manner, follow the example below:

```javascript
repositoryFactory: {
    create: () => ({
        create: () => {
            return Promise.resolve();
        },
        get: () => {
            return Promise.resolve();
        },
        search: () => {
            return Promise.resolve();
        }
    })
}
```

In the context of injections, the next warning can occur sometimes:

> \[Vue warn\]: Error in foo: "TypeError: Cannot read property 'create' of undefined"

This can be caused by several reasons. The best way to find out the solution is to look at the source of the code and find out what is missing. This is highly dependent on the component under test, though. In this example the service `repositoryFactory` is missing:

```javascript
Shopware.Service('repositoryFactory').create('product');
```

To fix this you need to add the mocked service before all tests. In our case we need to register the service:

```javascript
beforeAll(() => {
  Shopware.Service.register('repositoryFactory', {
    // your service mock
  });
});
```

### Mocking repository requests

The warning with `repositoryFactory` from the previous example might appear quite often in your tests. The repository is being used to fetch items from the shopware database.

In order to provide actual test data for your test you can mock the different methods of the `repositoryFactory` and return any fake data which is relevant to test you component. E.g. an array of dummy product data.

The first thing you need to do is to mock the initialization of the repository itself like this:

```javascript
provide: {
    repositoryFactory: {
        // The "create" is mocking the repository initialization
        create: () => ({
            // Mock repository methods here...
        })
    }
}
```

This is equivalent to a computed prop like this:

```javascript
productRepository() {
    return this.repositoryFactory.create('product');
}
```

Now let's say you want to test the repository request which fetches the product list. You can mock the search method of the created repository and return an array of dummy data. By returning a Promise which immediately resolves you can test the .then\(\) branch of the actual implementation.

```javascript
provide: {
    repositoryFactory: {
        create: () => ({ 
            search: () => ({
              return Promise.resolve([{
                  name: 'Foo bar', 
                  manufacturer: 'Test' 
              }, {
                  name: 'Lorem ipsum', 
                  manufacturer: 'Jest party' 
              }])
            })
        }) 
    }
}
```

This is the equivalent to:

```javascript
this.productRepository.search(new Criteria(), Shopware.Context.api).then((result) => {
    ...
});
```

The same applies to the get method which returns a single entity based on the uuid.

```javascript
provide: {
    repositoryFactory: {
        create: () => ({
            search: () => {
                return Promise.resolve([]);
            },
            get: () => {
                return Promise.resolve({
                    name: 'product1',
                    manufacturer: 'Blaa blubber bla'
                });
            }
        })
    }
}
```

This is the equivalent to the following:

```javascript
this.productRepository.get('uuid1234', new Criteria(), Shopware.Context.api).then((result) => {
    ...
});
```

You may come across situations where multiple different repositories are being created in the component you want to test. Every repository call will be mocked by your `provide` test implementation. To differentiate between different repositories you can receive the entity name which gets past as a parameter of the `create` method from the `repositoryFactory`. Then you are able to return different test data for different repositories:

```javascript
provide: {
    repositoryFactory: {
        create: (entityName) => {
            if (entityName === 'product') {
                // Methods and test data for product...
                return {
                    search: () => Promise.resolve(),
                    get: () => Promise.resolve()
                };
            }

            if (entityName === 'language') {
                // Methods and test data for language...
                return {
                    search: () => Promise.resolve(),
                    get: () => Promise.resolve()
                };
            }

            return {};
        }
    }
}
```

## Next steps

Do you want to see these examples in practise? Head over to our [video tutorial](https://youtu.be/nWUBK3fjwVg) on how
to write component tests in jest for the Shopware administration. 

Furthermore, you might want to have a look at one of the following guides as well:
* [Jest tests for the storefront](./jest-storefront.md)
* [PHPUnit tests](./php-unit.md)
* [End-to-end tests](./end-to-end-testing.md)