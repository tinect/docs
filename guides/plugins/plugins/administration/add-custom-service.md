# Adding services

## Overview

This guide will teach you how to add a service to the Shopware 6 Administration.

This documentation chapter will cover the following topics:

* What is an administration service?
* How to register a new administration service for your plugin

## Prerequisites

All you need for this guide is a running Shopware 6 instance and full access to both the files and a running plugin. Of course you'll have to understand JavaScript, but that's a prerequisite for Shopware as a whole and will not be taught as part of this documentation.

## Register a new service

For this example, we want to use the following service. It's supposed to get random jokes.
It is placed in `<administration root>/services/joke.service.js` and looks like the example seen below:

```javascript
export default class JokeService {
    constructor(httpClient) {
        this.httpClient = httpClient;
    }

    joke() {
        return this.httpClient
            .get('https://v2.jokeapi.dev/joke/Programming?blacklistFlags=nsfw,religious,political')
            .then(response => response.data)
    }
}
```

For now this service class is not available in the injection container.
To fix this, a new script is placed at `<administration root>/init/joke-service.init.js` and imported in the `main.js` file of our plugin:

```javascript
import JokeService from '../services/joke.service'

Shopware.Service().register('joker', (container) => {
    const initContainer = Shopware.Application.getContainer('init');
    return new JokeService(initContainer.httpClient);
});
```

## Service injection

A service is typically injected into a vue component and can simply be referenced in the `inject` property:

```javascript
Shopware.Component.register('swag-basic-example', {
    inject: ['joker'],

    created() {
        this.joker.joke().then(joke => console.log(joke))
    }
});
```

To avoid collision with other properties like computed fields or data fields there is an option to rename the service property using an object:

```javascript
Shopware.Component.register('swag-basic-example', {
    inject: {
        jokeService: 'joker'
    },

    created() {
        this.jokeService.joke().then(joke => console.log(joke))
    }
});
```

## Decorating a service

Service decoration can be us in a variety of ways.
Services can be initialized right after their creation and single methods can get an altered behavior.
Like in the service registration, a script that is part of the `main.js` is needed.

{% hint style="warning" %}
Decorators are just simple functions, which intercept a service in the provider phase.
This means that a service can only be decorated in the timeframe between it being created and it being accessed for the first time.
{% endhint %}

If you need to alter a service method return value or add an additional parameter you can also do this using decoration.
For this example a `funny` attribute is added to the requested jokes by the previously registered `JokeService`:

```javascript
Shopware.Application.addServiceProviderDecorator('joker', joker => {
    const decoratedMethod = joker.joke;

    joker.joke = function () {
        return decoratedMethod.call(joker).then(joke => ({
            ...joke,
            funny: joke.id % 2 === 0
        }))
    };

    return joker;
});
```