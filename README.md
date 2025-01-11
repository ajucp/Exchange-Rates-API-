# Currency Converter API Integration

## Introduction

This project is a Spring Boot application designed to provide real-time currency conversion functionality. It integrates with a public API (e.g., Exchange Rates API or Open Exchange Rates) to fetch live currency exchange rates and perform conversions. The application aims to be simple, robust, and adherent to best practices in Java and Spring Boot development.

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Endpoints](#endpoints)
- [Error Handling](#error-handling)
- [Development](#development)
- [Testing](#testing)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following installed:
- Java (compatible version)
- Maven
- An IDE such as IntelliJ IDEA, Eclipse, or Visual Studio Code

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/ajucp/Exchange-Rates-API
   ```
2. Navigate to the project directory:
   ```bash
   cd Exchange-Rates-API
   ```
3. Ensure Maven Wrapper is executable:
   ```bash
   chmod +x mvnw
   ```

## Usage

1. Build the project using Maven:
   ```bash
   ./mvnw clean install
   ```
2. Run the application:
   ```bash
   ./mvnw spring-boot:run
   ```
3. Access the application at `http://localhost:8080`.

## Endpoints

### 1. Fetch Exchange Rates
**GET /api/rates?base=USD**

Fetch the exchange rates for the given base currency. If no base is provided, the default is `USD`.

### 2. Convert Currency
**POST /api/convert**

Convert an amount from one currency to another using the fetched exchange rates.

**Request Body:**
```json
{
  "from": "USD",
  "to": "EUR",
  "amount": 100
}
```

**Response:**
```json
{
  "from": "USD",
  "to": "EUR",
  "amount": 100,
  "convertedAmount": 94.5
}
```

## Error Handling

The application handles the following cases:
1. External API is unavailable.
2. Invalid currency codes are provided in the request.

## Development

### Code Style and Guidelines
- Ensure all code adheres to the project's coding standards.
- Use line endings as specified in `.gitattributes`:
  - `eol=lf` for Unix-based systems.
  - `eol=crlf` for Windows-based systems.

### IDE-Specific Configurations
- `.gitignore` includes configurations for common IDEs like IntelliJ IDEA, Eclipse, and Visual Studio Code.
- Use the appropriate IDE plugins to enhance development experience.

## Testing

Unit tests for the service layer are written using JUnit. To run tests:
```bash
./mvnw test
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Example Code

```java
package com.example.project;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
@RequestMapping("/api")
class CurrencyController {

    @GetMapping("/rates")
    public String getRates(@RequestParam(defaultValue = "USD") String base) {
        // Logic to fetch exchange rates from an external API
        return "Rates for base currency: " + base;
    }

    @PostMapping("/convert")
    public ConversionResponse convertCurrency(@RequestBody ConversionRequest request) {
        // Logic to convert currency using fetched rates
        double convertedAmount = request.getAmount() * 0.945; // Example conversion logic
        return new ConversionResponse(request.getFrom(), request.getTo(), request.getAmount(), convertedAmount);
    }
}

class ConversionRequest {
    private String from;
    private String to;
    private double amount;

    // Getters and setters
}

class ConversionResponse {
    private String from;
    private String to;
    private double amount;
    private double convertedAmount;

    public ConversionResponse(String from, String to, double amount, double convertedAmount) {
        this.from = from;
        this.to = to;
        this.amount = amount;
        this.convertedAmount = convertedAmount;
    }

    // Getters and setters
}
}
```
