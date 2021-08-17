<!-- PROJECT LOGO -->
<br />
<p align="center">
  <img src="https://www.livescale.tv/wp-content/uploads/2020/11/Livescale-Horizontal-Logo-1.png" alt="Livescale Logo">

  <h3 align="center">Livescale Shopping API</h3>

  <p align="center">
    <a href="https://github.com/livescale/open-api/issues">Report Bug</a>
    ·
    <a href="https://github.com/livescale/open-api/issues">Request Feature</a>
  </p>
</p>

<!-- ABOUT THE PROJECT -->
## About The Project

Livescale is a platform that enables Live shopping for Merchants by connecting their e-commerce to our Live shopping solution. This project is for Merchants that operate on an e-commerce platform that we do not support yet or for the ones that have very custom needs.

This API specification allows you to create a server that will be able to interact with Livescale's Live shopping App.

<!-- GETTING STARTED -->
## Getting Started

This section will guide you into the server generation and explain the scope of work for the endpoints

### Server generation

To generate easily a server from this project YAML file you can paste it in the [Swagger Editor](https://editor.swagger.io/) and then select the desired server type. 

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

As we can see in the example above, there is three major steps in every endpoint of this API. 

1. Converting Livescale input into your e-commerce input format
2. Passing the converted input into your e-commerce
3. Converting your e-commerce response into the Livescale output format

<!-- ROADMAP -->

<!-- LICENSE -->
## License

Distributed under the Creative Commons Zero v1.0 Universal License. See `LICENSE` for more information.

