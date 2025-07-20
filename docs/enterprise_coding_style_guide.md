# Enterprise Software Development Coding Style Guide

## Document Information
- **Version**: 1.0
- **Purpose**: Establish consistent coding standards across all enterprise software development projects
- **Audience**: Software Developers, Technical Leads, Code Reviewers, DevOps Engineers
- **Scope**: All programming languages and development activities within enterprise projects

---

## 1. General Principles

### 1.1 Code Quality Fundamentals
**Readability First**: Code is read far more often than it is written. Optimize for human understanding over clever shortcuts.

**Consistency Over Personal Preference**: Follow established team conventions even if they differ from personal preferences.

**Explicit Over Implicit**: Make code intentions clear through explicit naming, typing, and structure rather than relying on implicit behavior.

**Fail Fast and Fail Clearly**: Design code to detect and report errors as early as possible with clear, actionable error messages.

### 1.2 SOLID Principles Compliance
All code must adhere to SOLID design principles:
- **Single Responsibility**: Each class/function should have one reason to change
- **Open/Closed**: Open for extension, closed for modification
- **Liskov Substitution**: Derived classes must be substitutable for base classes
- **Interface Segregation**: Clients should not depend on interfaces they don't use
- **Dependency Inversion**: Depend on abstractions, not concretions

---

## 2. Naming Conventions

### 2.1 Universal Naming Rules
**Descriptive and Meaningful**: Names should clearly indicate purpose and usage.

❌ **Poor**: `d`, `tmp`, `data1`, `processData()`

✅ **Good**: `customerBirthDate`, `temporaryUserSession`, `validateUserCredentials()`

**Avoid Abbreviations**: Use complete words unless the abbreviation is universally understood.

❌ **Poor**: `usrMgr`, `calcTax()`, `dbConn`

✅ **Good**: `userManager`, `calculateTax()`, `databaseConnection`

**Use Consistent Vocabulary**: Use the same terms for the same concepts throughout the codebase.

**Searchable Names**: Choose names that can be easily found with text search tools.

### 2.2 Language-Specific Conventions

#### Java/C# Conventions
```java
// Classes: PascalCase
public class CustomerAccountManager { }

// Methods and Variables: camelCase
public void processPaymentTransaction() { }
private String customerEmailAddress;

// Constants: UPPER_SNAKE_CASE
public static final int MAX_RETRY_ATTEMPTS = 3;

// Packages/Namespaces: lowercase with dots
package com.company.project.module;
namespace Company.Project.Module;

// Interfaces: Descriptive nouns or adjectives
public interface PaymentProcessor { }
public interface Serializable { }
```

#### Python Conventions
```python
# Classes: PascalCase
class CustomerAccountManager:

# Functions and Variables: snake_case
def process_payment_transaction():
customer_email_address = ""

# Constants: UPPER_SNAKE_CASE
MAX_RETRY_ATTEMPTS = 3

# Modules: lowercase with underscores
import user_authentication_service

# Private members: leading underscore
def _validate_internal_data():
```

#### JavaScript/TypeScript Conventions
```javascript
// Classes: PascalCase
class CustomerAccountManager { }

// Functions and Variables: camelCase
function processPaymentTransaction() { }
const customerEmailAddress = '';

// Constants: UPPER_SNAKE_CASE or camelCase for modern JS
const MAX_RETRY_ATTEMPTS = 3;
const apiConfiguration = Object.freeze({ });

// Files: kebab-case or camelCase
// customer-account-manager.js
// customerAccountManager.ts
```

#### C++ (C++17 and above) Conventions
```cpp
// Classes and Structs: PascalCase
class CustomerAccountManager {
public:
    // Public methods: camelCase
    void processPaymentTransaction();
    
private:
    // Private members: snake_case with trailing underscore
    std::string customer_email_address_;
    int retry_count_;
};

// Functions: camelCase (for consistency with modern C++)
void processPaymentTransaction();

// Variables: snake_case
std::string customer_email_address;
int payment_amount;

// Constants: UPPER_SNAKE_CASE or kConstantName
constexpr int MAX_RETRY_ATTEMPTS = 3;
constexpr double kDefaultInterestRate = 0.05;

// Namespaces: snake_case
namespace payment_processing {
namespace validation {
    // Nested namespaces
}
}

// Files: snake_case with appropriate extensions
// customer_account_manager.hpp
// customer_account_manager.cpp

// Template parameters: PascalCase
template<typename DataType, size_t BufferSize>
class DataBuffer { };

// Enums: PascalCase for enum class, UPPER_SNAKE_CASE for values
enum class PaymentStatus {
    PENDING,
    APPROVED,
    DECLINED,
    PROCESSING
};
```

---

## 3. Code Organization and Structure

### 3.1 File and Directory Structure
**Logical Grouping**: Organize files by feature or domain, not by technical layer.

✅ **Good Structure**:
```
src/
├── user-management/
│   ├── user.model.js
│   ├── user.service.js
│   ├── user.controller.js
│   └── user.test.js
├── payment-processing/
│   ├── payment.model.js
│   ├── payment.service.js
│   └── payment.test.js
└── shared/
    ├── utils/
    └── types/
```

❌ **Poor Structure**:
```
src/
├── models/
├── services/
├── controllers/
└── tests/
```

### 3.2 Class and Function Organization
**Consistent Method Ordering**:
1. Static methods
2. Constructors
3. Public methods (alphabetical order)
4. Protected methods
5. Private methods

**Function Length**: Keep functions small and focused (maximum 20-30 lines).

**Class Size**: Limit classes to single responsibility (maximum 200-300 lines).

### 3.3 Import and Dependency Management
**Explicit Imports**: Import only what is needed, avoid wildcard imports.

```javascript
// ✅ Good
import { validateEmail, formatPhoneNumber } from './utils/validation';

// ❌ Poor
import * from './utils/validation';
```

**Dependency Injection**: Use dependency injection for external dependencies to improve testability.

```java
// ✅ Good - Dependencies injected
public class PaymentService {
    private final PaymentGateway paymentGateway;
    private final NotificationService notificationService;
    
    public PaymentService(PaymentGateway gateway, NotificationService notifications) {
        this.paymentGateway = gateway;
        this.notificationService = notifications;
    }
}

// ❌ Poor - Hard-coded dependencies
public class PaymentService {
    private PaymentGateway paymentGateway = new StripeGateway();
    private NotificationService notificationService = new EmailService();
}
```

**C++ Specific Patterns**:
```cpp
// ✅ Good - Constructor injection with smart pointers
class PaymentService {
private:
    std::unique_ptr<PaymentGateway> payment_gateway_;
    std::shared_ptr<NotificationService> notification_service_;
    
public:
    PaymentService(std::unique_ptr<PaymentGateway> gateway,
                   std::shared_ptr<NotificationService> notifications)
        : payment_gateway_(std::move(gateway)),
          notification_service_(std::move(notifications)) {}
};

// ✅ Good - Factory pattern for complex object creation
class PaymentServiceFactory {
public:
    static std::unique_ptr<PaymentService> create(const Configuration& config) {
        auto gateway = createPaymentGateway(config.gateway_type);
        auto notifications = std::make_shared<EmailNotificationService>(config.email_config);
        return std::make_unique<PaymentService>(std::move(gateway), notifications);
    }
};
```

---

## 4. Documentation Standards

### 4.1 Code Comments
**When to Comment**:
- Complex business logic or algorithms
- Non-obvious design decisions
- Public API documentation
- Temporary workarounds (with TODO/FIXME)

**When NOT to Comment**:
- Self-explanatory code
- Restating what the code obviously does

```java
// ❌ Poor - Obvious comment
// Increment counter by 1
counter++;

// ✅ Good - Explains business logic
// Apply early payment discount only for premium customers
// as per business rule BR-2023-15
if (customer.isPremium() && payment.isEarlyPayment()) {
    payment.applyDiscount(EARLY_PAYMENT_DISCOUNT_RATE);
}
```

### 4.2 API Documentation
**Function/Method Documentation**: Use standard documentation formats (JSDoc, Javadoc, docstrings, Doxygen for C++) with requirement traceability.

```java
/**
 * Processes a payment transaction for the specified customer.
 * 
 * @param customerId The unique identifier for the customer
 * @param amount The payment amount in cents to avoid floating-point precision issues
 * @param paymentMethod The payment method (CREDIT_CARD, BANK_TRANSFER, etc.)
 * @return PaymentResult containing transaction ID and status
 * @throws PaymentException if payment processing fails
 * @throws ValidationException if input parameters are invalid
 * 
 * @requirement REQ-PAY-001 Process customer payments
 * @requirement REQ-SEC-003 Validate payment data before processing
 * @requirement REQ-LOG-001 Log all payment transactions
 */
public PaymentResult processPayment(String customerId, long amount, PaymentMethod paymentMethod) 
    throws PaymentException, ValidationException {
    // Implementation
}
```

```cpp
/**
 * @brief Processes a payment transaction for the specified customer.
 * 
 * This function handles the complete payment workflow including validation,
 * gateway communication, and result processing. All monetary amounts are
 * handled in cents to avoid floating-point precision issues.
 * 
 * @param customer_id The unique identifier for the customer
 * @param amount_cents The payment amount in cents (must be positive)
 * @param payment_method The payment method to use
 * @return PaymentResult containing transaction ID and processing status
 * @throws PaymentException if payment processing fails
 * @throws std::invalid_argument if input parameters are invalid
 * 
 * @requirement REQ-PAY-001 Process customer payments
 * @requirement REQ-VAL-005 Validate payment amounts are positive
 * @requirement REQ-SEC-003 Encrypt sensitive payment data
 * 
 * @note This function is thread-safe when used with different customer IDs
 * @warning Ensure payment_method is properly initialized before calling
 * 
 * @see PaymentResult for details on return value structure
 * @since Version 2.1.0
 */
PaymentResult processPayment(const std::string& customer_id, 
                           int64_t amount_cents,
                           const PaymentMethod& payment_method);
```

### 4.3 File Header Documentation
**Mandatory File Headers**: Every source file must include a header with requirement traceability and metadata.

```java
/**
 * PaymentService.java
 * 
 * Handles payment processing operations for customer transactions.
 * 
 * @author John Doe <john.doe@company.com>
 * @created 2024-01-15
 * @version 1.2.0
 * 
 * Requirements Implemented:
 * @requirement REQ-PAY-001 Process customer payment transactions
 * @requirement REQ-PAY-002 Support multiple payment methods
 * @requirement REQ-SEC-003 Validate and encrypt payment data
 * @requirement REQ-LOG-001 Log all payment activities
 * @requirement REQ-ERR-001 Handle payment processing errors gracefully
 * 
 * Dependencies:
 * - PaymentGateway: External payment processing
 * - CustomerService: Customer validation
 * - AuditLogger: Transaction logging
 * 
 * @copyright 2024 Company Name. All rights reserved.
 * @license Internal use only - Proprietary software
 */
package com.company.payment;

import java.util.logging.Logger;
// ... other imports
```

```cpp
/**
 * @file payment_service.hpp
 * @brief Payment processing service for customer transactions
 * 
 * This module provides comprehensive payment processing capabilities
 * including validation, gateway integration, and transaction logging.
 * 
 * @author Jane Smith <jane.smith@company.com>
 * @created 2024-01-15
 * @version 1.2.0
 * 
 * Requirements Implemented:
 * @requirement REQ-PAY-001 Process customer payment transactions
 * @requirement REQ-PAY-002 Support multiple payment methods
 * @requirement REQ-PERF-001 Process payments within 5 seconds
 * @requirement REQ-SEC-003 Encrypt payment data using AES-256
 * @requirement REQ-LOG-001 Maintain audit trail for all transactions
 * 
 * Dependencies:
 * - payment_gateway.hpp: External payment processing interface
 * - customer_validator.hpp: Customer data validation
 * - crypto_utils.hpp: Encryption and security functions
 * 
 * @note This module requires C++17 or later
 * @copyright 2024 Company Name. All rights reserved.
 * @license Internal use only - Proprietary software
 */

#pragma once

#include <memory>
#include <string>
#include <optional>
// ... other includes
```

```javascript
/**
 * @fileoverview Customer authentication service
 * 
 * Provides secure authentication and session management for customer accounts.
 * Implements OAuth 2.0 and multi-factor authentication workflows.
 * 
 * @author Mike Johnson <mike.johnson@company.com>
 * @created 2024-01-15
 * @version 1.3.2
 * 
 * Requirements Implemented:
 * @requirement REQ-AUTH-001 Authenticate users with username/password
 * @requirement REQ-AUTH-002 Support OAuth 2.0 authentication
 * @requirement REQ-AUTH-003 Implement multi-factor authentication
 * @requirement REQ-SEC-001 Hash passwords using bcrypt
 * @requirement REQ-SEC-002 Implement session timeout after 30 minutes
 * @requirement REQ-LOG-002 Log all authentication attempts
 * 
 * External Dependencies:
 * - bcrypt: Password hashing
 * - jsonwebtoken: JWT token management
 * - passport: Authentication middleware
 * 
 * @copyright 2024 Company Name. All rights reserved.
 * @license Internal use only - Proprietary software
 */

const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
// ... other imports
```

### 4.4 Requirement Traceability Standards
**Consistent Requirement Referencing**: Use standardized format for requirement references throughout code.

**Format**: `@requirement [REQ-ID] [Brief description]`

**Multiple Requirements**: List each requirement on a separate line for clarity.

**Inline Requirement Comments**: For complex logic implementing specific requirements.

```java
public class PaymentValidator {
    
    public void validatePayment(Payment payment) {
        // @requirement REQ-VAL-001: Validate payment amount is positive
        if (payment.getAmount() <= 0) {
            throw new ValidationException("Payment amount must be positive");
        }
        
        // @requirement REQ-VAL-002: Validate customer ID format
        if (!isValidCustomerId(payment.getCustomerId())) {
            throw new ValidationException("Invalid customer ID format");
        }
        
        // @requirement REQ-SEC-004: Validate payment method is supported
        if (!isSupportedPaymentMethod(payment.getPaymentMethod())) {
            throw new ValidationException("Unsupported payment method");
        }
    }
}
```

```cpp
class PaymentProcessor {
public:
    PaymentResult process(const Payment& payment) {
        // @requirement REQ-PERF-001: Start performance timing
        auto start_time = std::chrono::high_resolution_clock::now();
        
        try {
            // @requirement REQ-VAL-003: Validate input before processing
            validatePaymentInput(payment);
            
            // @requirement REQ-SEC-005: Encrypt sensitive data before gateway call
            auto encrypted_payment = encryptPaymentData(payment);
            
            // @requirement REQ-PAY-003: Process through external gateway
            auto gateway_result = payment_gateway_->processPayment(encrypted_payment);
            
            // @requirement REQ-LOG-003: Log successful transactions
            audit_logger_->logSuccessfulPayment(payment.getCustomerId(), 
                                              gateway_result.getTransactionId());
            
            return gateway_result;
            
        } catch (const std::exception& e) {
            // @requirement REQ-ERR-002: Log and handle processing errors
            audit_logger_->logPaymentError(payment.getCustomerId(), e.what());
            throw PaymentException("Payment processing failed: " + std::string(e.what()));
        }
    }
};
```

### 4.5 Automated Requirement Validation
**Tooling Integration**: Use automated tools to verify requirement coverage.

```bash
# Example script to check requirement coverage
#!/bin/bash
# check_requirement_coverage.sh

echo "Checking requirement coverage in codebase..."

# Extract all requirements from requirements document
grep -E "REQ-[A-Z]+-[0-9]+" requirements.md | sort -u > requirements_list.txt

# Extract all requirement references from source code
find src/ -name "*.java" -o -name "*.cpp" -o -name "*.hpp" -o -name "*.js" | \
    xargs grep -h "@requirement" | \
    grep -oE "REQ-[A-Z]+-[0-9]+" | \
    sort -u > implemented_requirements.txt

# Find missing requirements
comm -23 requirements_list.txt implemented_requirements.txt > missing_requirements.txt

if [ -s missing_requirements.txt ]; then
    echo "❌ Missing requirement implementations:"
    cat missing_requirements.txt
    exit 1
else
    echo "✅ All requirements have code implementations"
fi

# Find orphaned requirement references
comm -13 requirements_list.txt implemented_requirements.txt > orphaned_requirements.txt

if [ -s orphaned_requirements.txt ]; then
    echo "⚠️  Warning: Code references to non-existent requirements:"
    cat orphaned_requirements.txt
fi
```

### 4.6 README and Documentation Files
**Project README Requirements**:
- Project purpose and scope
- Setup and installation instructions
- Configuration requirements
- API documentation links
- Contributing guidelines
- License information

---

## 5. Error Handling and Logging

### 5.1 Exception Handling
**Specific Exception Types**: Create custom exception types for different error categories.

```java
// ✅ Good - Specific exceptions
public class PaymentValidationException extends PaymentException {
    public PaymentValidationException(String field, String message) {
        super(String.format("Validation failed for field '%s': %s", field, message));
    }
}

// ❌ Poor - Generic exceptions
throw new Exception("Something went wrong");
```

**C++ Exception Handling**:
```cpp
// ✅ Good - Custom exception hierarchy
class PaymentException : public std::runtime_error {
public:
    explicit PaymentException(const std::string& message) 
        : std::runtime_error("Payment Error: " + message) {}
};

class PaymentValidationException : public PaymentException {
private:
    std::string field_;
    
public:
    PaymentValidationException(const std::string& field, const std::string& message)
        : PaymentException("Validation failed for field '" + field + "': " + message),
          field_(field) {}
          
    const std::string& getField() const noexcept { return field_; }
};

// ✅ Good - RAII and exception safety
class PaymentProcessor {
public:
    PaymentResult processPayment(const PaymentRequest& request) {
        // Use RAII for automatic resource management
        DatabaseConnection connection{database_pool_.acquire()};
        
        try {
            validatePaymentRequest(request);
            auto result = processWithGateway(request);
            connection.commit();
            return result;
        } catch (const PaymentValidationException& e) {
            connection.rollback();
            logger_.error("Validation failed: {}", e.what());
            throw;
        } catch (const std::exception& e) {
            connection.rollback();
            logger_.error("Unexpected error during payment processing: {}", e.what());
            throw PaymentException("Internal processing error");
        }
    }
};

// ❌ Poor - Raw pointers and poor exception safety
void badPaymentProcessing(PaymentRequest* request) {
    DatabaseConnection* conn = new DatabaseConnection();
    // Memory leak if exception is thrown
    processPayment(request);
    delete conn; // May never execute
}
```

**Fail Fast**: Validate inputs at the beginning of methods.

```java
public void processPayment(Payment payment) {
    // ✅ Good - Validate early
    if (payment == null) {
        throw new IllegalArgumentException("Payment cannot be null");
    }
    if (payment.getAmount() <= 0) {
        throw new PaymentValidationException("amount", "Must be greater than zero");
    }
    
    // Process payment logic here
}
```

```cpp
void PaymentService::processPayment(const Payment& payment) {
    // ✅ Good - Validate early with specific exceptions
    if (payment.getCustomerId().empty()) {
        throw PaymentValidationException("customer_id", "Customer ID cannot be empty");
    }
    
    if (payment.getAmount() <= 0) {
        throw PaymentValidationException("amount", "Amount must be greater than zero");
    }
    
    if (!isValidPaymentMethod(payment.getPaymentMethod())) {
        throw PaymentValidationException("payment_method", "Invalid payment method specified");
    }
    
    // Process payment logic here
}

// ✅ Good - Use std::optional for potentially missing values
std::optional<Customer> findCustomerById(const std::string& customer_id) {
    auto it = customer_cache_.find(customer_id);
    if (it != customer_cache_.end()) {
        return it->second;
    }
    return std::nullopt;
}

// Usage with proper error handling
void processCustomerPayment(const std::string& customer_id, const Payment& payment) {
    auto customer = findCustomerById(customer_id);
    if (!customer.has_value()) {
        throw PaymentException("Customer not found: " + customer_id);
    }
    
    // Process with valid customer
    processPayment(customer.value(), payment);
}
```

### 5.2 Logging Standards
**Log Levels**:
- **ERROR**: System errors, exceptions, failures
- **WARN**: Potentially harmful situations, deprecated usage
- **INFO**: General application flow, business events
- **DEBUG**: Detailed information for debugging
- **TRACE**: Very detailed information, typically only for development

**Structured Logging**: Use consistent format with correlation IDs.

```java
// ✅ Good - Structured logging with context
logger.info("Payment processed successfully", 
    Map.of(
        "customerId", payment.getCustomerId(),
        "transactionId", result.getTransactionId(),
        "amount", payment.getAmount(),
        "correlationId", request.getCorrelationId()
    ));

// ❌ Poor - Unstructured logging
logger.info("Payment processed for customer " + customerId);
```

**Security in Logging**: Never log sensitive information.

```java
// ❌ Poor - Sensitive data in logs
logger.info("Processing payment with card number: " + cardNumber);

// ✅ Good - Masked sensitive data
logger.info("Processing payment with card ending in: " + maskCardNumber(cardNumber));
```

---

## 6. Security Best Practices

### 6.1 Input Validation
**Validate All Inputs**: Every external input must be validated before processing.

```java
public class UserRegistrationService {
    public void registerUser(UserRegistrationRequest request) {
        // ✅ Good - Comprehensive validation
        validateEmail(request.getEmail());
        validatePassword(request.getPassword());
        validatePhoneNumber(request.getPhoneNumber());
        sanitizeUserInput(request.getFirstName());
        sanitizeUserInput(request.getLastName());
        
        // Process registration
    }
    
    private void validateEmail(String email) {
        if (email == null || !EMAIL_PATTERN.matcher(email).matches()) {
            throw new ValidationException("Invalid email format");
        }
        if (email.length() > MAX_EMAIL_LENGTH) {
            throw new ValidationException("Email too long");
        }
    }
}
```

### 6.2 Secure Coding Patterns
**Use Parameterized Queries**: Prevent SQL injection attacks.

```java
// ✅ Good - Parameterized query
String sql = "SELECT * FROM users WHERE email = ? AND status = ?";
PreparedStatement stmt = connection.prepareStatement(sql);
stmt.setString(1, email);
stmt.setString(2, "ACTIVE");

// ❌ Poor - SQL injection vulnerable
String sql = "SELECT * FROM users WHERE email = '" + email + "'";
```

**Secure Password Handling**: Never store passwords in plain text.

```java
// ✅ Good - Hashed passwords with salt
public class PasswordService {
    private final BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder(12);
    
    public String hashPassword(String plainPassword) {
        return passwordEncoder.encode(plainPassword);
    }
    
    public boolean verifyPassword(String plainPassword, String hashedPassword) {
        return passwordEncoder.matches(plainPassword, hashedPassword);
    }
}
```

### 6.3 Secret Management
**Environment Variables**: Store secrets in environment variables, not in code.

```java
// ✅ Good - Configuration from environment
@Configuration
public class DatabaseConfig {
    @Value("${database.password}")
    private String databasePassword;
    
    @Value("${api.secret.key}")
    private String apiSecretKey;
}

// ❌ Poor - Hardcoded secrets
private static final String DATABASE_PASSWORD = "mySecretPassword123";
```

---

## 7. Performance Guidelines

### 7.1 Efficient Data Structures
**Choose Appropriate Collections**: Select data structures based on usage patterns.

```java
// ✅ Good - Appropriate data structure choice
Map<String, User> userCache = new ConcurrentHashMap<>();  // For concurrent access
List<String> orderedItems = new ArrayList<>();            // For indexed access
Set<String> uniqueIds = new HashSet<>();                  // For uniqueness checks

// ❌ Poor - Inefficient choice
List<User> users = new ArrayList<>();
// Then searching with: users.stream().filter(u -> u.getId().equals(id)).findFirst()
// Instead of using a Map for O(1) lookup
```

**C++ STL Container Selection**:
```cpp
// ✅ Good - Appropriate STL container choices
std::unordered_map<std::string, User> user_cache;        // O(1) average lookup
std::vector<std::string> ordered_items;                  // Contiguous memory, cache-friendly
std::unordered_set<std::string> unique_ids;             // Fast uniqueness checks
std::deque<Transaction> transaction_queue;               // Efficient front/back operations

// ✅ Good - Modern C++ with algorithms
std::vector<Payment> payments = getPayments();

// Use algorithms instead of raw loops
auto pending_payments = payments | std::views::filter([](const Payment& p) {
    return p.getStatus() == PaymentStatus::PENDING;
}) | std::ranges::to<std::vector>();

// ✅ Good - Use appropriate container for the access pattern
class PaymentCache {
private:
    // Fast lookup by ID
    std::unordered_map<std::string, Payment> payments_by_id_;
    
    // Ordered access by timestamp
    std::map<std::chrono::system_clock::time_point, std::string> payments_by_time_;
    
public:
    void addPayment(const Payment& payment) {
        const auto& id = payment.getId();
        payments_by_id_[id] = payment;
        payments_by_time_[payment.getTimestamp()] = id;
    }
    
    std::optional<Payment> findById(const std::string& id) const {
        auto it = payments_by_id_.find(id);
        return (it != payments_by_id_.end()) ? 
            std::make_optional(it->second) : std::nullopt;
    }
};

// ❌ Poor - Using wrong container for access pattern
std::vector<Payment> payments; // Then doing linear search
auto it = std::find_if(payments.begin(), payments.end(), 
    [&id](const Payment& p) { return p.getId() == id; });
```

### 7.2 Resource Management
**Proper Resource Cleanup**: Always close resources in finally blocks or use try-with-resources.

```java
// ✅ Good - Automatic resource management
try (Connection conn = dataSource.getConnection();
     PreparedStatement stmt = conn.prepareStatement(sql)) {
    
    // Use connection and statement
    
} catch (SQLException e) {
    logger.error("Database operation failed", e);
    throw new DataAccessException("Failed to execute query", e);
}

// ❌ Poor - Resource leak potential
Connection conn = dataSource.getConnection();
PreparedStatement stmt = conn.prepareStatement(sql);
// Missing cleanup code
```

**C++ RAII (Resource Acquisition Is Initialization)**:
```cpp
// ✅ Good - RAII with smart pointers and automatic cleanup
class DatabaseManager {
public:
    auto executeQuery(const std::string& query) -> std::vector<QueryResult> {
        // RAII - connection automatically cleaned up
        auto connection = connection_pool_.acquire();
        
        try {
            auto statement = connection->prepare(query);
            return statement->execute();
        } catch (const DatabaseException& e) {
            logger_.error("Database operation failed: {}", e.what());
            throw;
        }
        // Connection automatically returned to pool via RAII
    }
};

// ✅ Good - Custom RAII wrapper for C APIs
class FileHandler {
private:
    FILE* file_ptr_;
    
public:
    explicit FileHandler(const std::string& filename, const std::string& mode) 
        : file_ptr_(std::fopen(filename.c_str(), mode.c_str())) {
        if (!file_ptr_) {
            throw std::runtime_error("Failed to open file: " + filename);
        }
    }
    
    ~FileHandler() {
        if (file_ptr_) {
            std::fclose(file_ptr_);
        }
    }
    
    // Delete copy constructor/assignment to prevent double-free
    FileHandler(const FileHandler&) = delete;
    FileHandler& operator=(const FileHandler&) = delete;
    
    // Move constructor/assignment
    FileHandler(FileHandler&& other) noexcept : file_ptr_(other.file_ptr_) {
        other.file_ptr_ = nullptr;
    }
    
    FileHandler& operator=(FileHandler&& other) noexcept {
        if (this != &other) {
            if (file_ptr_) {
                std::fclose(file_ptr_);
            }
            file_ptr_ = other.file_ptr_;
            other.file_ptr_ = nullptr;
        }
        return *this;
    }
    
    FILE* get() const noexcept { return file_ptr_; }
};

// Usage
void processConfigFile(const std::string& config_path) {
    FileHandler config_file(config_path, "r");
    // File automatically closed when function exits
    
    char buffer[1024];
    while (std::fgets(buffer, sizeof(buffer), config_file.get())) {
        processConfigLine(buffer);
    }
} // File closed here automatically

// ✅ Good - Thread-safe resource management
class ThreadSafeCache {
private:
    mutable std::shared_mutex mutex_;
    std::unordered_map<std::string, std::shared_ptr<CacheEntry>> cache_;
    
public:
    std::shared_ptr<CacheEntry> get(const std::string& key) const {
        std::shared_lock lock(mutex_); // Automatic unlock
        auto it = cache_.find(key);
        return (it != cache_.end()) ? it->second : nullptr;
    }
    
    void put(const std::string& key, std::shared_ptr<CacheEntry> entry) {
        std::unique_lock lock(mutex_); // Automatic unlock
        cache_[key] = std::move(entry);
    }
};

// ❌ Poor - Manual resource management prone to leaks
void badResourceHandling() {
    FILE* file = fopen("config.txt", "r");
    DatabaseConnection* conn = new DatabaseConnection();
    
    // If exception thrown here, resources leak
    processData(file, conn);
    
    fclose(file);      // May never execute
    delete conn;       // May never execute
}
```

### 7.3 Caching Strategies
**Implement Appropriate Caching**: Cache expensive operations with proper invalidation.

```java
@Service
public class UserService {
    
    @Cacheable(value = "users", key = "#userId")
    public User getUserById(String userId) {
        return userRepository.findById(userId);
    }
    
    @CacheEvict(value = "users", key = "#user.id")
    public User updateUser(User user) {
        return userRepository.save(user);
    }
}
```

---

## 8. Testing Standards

### 8.1 Test Organization
**Test Structure**: Follow Arrange-Act-Assert (AAA) pattern.

```java
@Test
public void shouldCalculateDiscountForPremiumCustomer() {
    // Arrange
    Customer customer = createPremiumCustomer();
    Order order = createOrderWithAmount(100.00);
    DiscountService discountService = new DiscountService();
    
    // Act
    Discount discount = discountService.calculateDiscount(customer, order);
    
    // Assert
    assertThat(discount.getPercentage()).isEqualTo(15.0);
    assertThat(discount.getAmount()).isEqualTo(15.00);
}
```

**C++ Testing with Google Test**:
```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

class PaymentServiceTest : public ::testing::Test {
protected:
    void SetUp() override {
        // Common test setup
        mock_gateway_ = std::make_unique<MockPaymentGateway>();
        payment_service_ = std::make_unique<PaymentService>(
            std::move(mock_gateway_), logger_);
    }
    
    void TearDown() override {
        // Cleanup if needed
    }
    
    std::unique_ptr<MockPaymentGateway> mock_gateway_;
    std::unique_ptr<PaymentService> payment_service_;
    TestLogger logger_;
};

TEST_F(PaymentServiceTest, ShouldProcessValidPaymentSuccessfully) {
    // Arrange
    const Payment payment = createValidPayment(100.50, "USD");
    const PaymentResult expected_result{TransactionId{"txn_123"}, PaymentStatus::APPROVED};
    
    EXPECT_CALL(*mock_gateway_, processPayment(testing::_))
        .WillOnce(testing::Return(expected_result));
    
    // Act
    const auto result = payment_service_->processPayment(payment);
    
    // Assert
    EXPECT_EQ(result.getTransactionId(), expected_result.getTransactionId());
    EXPECT_EQ(result.getStatus(), PaymentStatus::APPROVED);
}

TEST_F(PaymentServiceTest, ShouldThrowExceptionForInvalidAmount) {
    // Arrange
    const Payment invalid_payment = createPaymentWithAmount(-10.0);
    
    // Act & Assert
    EXPECT_THROW({
        payment_service_->processPayment(invalid_payment);
    }, PaymentValidationException);
}

// ✅ Good - Parameterized tests for multiple scenarios
class PaymentValidationTest : public ::testing::TestWithParam<std::tuple<double, bool>> {
};

TEST_P(PaymentValidationTest, ShouldValidatePaymentAmounts) {
    // Arrange
    auto [amount, should_be_valid] = GetParam();
    const Payment payment = createPaymentWithAmount(amount);
    
    // Act & Assert
    if (should_be_valid) {
        EXPECT_NO_THROW(validatePayment(payment));
    } else {
        EXPECT_THROW(validatePayment(payment), PaymentValidationException);
    }
}

INSTANTIATE_TEST_SUITE_P(
    PaymentAmounts,
    PaymentValidationTest,
    ::testing::Values(
        std::make_tuple(0.01, true),    // Minimum valid amount
        std::make_tuple(100.00, true),  // Normal amount
        std::make_tuple(10000.00, true), // Large amount
        std::make_tuple(0.0, false),    // Zero amount
        std::make_tuple(-10.0, false)   // Negative amount
    )
);
```

### 8.2 Test Naming
**Descriptive Test Names**: Test names should describe the scenario and expected outcome.

```java
// ✅ Good - Descriptive test names
@Test
public void shouldThrowValidationExceptionWhenEmailIsInvalid() { }

@Test
public void shouldReturnEmptyListWhenNoUsersExist() { }

@Test
public void shouldApplyDiscountWhenCustomerIsPremiumAndOrderExceedsMinimum() { }

// ❌ Poor - Vague test names
@Test
public void testUser() { }

@Test
public void test1() { }
```

**C++ Test Naming**:
```cpp
// ✅ Good - Descriptive C++ test names
TEST(PaymentValidatorTest, ShouldRejectPaymentWhenAmountIsNegative) {
    // Test implementation
}

TEST(CustomerServiceTest, ShouldReturnNulloptWhenCustomerNotFound) {
    // Test implementation
}

TEST(DatabaseConnectionTest, ShouldRetryConnectionWhenInitialAttemptFails) {
    // Test implementation
}

// ✅ Good - Test fixture for related tests
class PaymentProcessorTest : public ::testing::Test {
protected:
    PaymentProcessor processor_;
    MockDatabase mock_db_;
    TestConfiguration config_;
};

TEST_F(PaymentProcessorTest, ShouldStoreTransactionWhenPaymentSucceeds) {
    // Test implementation
}

TEST_F(PaymentProcessorTest, ShouldRollbackWhenPaymentFails) {
    // Test implementation
}
```

### 8.3 Test Coverage Requirements
**Minimum Coverage Thresholds**:
- Unit Tests: 80% line coverage, 70% branch coverage
- Integration Tests: Critical paths and error scenarios
- End-to-End Tests: Major user workflows

**Mock Usage**: Mock external dependencies, test real logic.

```java
@Test
public void shouldProcessPaymentSuccessfully() {
    // Arrange
    PaymentGateway mockGateway = mock(PaymentGateway.class);
    when(mockGateway.processPayment(any())).thenReturn(successfulResponse());
    
    PaymentService service = new PaymentService(mockGateway);
    Payment payment = createValidPayment();
    
    // Act
    PaymentResult result = service.processPayment(payment);
    
    // Assert
    assertThat(result.isSuccessful()).isTrue();
    verify(mockGateway).processPayment(payment);
}
```

**C++ Mocking with Google Mock**:
```cpp
// Mock interface definition
class MockPaymentGateway : public PaymentGateway {
public:
    MOCK_METHOD(PaymentResult, processPayment, (const Payment& payment), (override));
    MOCK_METHOD(bool, isHealthy, (), (const, override));
    MOCK_METHOD(void, configure, (const GatewayConfig& config), (override));
};

TEST_F(PaymentServiceTest, ShouldHandleGatewayTimeoutGracefully) {
    // Arrange
    const Payment payment = createValidPayment();
    
    EXPECT_CALL(*mock_gateway_, processPayment(testing::_))
        .WillOnce(testing::Throw(GatewayTimeoutException("Request timed out")));
    
    // Act
    PaymentResult result = payment_service_->processPayment(payment);
    
    // Assert
    EXPECT_EQ(result.getStatus(), PaymentStatus::GATEWAY_ERROR);
    EXPECT_THAT(result.getErrorMessage(), testing::HasSubstr("timeout"));
}

// ✅ Good - Integration test with real components
class PaymentServiceIntegrationTest : public ::testing::Test {
protected:
    void SetUp() override {
        // Use real database and mock only external services
        test_database_ = std::make_unique<TestDatabase>();
        mock_notification_service_ = std::make_unique<MockNotificationService>();
        
        payment_service_ = std::make_unique<PaymentService>(
            test_database_.get(),
            mock_notification_service_.get()
        );
    }
    
    std::unique_ptr<TestDatabase> test_database_;
    std::unique_ptr<MockNotificationService> mock_notification_service_;
    std::unique_ptr<PaymentService> payment_service_;
};

TEST_F(PaymentServiceIntegrationTest, ShouldPersistPaymentDataCorrectly) {
    // Test real database interactions with mocked external services
    const Payment payment = createValidPayment();
    
    EXPECT_CALL(*mock_notification_service_, sendConfirmation(testing::_))
        .Times(1);
    
    const auto result = payment_service_->processPayment(payment);
    
    // Verify data was actually persisted in database
    const auto stored_payment = test_database_->findPaymentById(result.getTransactionId());
    ASSERT_TRUE(stored_payment.has_value());
    EXPECT_EQ(stored_payment->getAmount(), payment.getAmount());
}
```

---

## 9. Code Review Standards

### 9.1 Review Checklist
**Functionality**:
- [ ] Code meets requirements as specified
- [ ] Edge cases are handled appropriately
- [ ] Error conditions are properly managed

**Code Quality**:
- [ ] Code follows established style guidelines
- [ ] Names are descriptive and consistent
- [ ] Functions are small and focused
- [ ] No code duplication
- [ ] Requirement traceability is documented

**Security**:
- [ ] Input validation is comprehensive
- [ ] No sensitive data in logs or comments
- [ ] Authentication and authorization are correct

**Performance**:
- [ ] No obvious performance bottlenecks
- [ ] Appropriate data structures used
- [ ] Resources are properly managed

**Testing**:
- [ ] Adequate test coverage
- [ ] Tests are meaningful and maintainable
- [ ] Integration points are tested
- [ ] Test cases validate requirement implementations

### 9.2 Review Process
**Pre-Review Requirements**:
- All automated tests pass
- Code coverage meets minimum thresholds
- Static analysis tools show no critical issues
- Self-review completed by author

**Review Timeline**: 
- Code reviews should be completed within 24 hours for standard changes
- Critical fixes may require immediate review
- Large changes should be broken into smaller, reviewable chunks

---

## 10. Version Control Standards

### 10.1 Commit Message Format
**Conventional Commits**: Use standardized commit message format.

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

**Examples**:
```
feat(auth): add OAuth2 integration for Google login

fix(payment): resolve timeout issue in payment processing
- Increase timeout from 30s to 60s
- Add retry logic for network failures
- Update error messages for better user experience

Closes #123

docs(api): update authentication endpoints documentation

test(user): add integration tests for user registration flow
```

**Commit Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, semicolons, etc.)
- `refactor`: Code refactoring without functional changes
- `test`: Adding or updating tests
- `chore`: Maintenance tasks, dependency updates

### 10.2 Branching Strategy
**GitFlow Pattern**:
- `main`: Production-ready code
- `develop`: Integration branch for features
- `feature/*`: Feature development branches
- `release/*`: Release preparation branches
- `hotfix/*`: Critical production fixes

**Branch Naming**:
```
feature/user-authentication
feature/payment-integration
bugfix/login-timeout-issue
hotfix/security-vulnerability-fix
release/v2.1.0
```

---

## 11. Configuration and Environment Management

### 11.1 Configuration Principles
**Environment-Specific Configuration**: Use environment variables or configuration files for environment-specific settings.

```yaml
# application.yml
spring:
  profiles:
    active: ${ENVIRONMENT:development}
  
---
spring:
  profiles: development
  datasource:
    url: ${DATABASE_URL:jdbc:h2:mem:testdb}
    
---
spring:
  profiles: production
  datasource:
    url: ${DATABASE_URL}
    hikari:
      maximum-pool-size: ${DB_POOL_SIZE:20}
```

### 11.2 Feature Flags
**Feature Toggle Implementation**: Use feature flags for controlled rollouts.

```java
@Service
public class PaymentService {
    
    @Autowired
    private FeatureToggle featureToggle;
    
    public PaymentResult processPayment(Payment payment) {
        if (featureToggle.isEnabled("new-payment-engine")) {
            return newPaymentEngine.process(payment);
        } else {
            return legacyPaymentEngine.process(payment);
        }
    }
}
```

---

## 12. Monitoring and Observability

### 12.1 Application Metrics
**Key Metrics to Track**:
- Response times for critical operations
- Error rates by operation type
- Business metrics (transactions processed, user registrations)
- Resource utilization (CPU, memory, database connections)

```java
@RestController
public class PaymentController {
    
    private final MeterRegistry meterRegistry;
    private final Counter paymentCounter;
    private final Timer paymentTimer;
    
    public PaymentController(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.paymentCounter = Counter.builder("payments.processed")
            .description("Number of payments processed")
            .register(meterRegistry);
        this.paymentTimer = Timer.builder("payment.processing.time")
            .description("Payment processing time")
            .register(meterRegistry);
    }
    
    @PostMapping("/payments")
    public ResponseEntity<PaymentResult> processPayment(@RequestBody Payment payment) {
        return paymentTimer.recordCallable(() -> {
            PaymentResult result = paymentService.processPayment(payment);
            paymentCounter.increment(
                Tags.of("status", result.isSuccessful() ? "success" : "failure")
            );
            return ResponseEntity.ok(result);
        });
    }
}
```

### 12.2 Health Checks
**Comprehensive Health Monitoring**: Implement health checks for all critical dependencies.

```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    
    @Autowired
    private DataSource dataSource;
    
    @Override
    public Health health() {
        try (Connection connection = dataSource.getConnection()) {
            if (connection.isValid(1)) {
                return Health.up()
                    .withDetail("database", "Available")
                    .withDetail("connection_pool", getPoolStatus())
                    .build();
            }
        } catch (SQLException e) {
            return Health.down()
                .withDetail("database", "Unavailable")
                .withException(e)
                .build();
        }
        return Health.down().withDetail("database", "Connection invalid").build();
    }
}
```

---

## 13. Enforcement and Tooling

### 13.1 Automated Code Quality Tools
**Required Tools for All Projects**:

**Linting and Formatting**:
- Java: Checkstyle, SpotBugs, PMD
- JavaScript/TypeScript: ESLint, Prettier
- Python: Flake8, Black, isort
- C#: StyleCop, FxCop
- C++: clang-format, clang-tidy, cppcheck

**Security Analysis**:
- OWASP Dependency Check
- SonarQube Security Hotspots
- Language-specific security linters

**Example ESLint Configuration**:
```json
{
  "extends": ["@company/eslint-config"],
  "rules": {
    "no-console": "error",
    "prefer-const": "error",
    "no-unused-vars": "error",
    "max-len": ["error", { "code": 120 }],
    "complexity": ["error", 10]
  }
}
```

**C++ clang-format Configuration**:
```yaml
# .clang-format
BasedOnStyle: Google
IndentWidth: 4
ColumnLimit: 120
PointerAlignment: Left
ReferenceAlignment: Left
AlwaysBreakTemplateDeclarations: Yes
BreakBeforeBraces: Attach
IncludeBlocks: Regroup
IncludeCategories:
  - Regex: '^<.*\.h>'
    Priority: 1
  - Regex: '^<.*>'
    Priority: 2
  - Regex: '.*'
    Priority: 3
SortIncludes: true
AllowShortFunctionsOnASingleLine: Empty
AllowShortIfStatementsOnASingleLine: false
```

**C++ clang-tidy Configuration**:
```yaml
# .clang-tidy
Checks: >
  *,
  -abseil-*,
  -altera-*,
  -fuchsia-*,
  -google-*,
  -zircon-*,
  -readability-magic-numbers,
  -cppcoreguidelines-avoid-magic-numbers,
  -modernize-use-trailing-return-type
WarningsAsErrors: >
  cppcoreguidelines-*,
  modernize-*,
  performance-*,
  readability-*
CheckOptions:
  - key: readability-identifier-naming.ClassCase
    value: CamelCase
  - key: readability-identifier-naming.FunctionCase
    value: camelBack
  - key: readability-identifier-naming.VariableCase
    value: lower_case
  - key: readability-identifier-naming.PrivateMemberSuffix
    value: _
```

### 13.2 Pre-commit Hooks
**Automated Quality Gates**: Use pre-commit hooks to enforce standards.

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: eslint
        name: eslint
        entry: npx eslint
        language: node
        files: \.(js|ts)$
        
      - id: unit-tests
        name: unit-tests
        entry: npm test
        language: node
        pass_filenames: false
        
      - id: security-scan
        name: security-scan
        entry: npm audit
        language: node
        pass_filenames: false
        
      # C++ specific hooks
      - id: clang-format
        name: clang-format
        entry: clang-format
        language: system
        files: \.(cpp|hpp|cc|h)$
        args: [-i, --style=file]
        
      - id: clang-tidy
        name: clang-tidy
        entry: clang-tidy
        language: system
        files: \.(cpp|cc)$
        args: [--config-file=.clang-tidy]
        
      - id: cppcheck
        name: cppcheck
        entry: cppcheck
        language: system
        files: \.(cpp|cc)$
        args: [--enable=all, --error-exitcode=1, --suppress=missingIncludeSystem]
```

**C++ CMake Integration for Quality Tools**:
```cmake
# CMakeLists.txt additions for code quality
find_program(CLANG_TIDY_EXE NAMES "clang-tidy")
if(CLANG_TIDY_EXE)
    set(CMAKE_CXX_CLANG_TIDY "${CLANG_TIDY_EXE};--config-file=${CMAKE_SOURCE_DIR}/.clang-tidy")
endif()

find_program(CLANG_FORMAT_EXE NAMES "clang-format")
if(CLANG_FORMAT_EXE)
    # Add custom target for formatting
    file(GLOB_RECURSE ALL_SOURCE_FILES src/*.cpp src/*.hpp include/*.hpp)
    add_custom_target(format
        COMMAND ${CLANG_FORMAT_EXE} -i ${ALL_SOURCE_FILES}
        COMMENT "Running clang-format"
    )
endif()

# Enable compiler warnings
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-Wall -Wextra -Wpedantic -Werror>
)
```

### 13.3 IDE Configuration
**Shared IDE Settings**: Maintain consistent IDE configurations across the team.

**VSCode Settings Example**:
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "editor.rulers": [120],
  "typescript.preferences.importModuleSpecifier": "relative",
  
  "C_Cpp.clang_format_style": "file",
  "C_Cpp.clang_format_fallbackStyle": "Google",
  "C_Cpp.codeAnalysis.clangTidy.enabled": true,
  "C_Cpp.codeAnalysis.clangTidy.path": "/usr/bin/clang-tidy",
  "C_Cpp.default.cppStandard": "c++17",
  "C_Cpp.default.intelliSenseMode": "gcc-x64"
}
```

**CLion/IntelliJ Settings for C++**:
```xml
<!-- Code Style Settings -->
<code_scheme name="Company Standard">
  <option name="RIGHT_MARGIN" value="120" />
  <codeStyleSettings language="ObjectiveC">
    <option name="INDENT_SIZE" value="4" />
    <option name="TAB_SIZE" value="4" />
    <option name="USE_TAB_CHARACTER" value="false" />
    <option name="KEEP_BLANK_LINES_IN_DECLARATIONS" value="1" />
    <option name="KEEP_BLANK_LINES_IN_CODE" value="1" />
    <option name="BRACE_STYLE" value="1" />
    <option name="CLASS_BRACE_STYLE" value="1" />
    <option name="METHOD_BRACE_STYLE" value="1" />
  </codeStyleSettings>
</code_scheme>
```

**Eclipse CDT Settings**:
```
# Code Style -> Formatter
org.eclipse.cdt.core.formatter.tabulation.char=space
org.eclipse.cdt.core.formatter.tabulation.size=4
org.eclipse.cdt.core.formatter.lineSplit=120
org.eclipse.cdt.core.formatter.brace_position_for_method_declaration=same_line
org.eclipse.cdt.core.formatter.brace_position_for_type_declaration=same_line
```

---

## 14. Continuous Improvement

### 14.1 Metrics and Feedback
**Track Code Quality Metrics**:
- Code review cycle time
- Defect density by module
- Technical debt accumulation
- Developer satisfaction with coding standards

### 14.2 Regular Review Process
**Quarterly Standards Review**:
- Evaluate effectiveness of current standards
- Incorporate new best practices and tooling
- Address developer feedback and pain points
- Update guidelines based on lessons learned

### 14.3 Training and Knowledge Sharing
**Ongoing Education**:
- Regular coding standards workshops
- Code review best practices training
- Security awareness sessions
- New technology evaluation and adoption

---

## 15. Appendix: Quick Reference

### 15.1 Common Code Smells to Avoid
- Long methods (>30 lines)
- Large classes (>300 lines)
- Duplicate code
- Long parameter lists (>5 parameters)
- Deeply nested conditions (>3 levels)
- Magic numbers and strings
- God objects (classes that do too much)

**C++ Specific Code Smells**:
- Raw pointer usage when smart pointers are appropriate
- Manual memory management instead of RAII
- Using `new`/`delete` instead of `make_unique`/`make_shared`
- Ignoring compiler warnings
- Using C-style casts instead of C++ casts
- Not using `const` where appropriate
- Using `using namespace std` in header files

### 15.2 Refactoring Checklist
When refactoring code:
- [ ] Maintain existing functionality
- [ ] Improve code readability
- [ ] Reduce complexity
- [ ] Update tests accordingly
- [ ] Update documentation
- [ ] Measure performance impact

---

*This coding style guide establishes the foundation for maintainable, secure, and high-quality enterprise software development. Regular adherence to these standards ensures consistent code quality across all projects and teams.*