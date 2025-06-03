# ERP System Backend

A modular, scalable ERP (Enterprise Resource Planning) system backend built with Node.js, Express, and MongoDB.

## Project Structure

```
src/
├── applications/           # Application-specific modules
│   └── mrp/               # Manufacturing Resource Planning module
│       ├── domain/        # Business logic and domain models
│       │   ├── models/    # Mongoose schemas and models
│       │   ├── services/  # Business logic services
│       ├── infrastructure/# Infrastructure implementations
│       │   └── repositories/ # MongoDB repository implementations
│       │   └── storage/ # Storage functions
│       │   └── storage/ # Storage functions
│       │   └── services/ # Additional services (notification, messaging)
│       └── interface/     # API interface layer
│           ├── controllers/ # Request handlers
│           ├── middlewares/ # Middlewares for filtering
│           ├── routes/    # Route definitions
│           └── validators/ # Request validation schemas
│
├── core/                  # Core system functionality
│   ├── auth/             # Authentication module
│   │   ├── domain/       # Auth domain models and logic
│   │   ├── infrastructure/ # Auth implementations
│   │   └── interface/    # Auth API endpoints
│   └── shared/           # Shared core functionality
│       ├── config/       # Core configuration
│       └── utils/        # Shared utilities
│
├── config/               # Application configuration
│   ├── db.js            # Database configuration
│   └── module-alias.js  # Module alias configuration
│
├── middlewares/         # Express middlewares
│   ├── auth.js         # Authentication middleware
│   ├── error.js        # Error handling middleware
│   └── validation.js   # Request validation middleware
│
├── utils/              # Utility functions
│   ├── logger.js       # Logging utility
│   └── response.js     # Response formatting
│
├── scripts/            # Utility scripts
│   └── seed-mrp-data.js # Data seeding script
│
├── app.js             # Express application setup
└── server.js          # Server entry point
```

## Architecture Overview

### 1. Applications Layer
The `applications` directory contains all application-specific modules. Each applications follows a clean architecture pattern:

- **Domain Layer**: Contains business logic, models, and repository interfaces
  - `models/`: Mongoose schemas and models
  - `services/`: Business logic implementation

- **Infrastructure Layer**: Contains external service implementations
  - `repositories/`: MongoDB repository implementations

- **Interface Layer**: Contains API endpoints and request handling
  - `controllers/`: Request handlers
  - `routes/`: Route definitions
  - `validators/`: Request validation schemas
  - `middelwares/`: Internal middelwares

### 2. Core Layer
The `core` directory contains essential system functionality:

- **Auth Module**: Handles authentication and authorization
  - User management
  - JWT token handling
  - Permission management

- **Shared Core**: Common functionality used across modules
  - Configuration management
  - Shared utilities

### 3. Configuration
The `config` directory contains application-wide configuration:

- `db.js`: Database connection configuration
- `module-alias.js`: Module path aliases

### 4. Middlewares
Express middlewares for cross-cutting concerns:

- Authentication
- Error handling
- Request validation
- Logging

### 5. Utilities
Common utility functions:

- Logging
- Response formatting
- Error handling

## Database Configuration

### Multiple Database Support
The system supports multiple database types through a unified configuration in `src/config/db.js`:

1. **MongoDB Connections**
   - Core Database: For system-wide data (users, settings, etc.)
   - MRP Database: For manufacturing resource planning data
   - Each module can have its own database

2. **MySQL Connection**
   - Legacy Database: For existing data or specific use cases
   - Supports both Sequelize ORM and raw queries

### Database Configuration Example
```javascript
// src/config/db.js
const dbConfig = {
  mongodb: {
    core: {
      uri: process.env.MONGODB_CORE_URI,
      options: { /* MongoDB options */ }
    },
    mrp: {
      uri: process.env.MONGODB_MRP_URI,
      options: { /* MongoDB options */ }
    }
  },
  mysql: {
    erp: {
      host: process.env.MYSQL_HOST,
      port: process.env.MYSQL_PORT,
      database: process.env.MYSQL_DATABASE,
      username: process.env.MYSQL_USER,
      password: process.env.MYSQL_PASSWORD
    }
  }
};
```

### Using Multiple Databases
1. **MongoDB Usage**
   ```javascript
   const { getConnection } = require('@shared/config/database');
   
   // Get core database connection
   const coreDb = await getConnection('mongodb', 'core');
   
   // Get MRP database connection
   const mrpDb = await getConnection('mongodb', 'mrp');
   ```

2. **MySQL Usage**
   ```javascript
   const { getConnection } = require('@shared/config/database');
   
   // Get MySQL connection
   const mysqlDb = await getConnection('mysql', 'erp');
   ```

### Best Practices for Database Usage
1. **Connection Management**
   - Use connection pooling
   - Handle connection errors
   - Implement retry mechanisms

2. **Data Consistency**
   - Use transactions where needed
   - Implement proper error handling
   - Maintain data integrity

3. **Performance**
   - Use proper indexing
   - Implement caching
   - Optimize queries

## Getting Started

### Prerequisites
- Node.js >= 14.0.0
- MongoDB
- MySQL (for legacy data)

### Installation
1. Clone the repository
2. Install dependencies:
   ```bash
   npm install
   ```
3. Create `.env` file with required environment variables
4. Start the development server:
   ```bash
   npm run dev
   ```

### Environment Variables
Create a `.env` file with the following variables:
```
# Server
PORT=5000
NODE_ENV=development

# MongoDB
MONGODB_URI=mongodb://localhost:27017/erp
MONGODB_CORE_URI=mongodb://localhost:27017/erp_core
MONGODB_MRP_URI=mongodb://localhost:27017/erp_mrp

# MySQL
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_DATABASE=erp
MYSQL_USER=root
MYSQL_PASSWORD=password

# JWT
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=24h
```

## Development Guidelines

### 1. Adding a New Module
1. Create a new directory under `applications/`
2. Follow the clean architecture pattern:
   ```
   module/
   ├── domain/
   │   ├── models/
   │   ├── services/
   │   └── repositories/
   ├── infrastructure/
   │   └── repositories/
   └── interface/
       ├── controllers/
       ├── routes/
       └── validators/
   ```

### 2. Database Operations
- Use repository pattern for database operations
- Implement interfaces in domain layer
- Implement concrete classes in infrastructure layer

### 3. API Development
- Follow RESTful principles
- Use controllers for request handling
- Implement request validation
- Use proper error handling

### 4. Testing
- Write unit tests for services
- Write integration tests for APIs
- Use Jest as testing framework

## Best Practices

1. **Code Organization**
   - Follow the clean architecture pattern
   - Keep modules independent
   - Use dependency injection

2. **Error Handling**
   - Use custom error classes
   - Implement proper error middleware
   - Log errors appropriately

3. **Security**
   - Implement proper authentication
   - Use input validation
   - Follow security best practices

4. **Performance**
   - Use proper indexing
   - Implement caching where needed
   - Optimize database queries

