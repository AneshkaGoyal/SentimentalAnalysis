# Sequence Diagrams for APIs and Flows

## Overview
This document contains sequence diagrams for the API flows in the application. The application is a Spring Boot service that provides several REST endpoints and communicates with external services.

## 1. Basic Hello World API Flow

```mermaid
sequenceDiagram
    participant Client
    participant HelloWorldController
    
    Client->>HelloWorldController: GET /api/hello
    HelloWorldController-->>Client: Return "Hello, World!"
```

## 2. User Service API Flow

```mermaid
sequenceDiagram
    participant Client
    participant HelloWorldController
    participant LoggingInterceptor
    participant UserService
    
    Client->>HelloWorldController: GET /api/user-service
    HelloWorldController->>LoggingInterceptor: Intercept request
    Note over LoggingInterceptor: Log request details
    LoggingInterceptor->>UserService: Forward request to http://user-service/api/users
    UserService-->>LoggingInterceptor: Return response
    Note over LoggingInterceptor: Log response details
    LoggingInterceptor-->>HelloWorldController: Return processed response
    HelloWorldController-->>Client: Return user service data
```

## 3. Company Service API Flow

```mermaid
sequenceDiagram
    participant Client
    participant HelloWorldController
    participant LoggingInterceptor
    participant CompanyService
    
    Client->>HelloWorldController: GET /api/company-service
    HelloWorldController->>LoggingInterceptor: Intercept request (via companyServiceRestTemplate)
    Note over LoggingInterceptor: Log request details
    LoggingInterceptor->>CompanyService: Forward request to http://company-service/api/companies
    CompanyService-->>LoggingInterceptor: Return response
    Note over LoggingInterceptor: Log response details
    LoggingInterceptor-->>HelloWorldController: Return processed response
    HelloWorldController-->>Client: Return company service data
```

## 4. Flight Service API Flow

```mermaid
sequenceDiagram
    participant Client
    participant HelloWorldController
    participant LoggingInterceptor
    participant FlightService
    
    Client->>HelloWorldController: GET /api/flight-service
    HelloWorldController->>LoggingInterceptor: Intercept request (via flightServiceRestTemplate)
    Note over LoggingInterceptor: Log request details
    LoggingInterceptor->>FlightService: Forward request to http://flight-service/api/flights
    FlightService-->>LoggingInterceptor: Return response
    Note over LoggingInterceptor: Log response details
    LoggingInterceptor-->>HelloWorldController: Return processed response
    HelloWorldController-->>Client: Return flight service data
```

## 5. Overall Application Initialization Flow

```mermaid
sequenceDiagram
    participant User
    participant HelloWorldApplication
    participant SpringApplication
    participant AppConfig
    participant LoggingInterceptor
    participant HelloWorldController
    
    User->>HelloWorldApplication: Start application
    HelloWorldApplication->>SpringApplication: run(HelloWorldApplication.class, args)
    SpringApplication->>AppConfig: Initialize configuration
    AppConfig->>LoggingInterceptor: Create and autowire
    AppConfig->>AppConfig: Create RestTemplate beans
    Note over AppConfig: Configure with LoggingInterceptor
    SpringApplication->>HelloWorldController: Initialize controller
    Note over HelloWorldController: Autowire RestTemplate beans
    SpringApplication-->>HelloWorldApplication: Application started
    HelloWorldApplication-->>User: Application ready
```
