# POS REST API - Spring Boot

A complete REST API for managing a Point of Sale (POS) system with order management, inventory tracking, and sales reporting capabilities.

## 📋 Features

- ✅ **Product Management** - Full CRUD operations for products with inventory control
- ✅ **Order Management** - Create, update, complete, and cancel orders
- ✅ **Inventory Sync** - Auto-reduce inventory on order creation, auto-restore on cancellation
- ✅ **Sales Reports** - Generate daily sales analytics with total sales, order count, and average order value
- ✅ **Validation** - Input validation using Jakarta annotations
- ✅ **Exception Handling** - Global exception handler with meaningful HTTP responses
- ✅ **CORS Enabled** - Ready for frontend integration
- ✅ **Database** - MySQL with Hibernate ORM and auto-schema generation

## 🛠️ Technologies

- **Java 17**
- **Spring Boot 3.2.0**
- **Spring Data JPA**
- **MySQL 8.0**
- **Maven**
- **Lombok**
- **Jakarta Validation**

## 📁 Project Structure

```
src/main/java/com/pos/
├── PosRestApiApplication.java          # Main Spring Boot application
├── controller/
│   ├── ProductController.java
│   ├── OrderController.java
│   └── SalesReportController.java
├── service/
│   ├── ProductService.java
│   ├── OrderService.java
│   └── SalesReportService.java
├── repository/
│   ├── ProductRepository.java
│   ├── OrderRepository.java
│   ├── OrderItemRepository.java
│   └── SalesReportRepository.java
├── model/
│   ├── Product.java
│   ├── Order.java
│   ├── OrderItem.java
│   └── SalesReport.java
├── dto/
│   ├── OrderItemDTO.java
│   └── CreateOrderDTO.java
└── exception/
    ├── ResourceNotFoundException.java
    ├── ErrorResponse.java
    └── GlobalExceptionHandler.java
```

## 🚀 Getting Started

### Prerequisites
- Java 17+
- Maven 3.6+
- MySQL 8.0+

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/aravindpal768623/learning.git
   cd learning
   ```

2. **Create MySQL Database**
   ```sql
   CREATE DATABASE pos_system;
   ```

3. **Configure Database Credentials**
   - Open `src/main/resources/application.properties`
   - Update the database URL, username, and password:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/pos_system
   spring.datasource.username=root
   spring.datasource.password=your_password
   ```

4. **Build the Project**
   ```bash
   mvn clean install
   ```

5. **Run the Application**
   ```bash
   mvn spring-boot:run
   ```

   The API will be available at: `http://localhost:8080`

## 📡 API Endpoints

### Products

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/products` | Add a new product |
| GET | `/api/v1/products` | Get all products |
| GET | `/api/v1/products/{id}` | Get product by ID |
| GET | `/api/v1/products/code/{productCode}` | Get product by code |
| PUT | `/api/v1/products/{id}` | Update product |
| DELETE | `/api/v1/products/{id}` | Delete product |
| PUT | `/api/v1/products/{id}/reduce-inventory` | Reduce inventory (query param: quantity) |
| PUT | `/api/v1/products/{id}/increase-inventory` | Increase inventory (query param: quantity) |

### Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/orders` | Create a new order |
| GET | `/api/v1/orders` | Get all orders |
| GET | `/api/v1/orders/{id}` | Get order by ID |
| GET | `/api/v1/orders/number/{orderNumber}` | Get order by order number |
| GET | `/api/v1/orders/status/{status}` | Get orders by status (PENDING, COMPLETED, CANCELLED) |
| PUT | `/api/v1/orders/{id}/complete` | Complete an order |
| PUT | `/api/v1/orders/{id}/cancel` | Cancel an order |

### Sales Reports

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/sales-reports/generate` | Generate daily report (query param: reportDate) |
| GET | `/api/v1/sales-reports` | Get all reports |
| GET | `/api/v1/sales-reports/{id}` | Get report by ID |
| GET | `/api/v1/sales-reports/date/{reportDate}` | Get report by date |

## 📝 Sample API Requests

### Create a Product
```bash
POST http://localhost:8080/api/v1/products
Content-Type: application/json

{
  "name": "Laptop",
  "productCode": "LAPTOP-001",
  "price": 999.99,
  "quantity": 50,
  "description": "High-performance laptop"
}
```

### Create an Order
```bash
POST http://localhost:8080/api/v1/orders
Content-Type: application/json

{
  "orderNumber": "ORD-001",
  "customerName": "John Doe",
  "customerPhone": "1234567890",
  "notes": "Expedited shipping",
  "items": [
    {
      "productId": 1,
      "quantity": 2
    }
  ]
}
```

### Generate Sales Report
```bash
POST http://localhost:8080/api/v1/sales-reports/generate?reportDate=2026-05-11
```

### Complete an Order
```bash
PUT http://localhost:8080/api/v1/orders/1/complete
```

### Cancel an Order
```bash
PUT http://localhost:8080/api/v1/orders/1/cancel
```

## 🔐 Database Schema

### Products Table
- id (Primary Key)
- name
- productCode (Unique)
- price
- quantity
- description
- createdAt
- updatedAt

### Orders Table
- id (Primary Key)
- orderNumber (Unique)
- customerName
- customerPhone
- totalAmount
- status (PENDING, COMPLETED, CANCELLED)
- notes
- createdAt
- updatedAt

### OrderItems Table
- id (Primary Key)
- orderId (Foreign Key)
- productId (Foreign Key)
- quantity
- unitPrice
- totalPrice

### SalesReports Table
- id (Primary Key)
- reportDate (Unique)
- totalSales
- totalOrders
- totalItems
- averageOrderValue
- createdAt

## 🧪 Testing

You can test the API using:
- **Postman** - Import endpoints from the API Endpoints section
- **cURL** - Use the sample requests provided
- **VS Code REST Client** - Create a `.http` file with the requests

## 📚 Key Features Explained

### Inventory Management
- When an order is created, the inventory is automatically reduced
- When an order is cancelled, the inventory is automatically restored
- API prevents creating orders with insufficient inventory

### Sales Reports
- Daily reports are generated based on completed orders
- Reports include total sales, order count, item count, and average order value
- Prevents duplicate reports for the same date

### Error Handling
- Global exception handler with meaningful HTTP status codes
- Validation errors return 400 Bad Request
- Resource not found errors return 404 Not Found
- Server errors return 500 Internal Server Error

## 🔗 Database Connection Tips

If you encounter connection issues:
1. Ensure MySQL service is running
2. Verify database name is `pos_system`
3. Check username and password in `application.properties`
4. Use `jdbc:mysql://localhost:3306/pos_system?useSSL=false&serverTimezone=UTC` if timezone issues occur

## 📞 Support

For issues or questions, please create an issue in the repository.

## 📄 License

This project is open source and available under the MIT License.
