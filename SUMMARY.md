# Table of contents

* [Home](README.md)

## Concepts

* [Commerce](concepts/commerce/README.md)
  * [Catalog](concepts/commerce/catalog/README.md)
    * [Categories](concepts/commerce/catalog/categories.md)
    * [Products](concepts/commerce/catalog/products.md)
    * [Sales Channels](concepts/commerce/catalog/sales-channels.md)
  * [Checkout](concepts/commerce/checkout-concept/README.md)
    * [Cart](concepts/commerce/checkout-concept/cart.md)
    * [Orders](concepts/commerce/checkout-concept/orders.md)
  * [Content](concepts/commerce/core/README.md)
    * [Shopping Experiences \(CMS\)](concepts/commerce/core/shopping-experiences-cms.md)
* [Framework](concepts/framework/README.md)
  * [Architecture](concepts/framework/architecture/README.md)
    * [Storefront](concepts/framework/architecture/storefront-concept.md)
    * [Administration](concepts/framework/architecture/administration-concept.md)
  * [Data Abstraction Layer](concepts/framework/data-abstraction-layer/README.md)
  * [Messaging](concepts/framework/messaging.md)
  * [Migrations](concepts/framework/migrations.md)
  * [Rules](concepts/framework/rules.md)
  * [HTTP Cache](concepts/framework/http_cache.md)
* [Extensions](concepts/extensions/README.md)
  * [Apps](concepts/extensions/apps-concept.md)
  * [Plugins](concepts/extensions/plugins-concept.md)
* [API](concepts/api/README.md)
  * [Store API](concepts/api/store-api.md)

## Products

* [Editions](products/editions/README.md)
  * [Community Edition](products/editions/community-edition.md)
  * [Professional Edition](products/editions/professional-edition.md)
  * [Enterprise Edition](products/editions/enterprise-edition/README.md)
    * [B2B Suite](products/editions/enterprise-edition/b2b-suite.md)
    * [Search](products/editions/enterprise-edition/search/README.md)
      * [Installation](products/editions/enterprise-edition/search/installation.md)
      * [Field Configuration](products/editions/enterprise-edition/search/field-config.md)
      * [Extensibility](products/editions/enterprise-edition/search/extensibility.md)
      * [Boosting](products/editions/enterprise-edition/search/boosting.md)
      * [Completion](products/editions/enterprise-edition/search/completion.md)
      * [Relevance](products/editions/enterprise-edition/search/relevance.md)
      * [Synonyms](products/editions/enterprise-edition/search/synonyms.md)
* [Plugins](products/plugins/README.md)
  * [Migration Assistant](products/plugins/migration-assistant/README.md)
    * [Concept](products/plugins/migration-assistant/concept/README.md)
      * [Profile and Connection](products/plugins/migration-assistant/concept/profile-and-connection.md)
      * [DataSelection and DataSet](products/plugins/migration-assistant/concept/dataselection-and-dataset.md)
      * [Migration Context](products/plugins/migration-assistant/concept/migration-context.md)
      * [Premapping](products/plugins/migration-assistant/concept/premapping.md)
      * [Gateway and Reader](products/plugins/migration-assistant/concept/gateway-and-reader.md)
      * [Convert and Mapping](products/plugins/migration-assistant/concept/convert-and-mapping.md)
      * [Logging](products/plugins/migration-assistant/concept/logging.md)
      * [Writer](products/plugins/migration-assistant/concept/writer.md)
      * [Media Processing](products/plugins/migration-assistant/concept/media-processing.md)
    * [Guides](products/plugins/migration-assistant/guides/README.md)
      * [Extending a Shopware migration profile](products/plugins/migration-assistant/guides/extending-a-shopware-migration-profile.md)
      * [Extending the Migration Connector](products/plugins/migration-assistant/guides/extending-the-migration-connector.md)
      * [Decorating a Shopware Migration Assistant converter](products/plugins/migration-assistant/guides/decorating-a-shopware-migration-assistant-converter.md)
      * [Creating a new migration profile](products/plugins/migration-assistant/guides/creating-a-new-migration-profile.md)
* [Cloud](products/cloud-1.md)
* [PWA](products/pwa-1/README.md)

## Guides

* [Installation](guides/installation/README.md)
  * [Installation overview](guides/installation/overview.md)
  * [Dockware](guides/installation/dockware.md)
  * [Docker](guides/installation/docker.md)
  * [Installation from scratch](guides/installation/from-scratch.md)
  * [Valet+](guides/installation/valet.md)
  * [Vagrant](guides/installation/vagrant.md)
  * [MAMP](guides/installation/mamp.md)
* [Extensions](guides/plugins/README.md)
  * [Overview](guides/plugins/overview.md)
  * [Plugins](guides/plugins/plugins/README.md)
    * [Plugin Base Guide](guides/plugins/plugins/plugin-base-guide.md)
    * [Plugins for Symfony developers](guides/plugins/plugins/plugins-for-symfony-developers.md)
    * [Plugin fundamentals](guides/plugins/plugins/plugin-fundamentals/README.md)
      * [Add plugin configuration](guides/plugins/plugins/plugin-fundamentals/add-plugin-configuration.md)
      * [Use plugin configuration](guides/plugins/plugins/plugin-fundamentals/use-plugin-configuration.md)
      * [Database migrations](guides/plugins/plugins/plugin-fundamentals/database-migrations.md)
      * [Dependency injection](guides/plugins/plugins/plugin-fundamentals/dependency-injection.md)
      * [Listening to events](guides/plugins/plugins/plugin-fundamentals/listening-to-events.md)
      * [Add custom service](guides/plugins/plugins/plugin-fundamentals/add-custom-service.md)
      * [Adjusting a service](guides/plugins/plugins/plugin-fundamentals/adjusting-service.md)
      * [Add plugin dependencies](guides/plugins/plugins/plugin-fundamentals/add-plugin-dependencies.md)
      * [Add custom CLI commands](guides/plugins/plugins/plugin-fundamentals/add-custom-commands.md)
      * [Add scheduled task](guides/plugins/plugins/plugin-fundamentals/add-scheduled-task.md)
      * [Using custom fields of type media](guides/plugins/plugins/framework/custom-field/custom-fields-of-type-media.md)
      * [Adding NPM dependencies](guides/plugins/plugins/plugin-fundamentals/using-npm-dependencies.md)
      * [Adding Composer dependencies](guides/plugins/plugins/plugin-fundamentals/using-composer-dependencies.md)
      * [Plugin lifecycle methods](guides/plugins/plugins/plugin-fundamentals/plugin-lifecycle.md)
    * [Checkout](guides/plugins/plugins/checkout/README.md)
      * [Cart](guides/plugins/plugins/checkout/cart/README.md)
        * [Add cart items](guides/plugins/plugins/checkout/cart/add-cart-items.md)
        * [Add cart discounts](guides/plugins/plugins/checkout/cart/add-cart-discounts.md)
        * [Add cart validator](guides/plugins/plugins/checkout/cart/add-cart-validator.md)
        * [Change price of items in cart](guides/plugins/plugins/checkout/cart/change-price-of-item.md)
        * [Add cart validator](guides/plugins/plugins/checkout/cart/add-cart-validator.md)
      * [Document](guides/plugins/plugins/checkout/document/README.md)
        * [Add custom document](guides/plugins/plugins/checkout/document/add-custom-document.md)
        * [Add custom document type](guides/plugins/plugins/checkout/document/add-custom-document-type.md)
      * [Order](guides/plugins/plugins/checkout/order/README.md)
        * [Using the state machine](guides/plugins/plugins/checkout/order/using-the-state-machine.md)
        * [Listen to order changes](guides/plugins/plugins/checkout/order/listen-to-order-changes.md)
      * [Payment](guides/plugins/plugins/checkout/payment/README.md)
        * [Add payment plugin](guides/plugins/plugins/checkout/payment/add-payment-plugin.md)
        * [Customize payment provider](guides/plugins/plugins/checkout/payment/customize-payment-provider.md)
    * [Content](guides/plugins/plugins/content/README.md)
      * [CMS](guides/plugins/plugins/content/cms/README.md)
        * [Add CMS block](guides/plugins/plugins/content/cms/add-cms-block.md)
        * [Add CMS element](guides/plugins/plugins/content/cms/add-cms-element.md)
        * [Add data to CMS element](guides/plugins/plugins/content/cms/add-data-to-cms-elements.md)
      * [Mail](guides/plugins/plugins/content/mail/README.md)
          * [Add data to mails](guides/plugins/plugins/content/mail/add-data-to-mails.md)
          * [Add mail templates](guides/plugins/plugins/content/mail/add-mail-template.md)
    * [Framework](guides/plugins/plugins/framework/README.md)
      * [Data Handling / DataAbstractionLayer](guides/plugins/plugins/framework/data-handling/README.md)
        * [Reading data](guides/plugins/plugins/framework/data-handling/reading-data.md)
        * [Writing data](guides/plugins/plugins/framework/data-handling/writing-data.md)
        * [Adding custom complex data](guides/plugins/plugins/framework/data-handling/add-custom-complex-data.md)
        * [Adding complex data to existing entities](guides/plugins/plugins/framework/data-handling/add-complex-data-to-existing-entities.md)
        * [Replacing associated data](guides/plugins/plugins/framework/data-handling/replacing-associated-data.md)
        * [Removing associated data](guides/plugins/plugins/framework/data-handling/deleting-associated-data.md)
        * [Adding data associations](guides/plugins/plugins/framework/data-handling/add-data-associations.md)
        * [Adding data translations](guides/plugins/plugins/framework/data-handling/add-data-translations.md)
        * [Using database events](guides/plugins/plugins/framework/data-handling/using-database-events.md)
        * [Using flags](guides/plugins/plugins/framework/data-handling/using-flags.md)
        * [Field inheritance](guides/plugins/plugins/framework/data-handling/field-inheritance.md)
        * [Using database events](guides/plugins/plugins/framework/data-handling/using-database-events.md)
        * [Versioning entities](guides/plugins/plugins/framework/data-handling/versioning-entities.md)
      * [Custom Fields](guides/plugins/plugins/framework/custom-field/README.md)
        * [Add custom field](guides/plugins/plugins/framework/custom-field/add-custom-field.md)
        * [Fetching data from "entity selection" custom field](guides/plugins/plugins/framework/custom-field/fetching-data-from-entity-selection.md)
      * [Event](guides/plugins/plugins/framework/event/README.md)
        * [Add custom event](guides/plugins/plugins/framework/event/add-custom-event.md)
      * [Message Queue](guides/plugins/plugins/framework/message-queue/README.md)
        * [Add message to queue](guides/plugins/plugins/framework/message-queue/add-message-to-queue.md)
        * [Add message handler](guides/plugins/plugins/framework/message-queue/add-message-handler.md)
        * [Add middleware](guides/plugins/plugins/framework/message-queue/add-middleware.md)
      * [Rule](guides/plugins/plugins/framework/rule/README.md)
        * [Add custom rules](guides/plugins/plugins/framework/rule/add-custom-rules.md)
      * [Store API](guides/plugins/plugins/framework/store-api/README.md)
        * [Add store API route](guides/plugins/plugins/framework/store-api/add-store-api-route.md)
        * [Override existing route](guides/plugins/plugins/framework/store-api/override-existing-route.md)
    * [Administration](guides/plugins/plugins/administration/README.md)
      * [Add custom module](guides/plugins/plugins/administration/add-custom-module.md)
      * [Add custom component](guides/plugins/plugins/administration/add-custom-component.md)
      * [Add custom route](guides/plugins/plugins/administration/add-custom-route.md)
      * [Add menu entry](guides/plugins/plugins/administration/add-menu-entry.md)
      * [Writing templates](guides/plugins/plugins/administration/writing-templates.md)
      * [Adding snippets](guides/plugins/plugins/administration/adding-snippets.md)
      * [Adding Mixins](guides/plugins/plugins/administration/add-mixins.md)
      * [Using Mixins](guides/plugins/plugins/administration/using-mixins.md)
      * [Adding Services](guides/plugins/plugins/administration/add-custom-service.md)
      * [Adding permissions](guides/plugins/plugins/administration/add-acl-rules.md)
      * [Using the data handling](guides/plugins/plugins/administration/using-data-handling.md)
      * [Customize modules](guides/plugins/plugins/administration/customizing-modules.md)
      * [Override existing routes](guides/plugins/plugins/administration/overriding-routes.md)
      * [Add tab to existing module](guides/plugins/plugins/administration/add-new-tab.md)
      * [Customizing components](guides/plugins/plugins/administration/customizing-components.md)
      * [Using assets](guides/plugins/plugins/administration/using-assets.md)
      * [Using Directives](guides/plugins/plugins/administration/adding-directives.md)
      * [Add tab to existing module](guides/plugins/plugins/administration/add-new-tab.md)
      * [Add custom input field to existing component](guides/plugins/plugins/administration/add-custom-field.md)
      * [Using base components](guides/plugins/plugins/administration/using-base-components.md)
      * [Add custom styles](guides/plugins/plugins/administration/add-custom-styles.md)
      * [Using the data grid component](guides/plugins/plugins/administration/using-the-data-grid-component.md)
      * [Extending Webpack](guides/plugins/plugins/administration/extending-webpack.md)
      * [Using Utility functions](guides/plugins/plugins/administration/using-utils.md)
      * [The Shopware object](guides/plugins/plugins/administration/the-shopware-object.md)
      * [Add filter](guides/plugins/plugins/administration/add-filter.md)
      * [Using filter](guides/plugins/plugins/administration/using-filter.md)
      * [Using custom fields](guides/plugins/plugins/administration/using-custom-fields.md)
      * [Add custom data to the search](guides/plugins/plugins/administration/search-custom-data.md)
    * [Storefront](guides/plugins/plugins/storefront/README.md)
      * [Customize templates](guides/plugins/plugins/storefront/customize-templates.md)
      * [Add custom controller](guides/plugins/plugins/storefront/add-custom-controller.md)
      * [Add custom Javascript](guides/plugins/plugins/storefront/add-custom-javascript.md)
      * [Override existing Javascript](guides/plugins/plugins/storefront/override-existing-javascript.md)
      * [Add custom assets](guides/plugins/plugins/storefront/add-custom-assets.md)
      * [Add custom styling](guides/plugins/plugins/storefront/add-custom-styling.md)
      * [Add custom icons](guides/plugins/plugins/storefront/add-icons.md)
      * [Add custom page](guides/plugins/plugins/storefront/add-custom-page.md)
      * [Add custom pagelet](guides/plugins/plugins/storefront/add-custom-pagelet.md)
      * [Add translations](guides/plugins/plugins/storefront/add-translations.md)
      * [Fetching data with Javascript](guides/plugins/plugins/storefront/fetching-data-with-javascript.md)
      * [Add data to storefront page](guides/plugins/plugins/storefront/add-data-to-storefront-page.md)
      * [Add cookie to manager](guides/plugins/plugins/storefront/add-cookie-to-manager.md)
      * [Reacting to cookie consent changes](guides/plugins/plugins/storefront/reacting-to-cookie-consent-changes.md)
      * [Reacting to javascript events](guides/plugins/plugins/storefront/reacting-to-javascript-events.md)
      * [Use CSRF protection](guides/plugins/plugins/storefront/use-csrf-protection.md)
      * [Working with media and thumbnails](guides/plugins/plugins/storefront/use-media-thumbnails.md)
      * [Remove Javascript plugin](guides/plugins/plugins/storefront/remove-unnecessary-js-plugin.md)
      * [Add custom field in the storefront](guides/plugins/plugins/storefront/using-custom-fields-storefront.md)
      * [Add custom sorting for product listing](guides/plugins/plugins/storefront/add-custom-sorting-product-listing.md)
    * [Testing](guides/plugins/plugins/testing/README.md)
      * [End-to-end testing](guides/plugins/plugins/testing/end-to-end-testing.md)
      * [Jest unit tests in Shopware's administration](guides/plugins/plugins/testing/jest-admin.md)
      * [Jest unit tests in Shopware's storefront](guides/plugins/plugins/testing/jest-storefront.md)
      * [PHP unit testing](guides/plugins/plugins/testing/php-unit.md)
  * [Themes](guides/plugins/themes/README.md)
    * [Theme base guide](guides/plugins/themes/theme-base-guide.md)
    * [Create a first theme](guides/plugins/themes/create-a-theme.md)
    * [Differences Plugins and Apps vs Themes](guides/plugins/themes/differences-plugins-and-apps-vs-themes.md)
    * [Theme configuration](guides/plugins/themes/theme-configuration.md)
    * [Add SCSS Styling and JavaScript to a Theme](guides/plugins/themes/add-css-js-to-theme.md)
    * [Override Bootstrap variables in a Theme](guides/plugins/themes/override-bootstrap-variables-in-a-theme.md)
    * [Add assets to a Theme](guides/plugins/themes/add-assets-to-theme.md)
    * [Add custom icons](guides/plugins/plugins/storefront/add-icons.md)
    * [Theme inheritance](guides/plugins/themes/add-theme-inheritance.md)
    * [Theme with Bootstrap styling](guides/plugins/themes/add-theme-inheritance-without-resources.md)
  * [Apps](guides/plugins/apps/README.md)
    * [App Base Guide](guides/plugins/apps/app-base-guide.md)
    * [Storefront](guides/plugins/apps/storefront.md)
    * [Administration](guides/plugins/apps/administration/README.md)
      * [Add custom action button](guides/plugins/apps/administration/add-custom-action-button.md)
      * [Add custom module](guides/plugins/apps/administration/add-custom-module.md)
    * [Custom Data](guides/plugins/apps/custom-data.md)
    * [Configuration](guides/plugins/apps/configuration.md)
* [Hosting](guides/hosting/README.md)
  * [Installation & Updates](guides/hosting/installation-updates/README.md)
    * [Deployments](guides/hosting/installation-updates/deployments/README.md)
        * [Deployment with Deployer](guides/hosting/installation-updates/deployments/deployment-with-deployer.md)
    * [Versioning & Dependencies](guides/hosting/installation-updates/composer.md)
  * [Performance](guides/hosting/performance/README.md)
    * [HTTP Cache](guides/hosting/performance/caches.md)
  * [Infrastructure](guides/hosting/infrastructure/README.md)
    * [Filesystem](guides/hosting/infrastructure/filesystem.md)
    * [Message Queue](guides/hosting/infrastructure/message-queue.md)
    * [Reverse Http Cache](guides/hosting/infrastructure/reverse-http-cache.md)
    * [Elasticsearch](guides/hosting/infrastructure/elasticsearch.md)
* [Integrations / API](guides/integrations-api/README.md)
  * [General Concepts](guides/integrations-api/general-concepts/README.md)
    * [Search Criteria](guides/integrations-api/general-concepts/search-criteria.md)
    * [Request Headers](guides/integrations-api/general-concepts/request-headers.md)
    * [Generated Reference](guides/integrations-api/general-concepts/generated-reference.md)
  * [Admin API Guide](guides/integrations-api/admin-api/README.md)
    * [Authentication](guides/integrations-api/admin-api/authentication.md)
    * [Reading entities](guides/integrations-api/admin-api/reading-entities.md)
    * [Writing entities](guides/integrations-api/admin-api/writing-entities/README.md)
      * [Associations](guides/integrations-api/admin-api/writing-entities/associations.md)
      * [Bulk Payloads](guides/integrations-api/admin-api/writing-entities/bulk-payloads.md)
      * [Product Data](guides/integrations-api/admin-api/writing-entities/product-data.md)
    * [Action Routes](guides/integrations-api/admin-api/action-routes.md)
  * [Store API Guide](guides/integrations-api/store-api-guide/README.md)
    * [Registering a customer](guides/integrations-api/store-api-guide/register-a-customer.md)
    * [Searching for products](guides/integrations-api/store-api-guide/search-for-products.md)
    * [Working with the cart](guides/integrations-api/store-api-guide/work-with-the-cart.md)
    * [Handling the payment](guides/integrations-api/store-api-guide/handling-the-payment.md)
    * [FAQ](guides/integrations-api/store-api-guide/faq.md)

## Resources

* [References](resources/references/README.md)
  * [API Reference](resources/references/api-reference/README.md)
    * [Store API Reference](resources/references/api-reference/store-api-reference/README.md)
        * [Product](resources/references/api-reference/store-api-reference/product.md)
        * [Category](resources/references/api-reference/store-api-reference/category.md)
    * [Admin API Reference](resources/references/api-reference/admin-api-reference.md)
  * [Core Reference](resources/references/core-reference/README.md)
    * [DAL Reference](resources/references/core-reference/dal-reference/README.md)
      * [Fields Reference](resources/references/core-reference/dal-reference/fields-reference.md)
      * [Flags Reference](resources/references/core-reference/dal-reference/flags-reference.md)
      * [Filters Reference](resources/references/core-reference/dal-reference/filters-reference.md)
      * [Aggregations Reference](resources/references/core-reference/dal-reference/aggregations-reference.md)
    * [Administration Reference](./resources/references/administration-reference/README.md)
      * [Utils](./resources/references/administration-reference/utils.md))
      * [Mixins](./resources/references/core-reference/administration-reference/mixins.md)
    * [Commands Reference](resources/references/core-reference/commands-reference.md)
    * [Rules Reference](resources/references/core-reference/rules-reference.md)
  * [Config Reference](resources/references/config-reference/README.md)
    * [Server](resources/references/config-reference/server/README.md)
      * [Apache](resources/references/config-reference/server/apache.md)
      * [Nginx](resources/references/config-reference/server/nginx.md)
  * [App Reference](resources/references/app-reference/README.md)
    * [Manifest Reference](resources/references/app-reference/manifest-reference.md)
  * [Administration Reference](resources/references/administration-reference/README.md)
      * [Utils](resources/references/administration-reference/utils.md)
  * [Storefront Reference](resources/references/storefront-reference/README.md)
    * [Shopware's twig functions](resources/references/storefront-reference/twig-function-reference.md)
  * [Testing Reference](resources/references/testing-reference)
    + [Custom E2E Commands](resources/references/testing-reference/e2e-custom-commands.md)
    + [PSH E2E Commands](resources/references/testing-reference/e2e-psh-commands.md)
* [Guidelines](resources/guidelines/README.md)
  * [Code](resources/guidelines/code.md)
    * [Contribution Guidelines](resources/guidelines/code/contribution.md) 
    * [Cart Process](resources/guidelines/code/cart-process.md) 
    * [Context Rules &amp; Rule Systems](resources/guidelines/code/context-rules-rule-systems.md) 
    * [Dependency Injection &amp; Dependency Handling](resources/guidelines/code/dependency-injection-dependency-handling.md) 
    * [Document Code](resources/guidelines/code/document-code.md)
    * [Events](resources/guidelines/code/events.md) 
    * [Page Loader](resources/guidelines/code/pageloader.md) 
    * [Platform Domains](resources/guidelines/code/platform-domains.md)
    * [Public APIs](resources/guidelines/code/public-apis.md) 
    * [Routing](resources/guidelines/code/routing.md) 
    * [Session and State](resources/guidelines/code/session-and-state.md) 
    * [Store API](resources/guidelines/code/store-api.md) 
    * [Storefront Controller](resources/guidelines/code/storefront-controller.md) 
  * [Documentation](resources/guidelines/documentation/README.md)
    * [Structure](resources/guidelines/documentation/structure.md)
    * [Writing](resources/guidelines/documentation/writing.md)
  * [Testing](resources/guidelines/testing/README.md)
    * [Best practises on writing end-to-end tests](resources/guidelines/testing/e2e-best-practises.md)
