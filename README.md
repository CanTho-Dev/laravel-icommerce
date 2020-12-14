<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

# iCommerce

Simple online shopping application to sell products (backend only).

- [System Design](#system-design)
  - [1. Requirements](#1-requirements)
  - [2. High-level design](#2-high-level-design)
  - [3. Defining data model](#3-defining-data-model)
    - [Product Service](#product-service)
    - [Audit Service](#audit-service)
    - [Shopping Cart Service](#shopping-cart-service)
    - [Order Service](#order-service)
  - [4. Detailed design](#4-detailed-design)
    - [Authentication Service](#authentication-service)
    - [API Gateway](#api-gateway)
    - [Registry Service](#registry-service)
    - [Product Service](#product-service)
    - [Audit Service](#audit-service)
    - [Shopping Cart Service](#shopping-cart-service)
    - [Order Service](#order-service)
- [Software development principles](#software-development-principles)
  - [KISS (Keep It Simple Stupid)](#kiss-keep-it-simple-stupid)
  - [YAGNI (You aren't gonna need it)](#yagni-you-arent-gonna-need-it)
  - [Separation of Concerns](#separation-of-concerns)
  - [DRY](#dry)
  - [Code For The Maintainer](#code-for-the-maintainer)
  - [Avoid Premature Optimization](#avoid-premature-optimization)
  - [Minimise Coupling](#minimise-coupling)
  - [Inversion of Control](#inversion-of-control)
  - [Single Responsibility Principle](#single-responsibility-principle)
- [Design Patterns](#design-patterns)
- [Application default configuration](#application-default-configuration)
- [How to run the application](#how-to-run-the-application)
  - [Setup development workspace](#setup-development-workspace)
  - [Run a microservice](#run-a-microservice)
- [API Documentation](#api-documentation)
- [Project folder structure and Frameworks, Libraries](#project-folder-structure-and-frameworks-libraries)
  - [Project folder structure](#project-folder-structure)
  - [Frameworks and Libraries](#frameworks-and-libraries)
- [References](#references)
- [Other projects](#other-projects)

## System Design

### 1. Requirements
A small start-up named "iCommerce" wants to build a very simple online shopping application to sell their products. In order to get to the market quickly, they just want to build an MVP version with a very limited set of functionalities:
a. The application is simply a simple web page that shows all products on which customers can filter, short and search for products based on different criteria such as name, price, brand, colour etc.

b. All product prices are subject to change at any time and the company wants to keep track of it.

c. If a customer finds a product that they like, they can add it to their shopping cart and proceed to place an order.

d. For audit support, all customers' activities such as searching, filtering and viewing product's details need to be stored in the database. However, failure to store customer activity is completely transparent to customer and should have no impact to the activity itself.

e. Customer can login simply by clicking the button “Login with Facebook”. No further account registration is required.

f. No online payment is supported yet. Customer is required to pay by cash when the product got delivered.

### 2. High-level design
At a high-level, we need some following services (or components) to handle above requirements:

![High Level Design](external-files/HighLevelDesign.png)

- **Product Service**: manages our products with CRUD operations. This service also provides the ability to allow user could filter, sort and search for products based on dynamic criteria.
- **Audit Service**: records all customers activities (filtering, sorting, viewing product detail).
- **Shopping Cart Service**: manages customers shopping carts with CRUD operations.
- **Order Service**: manages customer orders with CRUD operations.
- **Authentication Service**: authenticates customers, integrates with 3rd party identity platform like Facebook, Google...
- **API Gateway**: Route requests to multiple services using a single endpoint. This service allows us to expose multiple services on a single endpoint and route to the appropriate service based on the request.

### 3. Defining data model
In this part, we describes considerations for managing data in our architecture. For each service, we discuss data schema and datastore considerations.

In general, we follow the basic principle of microservices is that each service manages its own data. Two services should not share a data store.

![Wrong Datastore](external-files/datastore-microservices-wrong.png)
#### Product Service
The Product service stores information about all of our product. The storage requirements for the Product are:
- Long-term storage.
- Read-heavy (it's common for ecommerce application because the traffic from users to view, search, sort product are always much higher than the traffic from administrators to update product's information).
- Structured data: Category, Product, Brand, ProductPriceHistory. 
- Need complex joins (for example, in the UI, maybe we have a menu with different categories, user could choose a category and then view all products in that category, then each product will have many different variants like a T-shirt will have many different sizes and colors).

A relational database is appropriate in our case. For the simplicity of the assignment, we simplify the data schema like this:

![Product Schema](external-files/Product.png)
