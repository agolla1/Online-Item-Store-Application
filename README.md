# Online-Item-Store-Application
This repository serves as the central hub for all microservices, collectively forming a large-scale application built on a microservices architecture.
# BookStore

BookStore is an online shopping platform designed for purchasing books. This application is built with the goal of exploring and learning various technologies through the development of a comprehensive, modular application that incorporates both frontend and backend components.

## Objective

The primary objective of this project is to gain hands-on experience with multiple technologies by developing an application with realistic complexity. This project will be a valuable addition to my resume, demonstrating my skills as a Computer Science and Data Science major at UW-Madison with a minor in Entrepreneurship.

## Technical Architecture

The BookStore application is composed of several key components:

### 1. bookstore-webapp
This is the customer-facing web application where users can:
- Browse the available book catalog
- Add books to their shopping cart
- Proceed to checkout and place orders

### 2. bookstore-backoffice
This is an administrative application for staff members to manage the book catalog and orders. It enables administrators to:
- Set up and maintain the book catalog
- Manage customer orders and view their statuses

### 3. Backing Services
The backend services are designed as independently deployable microservices. These services can be developed using different programming languages or frameworks based on the team’s preference. They expose REST APIs for communication with the `bookstore-webapp`, `bookstore-backoffice`, and other services. Some services also act as event processors, consuming events from an event store (e.g., Kafka, RabbitMQ), processing them, and optionally publishing new events.

### Frontend Implementation
The frontend web applications (`bookstore-webapp` and `bookstore-backoffice`) can be implemented using SPA frameworks such as Angular, Vue.js, or React.js, or using server-side rendering technologies like Thymeleaf.

## Application Flow

The typical usage flow of the application is as follows:

1. **Customer Actions**:
   - Browse the product catalog
   - Add selected books to the cart
   - Proceed to the checkout page
   - Enter customer details, delivery address, and payment information
   - Place an order

2. **Order Processing**:
   - Validate order and payment details
   - If the payment is valid, set the status to `NEW`; otherwise, set it to `ERROR`
   - Save order details in the database
   - Publish an `OrderCreated` event if the payment is valid; otherwise, publish an `OrderError` event

3. **Event Handling**:
   - `OrderService` handles `OrderCreated` and `OrderError` events, sending appropriate email notifications to the customer
   - `DeliveryService` processes `OrderCreated` events, updates the status to `READY_TO_SHIP`, and later to `DELIVERED`, publishing an `OrderDelivered` event

## Backend Services

The application consists of several domain-oriented microservices:

### 1. Catalog Service
Manages the book catalog and provides the following REST API endpoints:
   - Get books by page
   - Get book details by ISBN
   - Create, update, and delete books
   - Search books by title or description

### 2. Payment Service
Validates payment information, such as credit card number, CVV, and expiry date.

### 3. Order Service
Handles customer carts and orders with the following API endpoints:
   - **Cart APIs**: Create, add items, update quantity, remove items, or delete a cart
   - **Order APIs**: 
     - Create and save orders; publish an `ORDER_CREATED` event
     - Cancel orders and publish an `ORDER_CANCELLED` event
     - Retrieve orders by number or list all orders
   - **Event Handlers**:
     - `ORDER_DELIVERED`: Update status to `DELIVERED`
     - `ORDER_CANCELLED`: Update status to `CANCELLED`
     - `ORDER_ERROR`: Update status to `ERROR`
   - **Email Notification Handlers**:
     - Send notifications for order creation, cancellation, delivery, or errors

### 4. Delivery Service
Manages order delivery and processes events:
   - `ORDER_CREATED`: Save order with status `READY_TO_SHIP`
   - **Jobs**:
     - `OrderDeliveryJob`: Processes orders with status `READY_TO_SHIP`, marks them as delivered, and publishes `ORDER_DELIVERED` events. In case of failures, it publishes `ORDER_ERROR` events.

## Contribution Guidelines

To contribute to the development of the BookStore application:
1. Run the application locally and report any issues.
2. Review the code and provide feedback.
3. Implement a service using your preferred language or framework.

## Conclusion

The BookStore project is a comprehensive, modular application aimed at providing a hands-on learning experience in full-stack development, microservices architecture, and event-driven systems. This project will serve as a showcase of my technical skills and understanding of modern web development practices.


