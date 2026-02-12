# Firefly Framework - Client

[![CI](https://github.com/fireflyframework/fireflyframework-client/actions/workflows/ci.yml/badge.svg)](https://github.com/fireflyframework/fireflyframework-client/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-21%2B-orange.svg)](https://openjdk.org)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-green.svg)](https://spring.io/projects/spring-boot)

> Unified reactive client library for REST, SOAP, gRPC, GraphQL, and WebSocket services with circuit breakers and observability.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Firefly Framework Client provides a unified, reactive communication library that supports multiple protocols through a consistent API. It includes pre-built clients for REST, SOAP, gRPC, GraphQL, and WebSocket services, each with integrated circuit breaker patterns, health monitoring, and comprehensive metrics collection.

The library features builder-based client construction (`RestClientBuilder`, `SoapClientBuilder`, `GrpcClientBuilder`), service discovery integration (Eureka, Consul, Kubernetes, static), and advanced capabilities including HTTP response caching, request deduplication, OAuth2 authentication, API key management, and certificate pinning.

Additional features include a chaos engineering interceptor for fault injection during testing, client-side rate limiting, interceptor chain support for request/response transformation, and pluggable error handling with protocol-specific error mappers.

## Features

- REST, SOAP, gRPC, GraphQL, and WebSocket client implementations
- Builder pattern for type-safe client construction
- Circuit breaker with configurable sliding window and failure thresholds
- Service discovery: Eureka, Consul, Kubernetes, static endpoints
- Load balancer strategy support
- OAuth2 client helper with token management
- API key management and certificate pinning
- HTTP response caching with configurable cache policies
- Request deduplication manager
- Interceptor chain for logging, metrics, and custom transformations
- Chaos engineering interceptor for fault injection testing
- Client-side rate limiting
- Health indicators and metrics per client instance
- Error handling with HTTP, gRPC, and SOAP fault mappers
- Multipart upload helper
- Dynamic JSON response handling
- Spring Boot auto-configuration with validation

## Requirements

- Java 21+
- Spring Boot 3.x
- Maven 3.9+

## Installation

```xml
<dependency>
    <groupId>org.fireflyframework</groupId>
    <artifactId>fireflyframework-client</artifactId>
    <version>26.02.03</version>
</dependency>
```

## Quick Start

```java
import org.fireflyframework.client.RestClient;
import org.fireflyframework.client.builder.RestClientBuilder;

@Configuration
public class ClientConfig {

    @Bean
    public RestClient paymentClient() {
        return RestClientBuilder.create("payment-service")
            .baseUrl("https://api.payments.com")
            .connectTimeout(Duration.ofSeconds(5))
            .circuitBreaker(cb -> cb
                .failureRateThreshold(50)
                .waitDurationInOpenState(Duration.ofSeconds(30)))
            .build();
    }
}

@Service
public class PaymentService {

    private final RestClient paymentClient;

    public Mono<PaymentResponse> charge(PaymentRequest request) {
        return paymentClient.post("/charges", request, PaymentResponse.class);
    }
}
```

## Configuration

```yaml
firefly:
  client:
    services:
      payment-service:
        base-url: https://api.payments.com
        connect-timeout: 5s
        read-timeout: 10s
        circuit-breaker:
          enabled: true
          failure-rate-threshold: 50
        retry:
          max-attempts: 3
          backoff: 1s
```

## Documentation

Additional documentation is available in the [docs/](docs/) directory:

- [Quick Start](docs/QUICK_START.md)
- [Configuration](docs/CONFIGURATION.md)
- [Rest Client](docs/REST_CLIENT.md)
- [Soap Client](docs/SOAP_CLIENT.md)
- [Grpc Client](docs/GRPC_CLIENT.md)
- [Graphql Client](docs/GRAPHQL_CLIENT.md)
- [Websocket Helper](docs/WEBSOCKET_HELPER.md)
- [Multipart Helper](docs/MULTIPART_HELPER.md)
- [Oauth2 Helper](docs/OAUTH2_HELPER.md)
- [Security](docs/SECURITY.md)
- [Observability](docs/OBSERVABILITY.md)
- [Advanced Features](docs/ADVANCED_FEATURES.md)
- [Integration Testing](docs/INTEGRATION_TESTING.md)

## Contributing

Contributions are welcome. Please read the [CONTRIBUTING.md](CONTRIBUTING.md) guide for details on our code of conduct, development process, and how to submit pull requests.

## License

Copyright 2024-2026 Firefly Software Solutions Inc.

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
