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
package com.conversion.currency.controller;


import java.util.Map;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import com.conversion.currency.models.ConversionRequest;
import com.conversion.currency.models.ConversionResponse;
import com.conversion.currency.service.CurrencyService;

@RestController
@RequestMapping("/api")
public class currencyController {
    
    @Autowired
    private CurrencyService currencyService;

    @GetMapping("/rates")
    public ResponseEntity<Map<String, Double>> getExchangeRates(@RequestParam(defaultValue = "USD") String base) {
        Map<String, Double> rates = currencyService.fetchExchangeRates(base);
        return ResponseEntity.ok(rates);
    }

    @PostMapping("/convert")
    public ResponseEntity<ConversionResponse> convertCurrency(@RequestBody ConversionRequest request) {
        double convertedAmount = currencyService.convertCurrency(request.getFrom(), request.getTo(), request.getAmount());
        ConversionResponse response = new ConversionResponse(request.getFrom(), request.getTo(), request.getAmount(), convertedAmount);
        return ResponseEntity.ok(response);
    }


}
```
