# Instructions for incremental development

Each feature below is providing its test and implementation code.

- Consumer tests should be added in the `productsService.test.ts` file.
- Consumer Implementations have to be added to the `productsAPIClient.ts` file.
- Provider tetst should be added to `productsProvider.test.js` file in the `stateHandlers` object.
- Provider implementations should be added to the `provider.js` file.

## Get a single product by id

### Consumer Test

```ts
describe("GET /products/{id}", () => {
  it("returns an HTTP 200 and a single product by id", () => {
    return provider()
      .addInteraction()
      .given("there are many products including a product with id 1")
      .uponReceiving("a request for a product with id 1")
      .withRequest("GET", "/products/1", (builder) => {
        builder.headers({ Accept: "application/json" });
      })
      .willRespondWith(200, (builder) => {
        builder.headers({ "Content-Type": "application/json" });
        builder.jsonBody({
          id: 1,
          name: like(productExample.name),
        });
      })
      .executeTest(async (mockserver) => {
        const productsAPIClient = new ProductsAPIClient(mockserver.url);
        const product = await productsAPIClient.getProductById(1);

        expect(product).toBeInstanceOf(Product);
        expect(product!.id).toEqual(productExample.id);
        expect(product!.name).toEqual(productExample.name);
      });
  });

  it("returns an HTTP 404 when the product does not exist", () => {
    return provider()
      .addInteraction()
      .given("there are many products but none with id 1")
      .uponReceiving("a request for a product with id 1")
      .withRequest("GET", "/products/1", (builder) => {
        builder.headers({ Accept: "application/json" });
      })
      .willRespondWith(404)
      .executeTest(async (mockserver) => {
        const productsAPIClient = new ProductsAPIClient(mockserver.url);
        const product = await productsAPIClient.getProductById(1);

        expect(product).toBeNull();
      });
  });
});
```

### Consumer Implementation

```ts
  async getProductById(id: number): Promise<Product | null> {
    try {
      const response = await axios.request({
        baseURL: this.url,
        headers: { Accept: "application/json" },
        method: "GET",
        url: `/products/${id.toString()}`,
      });

      // return the data from the response coverted to a Product
      return new Product(response.data.id, response.data.name);
    } catch (error) {
      return null;
    }
  }
```

### Provider test

```js
        "there are many products including a product with id 1": () => {
          productRepository.clearAll();
          productRepository.insert({
            id: 1,
            name: "Some product name",
            price: 30.5,
          });
          productRepository.insert({
            id: 22,
            name: "Some other name",
            price: 11.12,
          });
        },
        "there are many products but none with id 1": () => {
          productRepository.clearAll();
          productRepository.insert({
            id: 11,
            name: "Some product name",
            price: 30.5,
          });

          productRepository.insert({
            id: 22,
            name: "Some other name",
            price: 11.12,
          });
        },
```

### Provider implementation

```js
server.get("/products/:id", function (req, res) {
  const id = req.params.id;
  console.log("Fetching...", productRepository.getById(id));

  const product = productRepository.getById(id);

  if (!product) {
    res.sendStatus(404);
  } else {
    res.json(product);
  }
});
```

## Create a product

### Consumer Test

```ts
describe("POST /products", () => {
  it("retuns an HTTP 201 when a product is created", () => {
    return provider()
      .addInteraction()
      .given("the product can be created")
      .uponReceiving("a request to create a product")
      .withRequest("POST", "/products", (builder) => {
        builder.headers({ Accept: "application/json" });
        builder.jsonBody({ name: productExample.name });
      })
      .willRespondWith(201, (builder) => {
        builder.headers({ "Content-Type": "application/json" });
        builder.jsonBody({
          id: like(productExample.id),
          name: productExample.name,
        });
      })
      .executeTest(async (mockserver) => {
        const productsAPIClient = new ProductsAPIClient(mockserver.url);
        const product: Product = await productsAPIClient.createProduct({
          name: productExample.name,
        });

        expect(product.id).toBeGreaterThan(0);
        expect(product.name).toEqual(productExample.name);
      });
  });
});
```

### Consumer Implementation

```ts
  async createProduct(productToBeCreated: { name: string; }): Promise<Product> {
    const response = await axios.request({
      baseURL: this.url,
      headers: { Accept: "application/json" },
      method: "POST",
      url: "/products",
      data: { name: productToBeCreated.name },
    });
    // return the data from the response coverted to a Product
    return new Product(response.data.id, response.data.name);
  }
```

### Provider test

```js
        "the product can be created": () => {
        }
```

### Provider implementation

```js
server.post("/products", express.json(), function (req, res) {
  const product = req.body;
  console.log("Creating...", product);
  productRepository.insert(product);
  console.log("Created...", product);
  res.status(201).json(product);
});
```
