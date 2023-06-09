# E-Commerce APIs

## How to Run Locally

### Run the server

```bash
npm start
```

### Run the server (dev)

```bash
npm run dev
```

## APIs

> **IMPORTANT**: **--->** Please use `Prod` environment in Postman to test the APIs. **<---**

> **POSTMAN LINK**: https://elements.getpostman.com/redirect?entityId=17581857-b9082ff4-ddb9-462f-a3af-e90ba1a35b3e&entityType=collection

### Public

- Register a new user [POST `/register`]

  ```json
  {
    "name": "John Doe",
    "email": "john.doe@foo.com",
    "password": "password"
  }
  ```

- Login [POST `/login`]

  ```json
  {
    "email": "john.doe@foo.com",
    "password": "password"
  }
  ```

- Logout [POST `/logout`]

### User [Authenticated (Accessible after Login) `/user`]

#### Product

- Create a new product [POST `/product/create`]

  ```json
  {
    "name": "H&M T-Shirt",
    "description": "T-Shirt Cotton",
    "price": 200,
    "userId": 1,
    "variant": [
      {
        "name": "Green",
        "sku": "AB123",
        "additionalCost": 0,
        "stock": 2
      },
      {
        "name": "Red",
        "sku": "AB124",
        "additionalCost": 10,
        "stock": 1
      }
    ]
  }
  ```

- Get all products [GET `/product/all`]

  Query Parameters:

  - `name` - Search by name
  - `description` - Search by description
  - 'variationName' - Search by variation name

- Update a product [PUT `/product/update/:id`]

  ```json
  {
    "description": "T-Shirt Cotton 100%"
  }
  ```

- Delete a product [DELETE `/product/delete/:id`]

#### Order

- Create a new order [POST `/order/create`]

  ```json
  {
    "status": "order placed",
    "totalCost": 500, // could be calculated on the frontend!
    "customerId": 1,
    "products": [6]
  }
  ```

- Get all order by customer [GET `/order/all/:id`]

## How to Run in Production

- API Documentation is hosted on
  Postman (https://www.postman.com/shaikh078/workspace/public/folder/17581857-e1e6a017-2cca-459c-9f55-b9c96e197bff?ctx=documentation)

## Technologies Used

- Node.js
- Express.js
- JWT Authentication
- PostgreSQL
- Prisma
- Railway
- Postman

## Database Schema

```sql
   +--------------+
   |     User     |
   +--------------+
   | id: Int      |
   | name: String |
   | email: String|
   | password: String |
   | createdAt: DateTime |
   | updatedAt: DateTime |
   +--------------+
         |
         |
   +--------------+
   |   Product    |
   +--------------+
   | id: Int      |
   | name: String |
   | description: String |
   | price: Int   |
   | authorId: Int (User.id) |
   | createdAt: DateTime |
   | updatedAt: DateTime |
   +--------------+
         |
         |
   +--------------+
   |   Variant    |
   +--------------+
   | id: Int      |
   | name: String |
   | sku: String  |
   | additionalCost: Int |
   | stock: Int   |
   | productId: Int (Product.id) |
   | createdAt: DateTime |
   | updatedAt: DateTime |
   +--------------+
         |
         |
   +--------------+
   |    Order     |
   +--------------+
   | id: Int      |
   | status: String |
   | totalCost: Int |
   | customerId: Int (User.id) |
   | createdAt: DateTime |
   | updatedAt: DateTime |
   +--------------+
         |
         |
   +-------------------+
   |    OrderProduct   |
   +-------------------+
   | orderId: Int (Order.id) |
   | variantId: Int (Variant.id) |
   +-------------------+
```

### The relationships between the tables are as follows:

- User has a one-to-many relationship with Product and Order. The authorId field in the Product table references the id
  field in the User table, representing the author of the product. The customerId field in the Order table references
  the id field in the User table, representing the customer who placed the order.

- Product has a many-to-one relationship with User. The authorId field in the Product table references the id field in
  the User table, indicating the author of the product.

- Product has a one-to-many relationship with Variant. The productId field in the Variant table references the id field
  in the Product table, associating a variant with its corresponding product.

- Variant has a many-to-one relationship with Product. The productId field in the Variant table references the id field
  in the Product table, indicating the product to which the variant belongs.

- Order has a many-to-one relationship with User. The customerId field in the Order table references the id field in the
  User table, representing the customer who placed the order.

- Order has a many-to-many relationship with Variant through the OrderProduct table. The orderId field in the
  OrderProduct table references the id field in the Order table, and the variantId field in the OrderProduct table
  references the id field in the Variant table. This association allows multiple variants to be associated with a single
  order, and vice versa.
