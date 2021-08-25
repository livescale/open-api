<!-- PROJECT LOGO -->
<br />
<p align="left">
  <img src="https://www.livescale.tv/wp-content/uploads/2020/11/Livescale-Horizontal-Logo-1.png" alt="Livescale Logo">
</p>

<!-- GETTING STARTED -->
## Getting Started
This repository holds the official Livescale Shopping OpenAPI specification.

Before you start, please consult the [Standard Flow](BASIC_FLOW.md) description.

Livescale is a platform that enables Live shopping for Merchants by connecting their e-commerce to our live shopping solution. This repository is for Merchants that operate on an e-commerce platform that have custom needs.

With this OpenAPI, the Merchant can choose to integrate fully with our seamless integrated checkout (i.e. viewer doesn't leave the live shopping experience) or can choose for the viewer to checkout on the Merchant or Retailer's website.

This OpenAPI specification allows you to create a server that will be able to interact with Livescale's live shopping App.

OpenAPI spec 3.0 In Release

* [Shopping API](https://github.com/livescale/open-api/blob/main/livescale-shopping-api.yml)

## Code Generators

You can use this [OpenAPI generator](https://editor.swagger.io/) along with this OAS yaml file to generate a server. 

<!-- USAGE EXAMPLES -->
## Usage

This is an example of the **POST /baskets/:basket_id/items** endpoint that use the SFCC© Commerce SDK.

```javascript

const { ClientConfig, Checkout } = require('commerce-sdk');
const config = new ClientConfig();
config.headers = {};
config.parameters = {
  clientId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx',
  organizationId: 'f_ecom_xxxx_xxx',
  shortCode: 'xxxxxxxx',
  siteId: 'livescale',
};


server.post('/baskets/:basket_id/items',
  restify.plugins.conditionalHandler([{
    version: ['1.0.0'], handler: async (req, res, next) => {
      config.headers.authorization = authorization;
      const shopperBasketsClient = new Checkout.ShopperBaskets(config);

      try {

        // Convert input for your ecommerce
        const products = converter.productsConverter(body);

        // Interact with your ecommerce
        const basket = await shopperBasketsClient.addItemToBasket({
          parameters: { basketId: basket_id },
          body: products,
        });

        // Convert your ecommerce response to Livescale Basket Object
        const convertedBasket = converter.basketConverter(basket);

        res.send(200, convertedBasket);
        return next();
      } catch (error) {
        const convertedError = converter.errorConverter(error);
        res.send(convertedError.statusCode, convertedError.message);
        return next();
      }
  }])
);
```

The example above show that there is three major steps in every endpoint of this API. 

1. Converting Livescale input into your e-commerce input format
2. Passing the converted input into your e-commerce
3. Converting your e-commerce response into the Livescale output format

<!-- ROADMAP -->

<!-- LICENSE -->
## License

Distributed under the Creative Commons Zero v1.0 Universal License. See [License](LICENSE) for more information.

