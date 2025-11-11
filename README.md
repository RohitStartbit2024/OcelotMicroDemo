# Ocelot Microservices Demo

A demonstration project showcasing a microservices architecture using Ocelot as an API Gateway in .NET. This project includes a gateway service that routes requests to three downstream microservices: Order Service, Product Service, and User Service.

## Architecture Overview

The project follows a microservices architecture with the following components:

- **Gateway Service**: Acts as the API Gateway using Ocelot, routing requests to appropriate microservices and aggregating Swagger documentation.
- **Order Service**: Handles order-related operations.
- **Product Service**: Manages product data and operations.
- **User Service**: Manages user information and authentication.

### Architecture Diagram

```
[Client] -> [Gateway Service (Port 5102)] -> [Order Service (Port 5015)]
                                         -> [Product Service (Port 5267)]
                                         -> [User Service (Port 5184)]
```

## Prerequisites

- .NET 9.0 SDK
- Visual Studio 2022 or Visual Studio Code
- Git (for cloning the repository)

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd OcelotMicroDemo
   ```

2. Restore NuGet packages for all services:
   ```bash
   dotnet restore
   ```

## Running the Services

### Option 1: Using Visual Studio

1. Open `OcelotMicroDemo.sln` in Visual Studio.
2. Set multiple startup projects:
   - Right-click the solution in Solution Explorer.
   - Select "Set Startup Projects".
   - Choose "Multiple startup projects" and set all four projects (GatewayService, OrderService, ProductService, UserService) to "Start".
3. Press F5 or click "Start" to run all services.

### Option 2: Using Command Line

Open separate terminal windows for each service:

1. **Gateway Service** (Port 5102):
   ```bash
   cd GatewayService
   dotnet run
   ```

2. **Order Service** (Port 5015):
   ```bash
   cd OrderService
   dotnet run
   ```

3. **Product Service** (Port 5267):
   ```bash
   cd ProductService
   dotnet run
   ```

4. **User Service** (Port 5184):
   ```bash
   cd UserService
   dotnet run
   ```

## API Documentation

Once all services are running, access the Swagger documentation:

- **Gateway Service Swagger UI**: http://localhost:5102/swagger/index.html
  - Aggregates documentation from all downstream services

- **Individual Service Swagger UIs**:
  - Order Service: http://localhost:5015/swagger/index.html
  - Product Service: http://localhost:5267/swagger/index.html
  - User Service: http://localhost:5184/swagger/index.html

## API Endpoints

### Gateway Routes

The gateway routes requests to downstream services using the following patterns:

- `/api/Order/{everything}` -> Order Service
- `/api/Product/{everything}` -> Product Service
- `/api/User/{everything}` -> User Service

### Sample Endpoints

- **Order Service**:
  - GET `/api/Order/Test/GetMagicNumber` - Returns a magic number (42)

- **Product Service**:
  - Health check available at `/api/Health`

- **User Service**:
  - Health check available at `/api/Health`

## Configuration

### Ocelot Configuration

The gateway configuration is defined in `GatewayService/ocelot.json`. This file contains:

- Route definitions for each downstream service
- Upstream and downstream path templates
- Host and port configurations
- Swagger endpoint configurations

### Service Configurations

Each service has its own `appsettings.json` and `appsettings.Development.json` for environment-specific configurations.

## Testing

### Health Checks

All services include health check endpoints:

- Gateway: Not directly available (routes to services)
- Order: http://localhost:5015/api/Health
- Product: http://localhost:5267/api/Health
- User: http://localhost:5184/api/Health

### API Testing

Use the Swagger UI or tools like Postman to test the APIs:

1. Access the Gateway Swagger UI at http://localhost:5102/swagger/index.html
2. Test endpoints through the gateway using the `/api/{Service}/{endpoint}` pattern

## Technologies Used

- **.NET 9.0**: Framework for building the services
- **Ocelot**: API Gateway library for .NET
- **Swagger/OpenAPI**: API documentation and testing
- **MMLib.SwaggerForOcelot**: Swagger aggregation for Ocelot
- **ASP.NET Core**: Web framework for building APIs

## Project Structure

```
OcelotMicroDemo/
├── GatewayService/          # API Gateway using Ocelot
│   ├── ocelot.json         # Gateway configuration
│   ├── Program.cs          # Application entry point
│   └── ...
├── OrderService/           # Order microservice
│   ├── Controllers/        # API controllers
│   ├── Program.cs          # Application entry point
│   └── ...
├── ProductService/         # Product microservice
│   ├── Controllers/        # API controllers
│   ├── Program.cs          # Application entry point
│   └── ...
├── UserService/            # User microservice
│   ├── Controllers/        # API controllers
│   ├── Program.cs          # Application entry point
│   └── ...
├── OcelotMicroDemo.sln     # Visual Studio solution file
└── README.md               # This file
```

### Common Issues

1. **Port conflicts**: Ensure the specified ports (5102, 5015, 5267, 5184) are available.
2. **NuGet restore failures**: Run `dotnet restore` in each service directory.
3. **Swagger not loading**: Check that all services are running and accessible.

### Logs

Check the console output of each running service for error messages and debugging information.
