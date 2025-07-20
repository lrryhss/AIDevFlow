# Enterprise Software Security Best Practices Guide

## Document Information
- **Version**: 1.0
- **Purpose**: Establish comprehensive security standards for enterprise software development
- **Audience**: Software Developers, Security Engineers, Technical Leads, DevOps Teams
- **Scope**: All programming languages and security domains in enterprise applications

---

## 1. Security Principles and Foundation

### 1.1 Core Security Principles
**Defense in Depth**: Implement multiple layers of security controls rather than relying on a single security measure.

**Principle of Least Privilege**: Grant users and systems only the minimum access rights needed to perform their functions.

**Fail Securely**: When systems fail, they should fail in a secure state that doesn't compromise security.

**Security by Design**: Integrate security considerations into every phase of the development lifecycle, not as an afterthought.

**Zero Trust Architecture**: Never trust, always verify - authenticate and authorize every request regardless of source.

### 1.2 Security Requirements Integration
**Requirement Traceability**: All security implementations must trace back to specific security requirements.

```
@requirement REQ-SEC-001 Implement authentication for all user access
@requirement REQ-SEC-002 Encrypt all data in transit using TLS 1.3
@requirement REQ-SEC-003 Hash passwords using bcrypt with minimum 12 rounds
```

---

## 2. Input Validation and Data Sanitization

### 2.1 Universal Input Validation Principles
**Validate All Inputs**: Every input from external sources must be validated before processing.

**Whitelist Over Blacklist**: Define what is allowed rather than what is forbidden.

**Fail Securely**: Invalid input should be rejected with minimal information disclosure.

**Canonicalization**: Normalize input data to prevent bypass attempts through encoding variations.

### 2.2 Language-Specific Input Validation

#### Java Input Validation
```java
/**
 * @requirement REQ-SEC-VAL-001 Validate all user inputs
 * @requirement REQ-SEC-VAL-002 Prevent injection attacks
 */
public class InputValidator {
    
    private static final Pattern EMAIL_PATTERN = 
        Pattern.compile("^[A-Za-z0-9+_.-]+@([A-Za-z0-9.-]+\\.[A-Za-z]{2,})$");
    
    private static final Pattern ALPHANUMERIC_PATTERN = 
        Pattern.compile("^[a-zA-Z0-9]+$");
    
    public static void validateEmail(String email) throws ValidationException {
        if (email == null || email.trim().isEmpty()) {
            throw new ValidationException("Email cannot be null or empty");
        }
        
        // Normalize input
        email = email.trim().toLowerCase();
        
        // Length validation
        if (email.length() > 254) {
            throw new ValidationException("Email too long");
        }
        
        // Format validation
        if (!EMAIL_PATTERN.matcher(email).matches()) {
            throw new ValidationException("Invalid email format");
        }
        
        // Additional security checks
        if (containsSuspiciousPatterns(email)) {
            throw new ValidationException("Email contains invalid characters");
        }
    }
    
    public static String sanitizeUserInput(String input) {
        if (input == null) return "";
        
        // Remove potential script tags and SQL injection patterns
        return input.replaceAll("<script[^>]*>.*?</script>", "")
                   .replaceAll("(?i)(javascript:|data:|vbscript:)", "")
                   .replaceAll("['\";<>\\(\\)\\{\\}\\[\\]]", "")
                   .trim();
    }
    
    public static void validateNumericInput(String input, int min, int max) 
            throws ValidationException {
        try {
            int value = Integer.parseInt(input);
            if (value < min || value > max) {
                throw new ValidationException(
                    String.format("Value must be between %d and %d", min, max));
            }
        } catch (NumberFormatException e) {
            throw new ValidationException("Invalid numeric format");
        }
    }
}

// ✅ Good - Parameterized queries prevent SQL injection
public class UserRepository {
    
    public User findUserByEmail(String email) {
        // Validate input first
        InputValidator.validateEmail(email);
        
        String sql = "SELECT * FROM users WHERE email = ? AND active = ?";
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            stmt.setString(1, email);
            stmt.setBoolean(2, true);
            
            ResultSet rs = stmt.executeQuery();
            return mapResultToUser(rs);
        } catch (SQLException e) {
            logger.error("Database error during user lookup", e);
            throw new DataAccessException("User lookup failed");
        }
    }
}

// ❌ Poor - SQL injection vulnerability
public User findUserBadExample(String email) {
    String sql = "SELECT * FROM users WHERE email = '" + email + "'";
    // This allows SQL injection attacks
}
```

#### C++ Input Validation
```cpp
/**
 * @brief Input validation utilities for secure C++ applications
 * @requirement REQ-SEC-VAL-001 Validate all external inputs
 * @requirement REQ-SEC-VAL-003 Prevent buffer overflow attacks
 */
class InputValidator {
private:
    static constexpr size_t MAX_EMAIL_LENGTH = 254;
    static constexpr size_t MAX_USERNAME_LENGTH = 50;
    
    static const std::regex email_pattern_;
    static const std::regex alphanumeric_pattern_;
    
public:
    static bool isValidEmail(const std::string& email) {
        if (email.empty() || email.length() > MAX_EMAIL_LENGTH) {
            return false;
        }
        
        return std::regex_match(email, email_pattern_);
    }
    
    static std::string sanitizeString(const std::string& input, size_t max_length) {
        if (input.empty()) {
            return "";
        }
        
        // Truncate to prevent buffer overflow
        std::string sanitized = input.substr(0, max_length);
        
        // Remove potentially dangerous characters
        sanitized.erase(
            std::remove_if(sanitized.begin(), sanitized.end(),
                [](char c) {
                    return c == '<' || c == '>' || c == '\'' || c == '"' ||
                           c == ';' || c == '&' || c == '|' || c == '`';
                }),
            sanitized.end());
        
        return sanitized;
    }
    
    static bool isValidNumericRange(const std::string& input, int min_val, int max_val) {
        try {
            int value = std::stoi(input);
            return value >= min_val && value <= max_val;
        } catch (const std::exception&) {
            return false;
        }
    }
    
    // Secure string handling to prevent buffer overflows
    static bool secureStringCopy(char* dest, size_t dest_size, 
                                 const char* src, size_t src_length) {
        if (dest == nullptr || src == nullptr || dest_size == 0) {
            return false;
        }
        
        size_t copy_length = std::min(src_length, dest_size - 1);
        std::memcpy(dest, src, copy_length);
        dest[copy_length] = '\0';
        
        return copy_length == src_length; // Returns false if truncated
    }
};

// ✅ Good - Safe database interaction with prepared statements
class DatabaseManager {
private:
    std::unique_ptr<sql::Connection> connection_;
    
public:
    std::optional<User> findUserByEmail(const std::string& email) {
        // Validate input
        if (!InputValidator::isValidEmail(email)) {
            throw std::invalid_argument("Invalid email format");
        }
        
        try {
            // Use prepared statements to prevent SQL injection
            std::unique_ptr<sql::PreparedStatement> stmt(
                connection_->prepareStatement(
                    "SELECT id, username, email FROM users WHERE email = ? AND active = ?"));
            
            stmt->setString(1, email);
            stmt->setBoolean(2, true);
            
            std::unique_ptr<sql::ResultSet> result(stmt->executeQuery());
            
            if (result->next()) {
                return User{
                    result->getInt("id"),
                    result->getString("username"),
                    result->getString("email")
                };
            }
            
            return std::nullopt;
            
        } catch (const sql::SQLException& e) {
            logger_.error("Database error: {}", e.what());
            throw DatabaseException("User lookup failed");
        }
    }
};

// ❌ Poor - Buffer overflow vulnerability
void badStringHandling(const char* user_input) {
    char buffer[100];
    strcpy(buffer, user_input); // Dangerous - no bounds checking
}

// ✅ Good - Safe string handling
void safeStringHandling(const char* user_input) {
    constexpr size_t BUFFER_SIZE = 100;
    char buffer[BUFFER_SIZE];
    
    if (user_input != nullptr) {
        strncpy(buffer, user_input, BUFFER_SIZE - 1);
        buffer[BUFFER_SIZE - 1] = '\0'; // Ensure null termination
    } else {
        buffer[0] = '\0';
    }
}
```

#### JavaScript/Node.js Input Validation
```javascript
/**
 * @fileoverview Secure input validation for Node.js applications
 * @requirement REQ-SEC-VAL-001 Validate all client inputs
 * @requirement REQ-SEC-VAL-004 Prevent XSS and injection attacks
 */

const validator = require('validator');
const DOMPurify = require('isomorphic-dompurify');

class InputValidator {
    
    static validateEmail(email) {
        if (!email || typeof email !== 'string') {
            throw new ValidationError('Email is required and must be a string');
        }
        
        // Normalize
        email = email.trim().toLowerCase();
        
        // Length check
        if (email.length > 254) {
            throw new ValidationError('Email too long');
        }
        
        // Format validation
        if (!validator.isEmail(email)) {
            throw new ValidationError('Invalid email format');
        }
        
        return email;
    }
    
    static sanitizeHtml(input) {
        if (typeof input !== 'string') {
            return '';
        }
        
        // Use DOMPurify to prevent XSS
        return DOMPurify.sanitize(input, {
            ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p', 'br'],
            ALLOWED_ATTR: []
        });
    }
    
    static validateAndSanitizeString(input, maxLength = 255) {
        if (typeof input !== 'string') {
            throw new ValidationError('Input must be a string');
        }
        
        // Trim whitespace
        input = input.trim();
        
        // Length validation
        if (input.length > maxLength) {
            throw new ValidationError(`Input too long. Maximum ${maxLength} characters allowed`);
        }
        
        // Remove potentially dangerous characters
        input = input.replace(/[<>'";&|`]/g, '');
        
        return input;
    }
    
    static validateNumericInput(input, min, max) {
        const num = parseFloat(input);
        
        if (isNaN(num)) {
            throw new ValidationError('Invalid numeric format');
        }
        
        if (num < min || num > max) {
            throw new ValidationError(`Value must be between ${min} and ${max}`);
        }
        
        return num;
    }
}

// ✅ Good - Secure database queries with parameterization
class UserRepository {
    
    async findUserByEmail(email) {
        // Validate input first
        const validatedEmail = InputValidator.validateEmail(email);
        
        // Use parameterized queries to prevent SQL injection
        const query = 'SELECT id, username, email FROM users WHERE email = $1 AND active = $2';
        const values = [validatedEmail, true];
        
        try {
            const result = await this.database.query(query, values);
            return result.rows[0] || null;
        } catch (error) {
            logger.error('Database error during user lookup', { error: error.message });
            throw new DatabaseError('User lookup failed');
        }
    }
}

// ✅ Good - Express.js middleware for input validation
const validateUserInput = (req, res, next) => {
    try {
        // Validate required fields
        if (req.body.email) {
            req.body.email = InputValidator.validateEmail(req.body.email);
        }
        
        if (req.body.username) {
            req.body.username = InputValidator.validateAndSanitizeString(req.body.username, 50);
        }
        
        if (req.body.age) {
            req.body.age = InputValidator.validateNumericInput(req.body.age, 13, 120);
        }
        
        next();
    } catch (error) {
        res.status(400).json({
            error: 'Validation failed',
            message: error.message
        });
    }
};

// ❌ Poor - Direct string concatenation in queries
async function badUserLookup(email) {
    const query = `SELECT * FROM users WHERE email = '${email}'`;
    return await database.query(query); // SQL injection vulnerability
}
```

#### Python Input Validation
```python
"""
Secure input validation utilities for Python applications.

@requirement REQ-SEC-VAL-001 Validate all external inputs
@requirement REQ-SEC-VAL-005 Prevent Python-specific injection attacks
"""

import re
import html
import bleach
from typing import Optional, Union
import logging

logger = logging.getLogger(__name__)

class ValidationError(Exception):
    """Custom exception for validation errors."""
    pass

class InputValidator:
    
    EMAIL_PATTERN = re.compile(r'^[A-Za-z0-9+_.-]+@([A-Za-z0-9.-]+\.[A-Za-z]{2,})$')
    ALPHANUMERIC_PATTERN = re.compile(r'^[a-zA-Z0-9]+$')
    
    @staticmethod
    def validate_email(email: str) -> str:
        """Validate email address format and length."""
        if not email or not isinstance(email, str):
            raise ValidationError("Email is required and must be a string")
        
        # Normalize
        email = email.strip().lower()
        
        # Length check
        if len(email) > 254:
            raise ValidationError("Email too long")
        
        # Format validation
        if not InputValidator.EMAIL_PATTERN.match(email):
            raise ValidationError("Invalid email format")
        
        return email
    
    @staticmethod
    def sanitize_html(input_str: str) -> str:
        """Sanitize HTML input to prevent XSS attacks."""
        if not isinstance(input_str, str):
            return ""
        
        # Allow only specific safe tags
        allowed_tags = ['b', 'i', 'em', 'strong', 'p', 'br']
        allowed_attributes = {}
        
        return bleach.clean(input_str, tags=allowed_tags, attributes=allowed_attributes)
    
    @staticmethod
    def validate_and_sanitize_string(input_str: str, max_length: int = 255) -> str:
        """Validate and sanitize string input."""
        if not isinstance(input_str, str):
            raise ValidationError("Input must be a string")
        
        # Trim whitespace
        input_str = input_str.strip()
        
        # Length validation
        if len(input_str) > max_length:
            raise ValidationError(f"Input too long. Maximum {max_length} characters allowed")
        
        # Remove potentially dangerous characters
        dangerous_chars = ['<', '>', "'", '"', ';', '&', '|', '`']
        for char in dangerous_chars:
            input_str = input_str.replace(char, '')
        
        return input_str
    
    @staticmethod
    def validate_numeric_input(input_val: Union[str, int, float], 
                             min_val: Union[int, float], 
                             max_val: Union[int, float]) -> Union[int, float]:
        """Validate numeric input within specified range."""
        try:
            if isinstance(input_val, str):
                # Try integer first, then float
                try:
                    num = int(input_val)
                except ValueError:
                    num = float(input_val)
            else:
                num = input_val
            
            if num < min_val or num > max_val:
                raise ValidationError(f"Value must be between {min_val} and {max_val}")
            
            return num
            
        except (ValueError, TypeError):
            raise ValidationError("Invalid numeric format")

# ✅ Good - Secure database queries with parameterization
class UserRepository:
    
    def __init__(self, database_connection):
        self.db = database_connection
    
    def find_user_by_email(self, email: str) -> Optional[dict]:
        """Find user by email with proper validation and parameterized queries."""
        # Validate input first
        validated_email = InputValidator.validate_email(email)
        
        # Use parameterized queries to prevent SQL injection
        query = "SELECT id, username, email FROM users WHERE email = %s AND active = %s"
        parameters = (validated_email, True)
        
        try:
            cursor = self.db.cursor()
            cursor.execute(query, parameters)
            result = cursor.fetchone()
            
            if result:
                return {
                    'id': result[0],
                    'username': result[1],
                    'email': result[2]
                }
            return None
            
        except Exception as e:
            logger.error(f"Database error during user lookup: {e}")
            raise DatabaseError("User lookup failed")
        finally:
            cursor.close()

# ✅ Good - Flask input validation decorator
from functools import wraps
from flask import request, jsonify

def validate_json_input(*expected_fields):
    """Decorator to validate JSON input for Flask routes."""
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            try:
                # Check if request contains JSON
                if not request.is_json:
                    return jsonify({'error': 'Request must contain JSON'}), 400
                
                data = request.get_json()
                
                # Validate expected fields
                for field in expected_fields:
                    if field not in data:
                        return jsonify({'error': f'Missing required field: {field}'}), 400
                
                # Validate specific fields
                if 'email' in data:
                    data['email'] = InputValidator.validate_email(data['email'])
                
                if 'username' in data:
                    data['username'] = InputValidator.validate_and_sanitize_string(
                        data['username'], 50)
                
                if 'age' in data:
                    data['age'] = InputValidator.validate_numeric_input(
                        data['age'], 13, 120)
                
                # Store validated data for use in route
                request.validated_json = data
                
                return f(*args, **kwargs)
                
            except ValidationError as e:
                return jsonify({'error': 'Validation failed', 'message': str(e)}), 400
            except Exception as e:
                logger.error(f"Unexpected validation error: {e}")
                return jsonify({'error': 'Internal validation error'}), 500
        
        return decorated_function
    return decorator

# Usage example
@app.route('/users', methods=['POST'])
@validate_json_input('email', 'username')
def create_user():
    data = request.validated_json
    # data is now validated and safe to use
    return jsonify({'message': 'User created successfully'})

# ❌ Poor - String formatting in SQL queries
def bad_user_lookup(email):
    query = f"SELECT * FROM users WHERE email = '{email}'"
    return database.execute(query)  # SQL injection vulnerability
```

---

## 3. Authentication and Authorization

### 3.1 Authentication Best Practices
**Strong Password Policies**: Enforce minimum complexity requirements and regular password changes.

**Multi-Factor Authentication**: Implement MFA for all administrative accounts and sensitive operations.

**Account Lockout**: Implement account lockout after multiple failed attempts to prevent brute force attacks.

**Session Management**: Use secure session tokens with proper expiration and invalidation.

### 3.2 Password Security Implementation

#### Secure Password Hashing (Java)
```java
/**
 * @requirement REQ-SEC-AUTH-001 Hash passwords securely using bcrypt
 * @requirement REQ-SEC-AUTH-002 Implement secure password validation
 */
public class PasswordService {
    
    private static final int BCRYPT_ROUNDS = 12; // Minimum recommended rounds
    private static final BCryptPasswordEncoder passwordEncoder = 
        new BCryptPasswordEncoder(BCRYPT_ROUNDS);
    
    // Password complexity requirements
    private static final int MIN_LENGTH = 12;
    private static final Pattern UPPERCASE_PATTERN = Pattern.compile(".*[A-Z].*");
    private static final Pattern LOWERCASE_PATTERN = Pattern.compile(".*[a-z].*");
    private static final Pattern DIGIT_PATTERN = Pattern.compile(".*[0-9].*");
    private static final Pattern SPECIAL_CHAR_PATTERN = Pattern.compile(".*[!@#$%^&*()_+\\-=\\[\\]{};':\"\\\\|,.<>/?].*");
    
    public static String hashPassword(String plainPassword) {
        validatePasswordComplexity(plainPassword);
        
        // Generate salt and hash password
        return passwordEncoder.encode(plainPassword);
    }
    
    public static boolean verifyPassword(String plainPassword, String hashedPassword) {
        try {
            return passwordEncoder.matches(plainPassword, hashedPassword);
        } catch (Exception e) {
            // Log error but don't reveal details
            logger.warn("Password verification failed", e);
            return false;
        }
    }
    
    public static void validatePasswordComplexity(String password) throws SecurityException {
        if (password == null || password.length() < MIN_LENGTH) {
            throw new SecurityException("Password must be at least " + MIN_LENGTH + " characters long");
        }
        
        if (!UPPERCASE_PATTERN.matcher(password).matches()) {
            throw new SecurityException("Password must contain at least one uppercase letter");
        }
        
        if (!LOWERCASE_PATTERN.matcher(password).matches()) {
            throw new SecurityException("Password must contain at least one lowercase letter");
        }
        
        if (!DIGIT_PATTERN.matcher(password).matches()) {
            throw new SecurityException("Password must contain at least one digit");
        }
        
        if (!SPECIAL_CHAR_PATTERN.matcher(password).matches()) {
            throw new SecurityException("Password must contain at least one special character");
        }
        
        // Check against common passwords
        if (isCommonPassword(password)) {
            throw new SecurityException("Password is too common, please choose a different one");
        }
    }
    
    private static boolean isCommonPassword(String password) {
        // Check against known common passwords list
        Set<String> commonPasswords = loadCommonPasswords();
        return commonPasswords.contains(password.toLowerCase());
    }
}

/**
 * @requirement REQ-SEC-AUTH-003 Implement secure session management
 */
public class SessionManager {
    
    private static final int SESSION_TIMEOUT_MINUTES = 30;
    private static final SecureRandom secureRandom = new SecureRandom();
    
    public static String generateSecureSessionToken() {
        byte[] tokenBytes = new byte[32];
        secureRandom.nextBytes(tokenBytes);
        return Base64.getUrlEncoder().withoutPadding().encodeToString(tokenBytes);
    }
    
    public static void invalidateSession(String sessionToken) {
        // Remove from session store
        sessionStore.remove(sessionToken);
        
        // Log session invalidation for audit
        auditLogger.info("Session invalidated", Map.of("sessionToken", maskToken(sessionToken)));
    }
    
    public static boolean isSessionValid(String sessionToken) {
        SessionData session = sessionStore.get(sessionToken);
        
        if (session == null) {
            return false;
        }
        
        // Check if session has expired
        if (session.isExpired()) {
            invalidateSession(sessionToken);
            return false;
        }
        
        // Update last access time
        session.updateLastAccess();
        return true;
    }
}
```

#### C++ Authentication
```cpp
/**
 * @brief Secure authentication implementation for C++ applications
 * @requirement REQ-SEC-AUTH-001 Implement secure password hashing
 * @requirement REQ-SEC-AUTH-004 Secure session token generation
 */
class AuthenticationService {
private:
    static constexpr int BCRYPT_ROUNDS = 12;
    static constexpr size_t TOKEN_LENGTH = 32;
    static constexpr int SESSION_TIMEOUT_MINUTES = 30;
    
    std::random_device random_device_;
    std::mt19937 generator_;
    std::uniform_int_distribution<> distribution_;
    
public:
    AuthenticationService() 
        : generator_(random_device_()),
          distribution_(0, 255) {}
    
    std::string hashPassword(const std::string& plain_password) {
        validatePasswordComplexity(plain_password);
        
        // Use bcrypt library for secure hashing
        char salt[BCRYPT_HASHSIZE];
        char hash[BCRYPT_HASHSIZE];
        
        // Generate salt
        bcrypt_gensalt(BCRYPT_ROUNDS, salt);
        
        // Hash password with salt
        bcrypt_hashpw(plain_password.c_str(), salt, hash);
        
        return std::string(hash);
    }
    
    bool verifyPassword(const std::string& plain_password, 
                       const std::string& hashed_password) {
        try {
            return bcrypt_checkpw(plain_password.c_str(), hashed_password.c_str()) == 0;
        } catch (const std::exception& e) {
            logger_.warn("Password verification failed: {}", e.what());
            return false;
        }
    }
    
    std::string generateSecureToken() {
        std::vector<unsigned char> token_bytes(TOKEN_LENGTH);
        
        // Generate cryptographically secure random bytes
        for (size_t i = 0; i < TOKEN_LENGTH; ++i) {
            token_bytes[i] = static_cast<unsigned char>(distribution_(generator_));
        }
        
        // Convert to base64 for safe transmission
        return base64_encode(token_bytes);
    }
    
private:
    void validatePasswordComplexity(const std::string& password) {
        constexpr int MIN_LENGTH = 12;
        
        if (password.length() < MIN_LENGTH) {
            throw SecurityException("Password must be at least " + 
                                  std::to_string(MIN_LENGTH) + " characters long");
        }
        
        bool has_upper = false, has_lower = false, has_digit = false, has_special = false;
        
        for (char c : password) {
            if (std::isupper(c)) has_upper = true;
            else if (std::islower(c)) has_lower = true;
            else if (std::isdigit(c)) has_digit = true;
            else if (std::ispunct(c)) has_special = true;
        }
        
        if (!has_upper) {
            throw SecurityException("Password must contain at least one uppercase letter");
        }
        if (!has_lower) {
            throw SecurityException("Password must contain at least one lowercase letter");
        }
        if (!has_digit) {
            throw SecurityException("Password must contain at least one digit");
        }
        if (!has_special) {
            throw SecurityException("Password must contain at least one special character");
        }
    }
    
    std::string base64_encode(const std::vector<unsigned char>& data) {
        // Base64 encoding implementation
        const std::string chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
        std::string result;
        
        int val = 0, valb = -6;
        for (unsigned char c : data) {
            val = (val << 8) + c;
            valb += 8;
            while (valb >= 0) {
                result.push_back(chars[(val >> valb) & 0x3F]);
                valb -= 6;
            }
        }
        if (valb > -6) {
            result.push_back(chars[((val << 8) >> (valb + 8)) & 0x3F]);
        }
        while (result.size() % 4) {
            result.push_back('=');
        }
        return result;
    }
};

/**
 * @brief Secure session management
 */
class SessionManager {
private:
    std::unordered_map<std::string, SessionData> sessions_;
    std::shared_mutex sessions_mutex_;
    
public:
    std::string createSession(const std::string& user_id) {
        AuthenticationService auth_service;
        std::string session_token = auth_service.generateSecureToken();
        
        SessionData session{
            user_id,
            std::chrono::system_clock::now(),
            std::chrono::system_clock::now() + std::chrono::minutes(30)
        };
        
        {
            std::unique_lock lock(sessions_mutex_);
            sessions_[session_token] = session;
        }
        
        return session_token;
    }
    
    bool validateSession(const std::string& session_token) {
        std::shared_lock lock(sessions_mutex_);
        
        auto it = sessions_.find(session_token);
        if (it == sessions_.end()) {
            return false;
        }
        
        const auto& session = it->second;
        auto now = std::chrono::system_clock::now();
        
        return now < session.expires_at;
    }
    
    void invalidateSession(const std::string& session_token) {
        std::unique_lock lock(sessions_mutex_);
        sessions_.erase(session_token);
    }
};
```

---

## 4. Cryptography and Data Protection

### 4.1 Encryption Standards
**Data at Rest**: Use AES-256 encryption for stored sensitive data.

**Data in Transit**: Use TLS 1.3 for all network communications.

**Key Management**: Implement proper key rotation and secure key storage.

**Digital Signatures**: Use RSA-2048 or ECDSA P-256 for digital signatures.

### 4.2 Encryption Implementation Examples

#### Java Encryption
```java
/**
 * @requirement REQ-SEC-CRYPTO-001 Encrypt sensitive data using AES-256
 * @requirement REQ-SEC-CRYPTO-002 Implement secure key management
 */
public class EncryptionService {
    
    private static final String ALGORITHM = "AES";
    private static final String TRANSFORMATION = "AES/GCM/NoPadding";
    private static final int GCM_IV_LENGTH = 12;
    private static final int GCM_TAG_LENGTH = 16;
    
    private final SecretKey encryptionKey;
    private final SecureRandom secureRandom;
    
    public EncryptionService(SecretKey key) {
        this.encryptionKey = key;
        this.secureRandom = new SecureRandom();
    }
    
    public EncryptedData encrypt(String plaintext) throws EncryptionException {
        try {
            Cipher cipher = Cipher.getInstance(TRANSFORMATION);
            
            // Generate random IV
            byte[] iv = new byte[GCM_IV_LENGTH];
            secureRandom.nextBytes(iv);
            GCMParameterSpec gcmSpec = new GCMParameterSpec(GCM_TAG_LENGTH * 8, iv);
            
            cipher.init(Cipher.ENCRYPT_MODE, encryptionKey, gcmSpec);
            
            byte[] ciphertext = cipher.doFinal(plaintext.getBytes(StandardCharsets.UTF_8));
            
            return new EncryptedData(
                Base64.getEncoder().encodeToString(ciphertext),
                Base64.getEncoder().encodeToString(iv)
            );
            
        } catch (Exception e) {
            logger.error("Encryption failed", e);
            throw new EncryptionException("Failed to encrypt data", e);
        }
    }
    
    public String decrypt(EncryptedData encryptedData) throws EncryptionException {
        try {
            Cipher cipher = Cipher.getInstance(TRANSFORMATION);
            
            byte[] iv = Base64.getDecoder().decode(encryptedData.getIv());
            byte[] ciphertext = Base64.getDecoder().decode(encryptedData.getCiphertext());
            
            GCMParameterSpec gcmSpec = new GCMParameterSpec(GCM_TAG_LENGTH * 8, iv);
            cipher.init(Cipher.DECRYPT_MODE, encryptionKey, gcmSpec);
            
            byte[] plaintext = cipher.doFinal(ciphertext);
            
            return new String(plaintext, StandardCharsets.UTF_8);
            
        } catch (Exception e) {
            logger.error("Decryption failed", e);
            throw new EncryptionException("Failed to decrypt data", e);
        }
    }
    
    public static SecretKey generateKey() throws NoSuchAlgorithmException {
        KeyGenerator keyGenerator = KeyGenerator.getInstance(ALGORITHM);
        keyGenerator.init(256); // AES-256
        return keyGenerator.generateKey();
    }
}

/**
 * @requirement REQ-SEC-CRYPTO-003 Implement secure key storage and rotation
 */
public class KeyManager {
    
    private final KeyStore keyStore;
    private final char[] keyStorePassword;
    
    public KeyManager(String keyStorePath, char[] password) throws Exception {
        this.keyStorePassword = password.clone();
        this.keyStore = KeyStore.getInstance("PKCS12");
        
        try (FileInputStream fis = new FileInputStream(keyStorePath)) {
            keyStore.load(fis, password);
        }
    }
    
    public SecretKey getEncryptionKey(String alias) throws Exception {
        return (SecretKey) keyStore.getKey(alias, keyStorePassword);
    }
    
    public void rotateKey(String alias) throws Exception {
        // Generate new key
        SecretKey newKey = EncryptionService.generateKey();
        
        // Store new key with versioned alias
        String newAlias = alias + "_" + System.currentTimeMillis();
        keyStore.setKeyEntry(newAlias, newKey, keyStorePassword, null);
        
        // Save keystore
        saveKeyStore();
        
        logger.info("Key rotated successfully", Map.of("alias", alias, "newAlias", newAlias));
    }
    
    private void saveKeyStore() throws Exception {
        try (FileOutputStream fos = new FileOutputStream("keystore.p12")) {
            keyStore.store(fos, keyStorePassword);
        }
    }
}
```

#### Node.js Encryption
```javascript
/**
 * @fileoverview Secure encryption service for Node.js applications
 * @requirement REQ-SEC-CRYPTO-001 Implement AES-256-GCM encryption
 * @requirement REQ-SEC-CRYPTO-004 Secure random number generation
 */

const crypto = require('crypto');
const fs = require('fs').promises;

class EncryptionService {
    
    constructor(encryptionKey) {
        this.algorithm = 'aes-256-gcm';
        this.keyLength = 32; // 256 bits
        this.ivLength = 12;  // 96 bits for GCM
        this.tagLength = 16; // 128 bits
        
        if (!encryptionKey || encryptionKey.length !== this.keyLength) {
            throw new Error('Invalid encryption key length. Must be 32 bytes for AES-256.');
        }
        
        this.encryptionKey = encryptionKey;
    }
    
    encrypt(plaintext) {
        try {
            // Generate random IV
            const iv = crypto.randomBytes(this.ivLength);
            
            // Create cipher
            const cipher = crypto.createCipher(this.algorithm, this.encryptionKey);
            cipher.setIV(iv);
            
            // Encrypt data
            let ciphertext = cipher.update(plaintext, 'utf8', 'binary');
            ciphertext += cipher.final('binary');
            
            // Get authentication tag
            const tag = cipher.getAuthTag();
            
            return {
                ciphertext: Buffer.from(ciphertext, 'binary').toString('base64'),
                iv: iv.toString('base64'),
                tag: tag.toString('base64')
            };
            
        } catch (error) {
            logger.error('Encryption failed', { error: error.message });
            throw new Error('Failed to encrypt data');
        }
    }
    
    decrypt(encryptedData) {
        try {
            const { ciphertext, iv, tag } = encryptedData;
            
            // Convert from base64
            const ciphertextBuffer = Buffer.from(ciphertext, 'base64');
            const ivBuffer = Buffer.from(iv, 'base64');
            const tagBuffer = Buffer.from(tag, 'base64');
            
            // Create decipher
            const decipher = crypto.createDecipher(this.algorithm, this.encryptionKey);
            decipher.setIV(ivBuffer);
            decipher.setAuthTag(tagBuffer);
            
            // Decrypt data
            let plaintext = decipher.update(ciphertextBuffer, 'binary', 'utf8');
            plaintext += decipher.final('utf8');
            
            return plaintext;
            
        } catch (error) {
            logger.error('Decryption failed', { error: error.message });
            throw new Error('Failed to decrypt data');
        }
    }
    
    static generateKey() {
        return crypto.randomBytes(32); // 256-bit key
    }
    
    static generateSecureToken(length = 32) {
        return crypto.randomBytes(length).toString('base64url');
    }
}

/**
 * Key management utilities
 */
class KeyManager {
    
    constructor(keyFilePath) {
        this.keyFilePath = keyFilePath;
    }
    
    async loadKey() {
        try {
            const keyData = await fs.readFile(this.keyFilePath);
            return JSON.parse(keyData.toString());
        } catch (error) {
            throw new Error('Failed to load encryption key');
        }
    }
    
    async saveKey(keyData) {
        try {
            const keyJson = JSON.stringify(keyData, null, 2);
            await fs.writeFile(this.keyFilePath, keyJson, { mode: 0o600 });
        } catch (error) {
            throw new Error('Failed to save encryption key');
        }
    }
    
    async rotateKey() {
        const newKey = EncryptionService.generateKey();
        const keyData = {
            key: newKey.toString('base64'),
            created: new Date().toISOString(),
            version: Date.now()
        };
        
        await this.saveKey(keyData);
        
        logger.info('Encryption key rotated successfully', { 
            version: keyData.version 
        });
        
        return newKey;
    }
}

module.exports = { EncryptionService, KeyManager };
```

---

## 5. Secure Communication

### 5.1 TLS/SSL Best Practices
**Minimum TLS Version**: Use TLS 1.3, disable older versions.

**Certificate Validation**: Always validate server certificates and implement certificate pinning for mobile apps.

**Perfect Forward Secrecy**: Ensure cipher suites support perfect forward secrecy.

**HSTS**: Implement HTTP Strict Transport Security headers.

### 5.2 API Security

#### Secure REST API Implementation
```java
/**
 * @requirement REQ-SEC-API-001 Implement secure API authentication
 * @requirement REQ-SEC-API-002 Rate limiting and request validation
 */
@RestController
@RequestMapping("/api/v1")
public class SecureApiController {
    
    @Autowired
    private JwtTokenValidator tokenValidator;
    
    @Autowired
    private RateLimitingService rateLimitingService;
    
    @PostMapping("/payments")
    @PreAuthorize("hasRole('PAYMENT_PROCESSOR')")
    public ResponseEntity<PaymentResult> processPayment(
            @RequestHeader("Authorization") String authHeader,
            @Valid @RequestBody PaymentRequest request,
            HttpServletRequest httpRequest) {
        
        try {
            // Rate limiting check
            String clientIp = getClientIpAddress(httpRequest);
            if (!rateLimitingService.isRequestAllowed(clientIp)) {
                return ResponseEntity.status(HttpStatus.TOO_MANY_REQUESTS)
                    .header("Retry-After", "60")
                    .build();
            }
            
            // Validate JWT token
            String token = extractToken(authHeader);
            JwtClaims claims = tokenValidator.validateToken(token);
            
            // Additional authorization check
            if (!hasPaymentPermission(claims, request.getCustomerId())) {
                return ResponseEntity.status(HttpStatus.FORBIDDEN).build();
            }
            
            // Process payment with validated input
            PaymentResult result = paymentService.processPayment(request);
            
            // Audit log
            auditLogger.logApiAccess(claims.getSubject(), "PAYMENT_PROCESS", 
                                   request.getCustomerId(), "SUCCESS");
            
            return ResponseEntity.ok(result);
            
        } catch (SecurityException e) {
            auditLogger.logSecurityEvent("UNAUTHORIZED_API_ACCESS", 
                                        Map.of("ip", clientIp, "error", e.getMessage()));
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
        }
    }
    
    private String getClientIpAddress(HttpServletRequest request) {
        String xForwardedFor = request.getHeader("X-Forwarded-For");
        if (xForwardedFor != null && !xForwardedFor.isEmpty()) {
            return xForwardedFor.split(",")[0].trim();
        }
        return request.getRemoteAddr();
    }
    
    private String extractToken(String authHeader) throws SecurityException {
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            throw new SecurityException("Invalid authorization header");
        }
        return authHeader.substring(7);
    }
}

/**
 * @requirement REQ-SEC-API-003 Implement request/response security headers
 */
@Component
public class SecurityHeadersFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        
        // Security headers
        httpResponse.setHeader("X-Content-Type-Options", "nosniff");
        httpResponse.setHeader("X-Frame-Options", "DENY");
        httpResponse.setHeader("X-XSS-Protection", "1; mode=block");
        httpResponse.setHeader("Strict-Transport-Security", 
                              "max-age=31536000; includeSubDomains; preload");
        httpResponse.setHeader("Content-Security-Policy", 
                              "default-src 'self'; script-src 'self'; object-src 'none';");
        httpResponse.setHeader("Referrer-Policy", "strict-origin-when-cross-origin");
        
        // Remove server information
        httpResponse.setHeader("Server", "");
        
        chain.doFilter(request, response);
    }
}
```

---

## 6. Error Handling and Logging Security

### 6.1 Secure Error Handling
**Information Disclosure Prevention**: Never expose internal system details in error messages.

**Consistent Error Responses**: Use standardized error formats that don't leak implementation details.

**Logging vs User Messages**: Log detailed errors internally, show generic messages to users.

### 6.2 Security Logging Best Practices

```java
/**
 * @requirement REQ-SEC-LOG-001 Implement comprehensive security logging
 * @requirement REQ-SEC-LOG-002 Prevent sensitive data exposure in logs
 */
public class SecurityAuditLogger {
    
    private static final Logger securityLogger = LoggerFactory.getLogger("SECURITY");
    private static final Logger auditLogger = LoggerFactory.getLogger("AUDIT");
    
    public void logAuthenticationSuccess(String userId, String ipAddress, String userAgent) {
        auditLogger.info("Authentication successful", 
            Map.of(
                "event", "AUTH_SUCCESS",
                "userId", userId,
                "ipAddress", maskIpAddress(ipAddress),
                "userAgent", sanitizeUserAgent(userAgent),
                "timestamp", Instant.now().toString()
            ));
    }
    
    public void logAuthenticationFailure(String attemptedUsername, String ipAddress, 
                                       String failureReason) {
        securityLogger.warn("Authentication failed",
            Map.of(
                "event", "AUTH_FAILURE",
                "username", maskUsername(attemptedUsername),
                "ipAddress", maskIpAddress(ipAddress),
                "reason", failureReason,
                "timestamp", Instant.now().toString()
            ));
    }
    
    public void logSuspiciousActivity(String userId, String activity, 
                                    Map<String, Object> context) {
        securityLogger.error("Suspicious activity detected",
            Map.of(
                "event", "SUSPICIOUS_ACTIVITY",
                "userId", userId,
                "activity", activity,
                "context", sanitizeContext(context),
                "timestamp", Instant.now().toString()
            ));
        
        // Also trigger security alert
        securityAlertService.triggerAlert("SUSPICIOUS_ACTIVITY", userId, activity);
    }
    
    public void logDataAccess(String userId, String resource, String operation) {
        auditLogger.info("Data access",
            Map.of(
                "event", "DATA_ACCESS",
                "userId", userId,
                "resource", resource,
                "operation", operation,
                "timestamp", Instant.now().toString()
            ));
    }
    
    // Security-conscious data masking methods
    private String maskUsername(String username) {
        if (username == null || username.length() <= 2) {
            return "***";
        }
        return username.substring(0, 2) + "***";
    }
    
    private String maskIpAddress(String ipAddress) {
        if (ipAddress == null) return "***";
        
        String[] parts = ipAddress.split("\\.");
        if (parts.length == 4) {
            return parts[0] + "." + parts[1] + ".***." + "***";
        }
        return "***";
    }
    
    private String sanitizeUserAgent(String userAgent) {
        if (userAgent == null) return "";
        
        // Remove potentially sensitive information
        return userAgent.replaceAll("(?i)(password|token|key|secret)=[^\\s;]+", "$1=***");
    }
    
    private Map<String, Object> sanitizeContext(Map<String, Object> context) {
        Map<String, Object> sanitized = new HashMap<>();
        
        for (Map.Entry<String, Object> entry : context.entrySet()) {
            String key = entry.getKey().toLowerCase();
            Object value = entry.getValue();
            
            // Mask sensitive fields
            if (key.contains("password") || key.contains("token") || 
                key.contains("secret") || key.contains("key")) {
                sanitized.put(entry.getKey(), "***");
            } else if (value instanceof String) {
                sanitized.put(entry.getKey(), sanitizeString((String) value));
            } else {
                sanitized.put(entry.getKey(), value);
            }
        }
        
        return sanitized;
    }
    
    private String sanitizeString(String value) {
        if (value == null) return null;
        
        // Remove potential sensitive patterns
        return value.replaceAll("(?i)(password|token|key|secret)\\s*[=:]\\s*[^\\s;]+", 
                               "$1=***");
    }
}
```

---

## 7. Dependency and Supply Chain Security

### 7.1 Dependency Management
**Vulnerability Scanning**: Regularly scan dependencies for known vulnerabilities.

**Version Pinning**: Pin dependency versions and update deliberately.

**License Compliance**: Ensure all dependencies have compatible licenses.

**Minimal Dependencies**: Only include dependencies that are absolutely necessary.

### 7.2 Automated Security Scanning

```yaml
# Example GitHub Actions workflow for security scanning
name: Security Scan

on: [push, pull_request]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Run Dependency Check
      uses: dependency-check/Dependency-Check_Action@main
      with:
        project: 'enterprise-app'
        path: '.'
        format: 'ALL'
        
    - name: Run SAST Scan
      uses: github/codeql-action/analyze@v2
      with:
        languages: java, javascript, cpp
        
    - name: Run Container Security Scan
      run: |
        docker build -t app:latest .
        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
          aquasec/trivy image app:latest
          
    - name: Check for Secrets
      uses: trufflesecurity/trufflehog@main
      with:
        path: ./
        base: main
        head: HEAD
```

---

## 8. Security Testing

### 8.1 Security Test Categories
**Unit Security Tests**: Test individual security functions (encryption, validation).

**Integration Security Tests**: Test security across system boundaries.

**Penetration Testing**: Regular external security assessments.

**Security Regression Tests**: Ensure security fixes don't break.

### 8.2 Security Test Examples

```java
/**
 * Security-focused unit tests
 * @requirement REQ-SEC-TEST-001 Implement comprehensive security testing
 */
public class SecurityTest {
    
    @Test
    public void shouldRejectWeakPasswords() {
        PasswordService passwordService = new PasswordService();
        
        // Test various weak password patterns
        String[] weakPasswords = {
            "password123",      // Common pattern
            "12345678",         // All digits
            "abcdefgh",         // All lowercase
            "PASSWORD",         // All uppercase
            "pass",             // Too short
            "P@ssw0rd"          // Common substitution pattern
        };
        
        for (String weakPassword : weakPasswords) {
            assertThatThrownBy(() -> passwordService.validatePasswordComplexity(weakPassword))
                .isInstanceOf(SecurityException.class)
                .hasMessageContaining("Password");
        }
    }
    
    @Test
    public void shouldPreventSqlInjectionInUserLookup() {
        UserRepository userRepository = new UserRepository(testDatabase);
        
        String[] sqlInjectionAttempts = {
            "admin'; DROP TABLE users; --",
            "' OR '1'='1",
            "admin' UNION SELECT * FROM sensitive_data --",
            "'; INSERT INTO users (username) VALUES ('hacker'); --"
        };
        
        for (String maliciousInput : sqlInjectionAttempts) {
            // Should either throw validation exception or return null safely
            assertThatCode(() -> {
                User result = userRepository.findUserByEmail(maliciousInput);
                assertThat(result).isNull(); // Should not find user with malicious input
            }).doesNotThrowAnyException();
        }
        
        // Verify database integrity
        assertThat(testDatabase.countTables()).isEqualTo(EXPECTED_TABLE_COUNT);
    }
    
    @Test
    public void shouldPreventXssInUserInput() {
        InputValidator validator = new InputValidator();
        
        String[] xssAttempts = {
            "<script>alert('XSS')</script>",
            "javascript:alert('XSS')",
            "<img src=x onerror=alert('XSS')>",
            "<iframe src='javascript:alert(\"XSS\")'></iframe>"
        };
        
        for (String xssAttempt : xssAttempts) {
            String sanitized = validator.sanitizeHtml(xssAttempt);
            
            // Verify no executable code remains
            assertThat(sanitized)
                .doesNotContain("<script")
                .doesNotContain("javascript:")
                .doesNotContain("onerror=")
                .doesNotContain("<iframe");
        }
    }
    
    @Test
    public void shouldEnforceRateLimiting() throws InterruptedException {
        RateLimitingService rateLimiter = new RateLimitingService();
        String clientIp = "192.168.1.100";
        
        // Make requests up to the limit
        for (int i = 0; i < RATE_LIMIT_PER_MINUTE; i++) {
            assertThat(rateLimiter.isRequestAllowed(clientIp)).isTrue();
        }
        
        // Next request should be blocked
        assertThat(rateLimiter.isRequestAllowed(clientIp)).isFalse();
        
        // Wait for rate limit window to reset
        Thread.sleep(61000); // Wait 61 seconds
        
        // Should allow requests again
        assertThat(rateLimiter.isRequestAllowed(clientIp)).isTrue();
    }
}
```

---

## 9. Deployment and Infrastructure Security

### 9.1 Container Security
**Minimal Base Images**: Use minimal, security-hardened base images.

**No Root Execution**: Run containers as non-root users.

**Secret Management**: Use proper secret management, never embed secrets in images.

**Regular Updates**: Keep base images and dependencies updated.

### 9.2 Secure Deployment Configuration

```dockerfile
# Secure Dockerfile example
FROM eclipse-temurin:17-jre-alpine

# Create non-root user
RUN addgroup -g 1001 -S appgroup && \
    adduser -u 1001 -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Copy application files
COPY --chown=appuser:appgroup target/app.jar app.jar

# Switch to non-root user
USER appuser

# Security configurations
ENV JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom \
               -Dfile.encoding=UTF-8 \
               -Djava.awt.headless=true \
               -XX:+UseContainerSupport \
               -XX:MaxRAMPercentage=75.0"

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1

# Expose port (non-privileged)
EXPOSE 8080

# Run application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```yaml
# Kubernetes security configuration
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secure-app
spec:
  template:
    spec:
      serviceAccountName: app-service-account
      securityContext:
        runAsNonRoot: true
        runAsUser: 1001
        runAsGroup: 1001
        fsGroup: 1001
        seccompProfile:
          type: RuntimeDefault
      containers:
      - name: app
        image: myregistry/secure-app:latest
        securityContext:
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          runAsNonRoot: true
          runAsUser: 1001
          capabilities:
            drop:
            - ALL
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
          requests:
            memory: "256Mi"
            cpu: "250m"
        env:
        - name: DATABASE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
        volumeMounts:
        - name: tmp-volume
          mountPath: /tmp
        - name: logs-volume
          mountPath: /app/logs
      volumes:
      - name: tmp-volume
        emptyDir: {}
      - name: logs-volume
        emptyDir: {}
```

---

## 10. Incident Response and Security Monitoring

### 10.1 Security Monitoring
**Real-time Alerting**: Implement automated alerts for security events.

**Anomaly Detection**: Monitor for unusual patterns in user behavior and system access.

**Log Correlation**: Correlate logs across different systems to detect complex attacks.

**Threat Intelligence**: Integrate with threat intelligence feeds.

### 10.2 Incident Response Implementation

```java
/**
 * @requirement REQ-SEC-MON-001 Implement security incident detection and response
 */
@Component
public class SecurityIncidentHandler {
    
    private final NotificationService notificationService;
    private final ThreatIntelligenceService threatIntelService;
    
    @EventListener
    public void handleSuspiciousActivity(SuspiciousActivityEvent event) {
        SecurityIncident incident = createIncident(event);
        
        // Immediate response actions
        if (incident.getSeverity() == Severity.CRITICAL) {
            // Automatically block the source
            firewallService.blockIpAddress(event.getSourceIp(), Duration.ofHours(24));
            
            // Invalidate all sessions for the affected user
            if (event.getUserId() != null) {
                sessionManager.invalidateAllUserSessions(event.getUserId());
            }
            
            // Send immediate alert
            notificationService.sendCriticalAlert(
                "Critical security incident detected",
                incident.getDescription(),
                SecurityTeam.INCIDENT_RESPONSE
            );
        }
        
        // Log the incident
        securityLogger.logIncident(incident);
        
        // Store for investigation
        incidentRepository.save(incident);
        
        // Check against threat intelligence
        enrichWithThreatIntelligence(incident);
    }
    
    @EventListener
    public void handleFailedAuthentication(AuthenticationFailureEvent event) {
        String sourceIp = event.getSourceIp();
        String username = event.getUsername();
        
        // Track failed attempts
        int failureCount = authenticationMetrics.incrementFailureCount(sourceIp, username);
        
        // Progressive response based on failure count
        if (failureCount >= 3) {
            // Lock the account
            userAccountService.lockAccount(username, Duration.ofMinutes(15));
            
            if (failureCount >= 10) {
                // Potential brute force attack
                firewallService.blockIpAddress(sourceIp, Duration.ofHours(1));
                
                SecurityIncident incident = SecurityIncident.builder()
                    .type(IncidentType.BRUTE_FORCE_ATTACK)
                    .severity(Severity.HIGH)
                    .sourceIp(sourceIp)
                    .targetUsername(username)
                    .description("Potential brute force attack detected")
                    .build();
                
                handleSecurityIncident(incident);
            }
        }
    }
    
    private void enrichWithThreatIntelligence(SecurityIncident incident) {
        CompletableFuture.supplyAsync(() -> {
            try {
                ThreatIntelligenceData threatData = 
                    threatIntelService.lookupIpAddress(incident.getSourceIp());
                
                if (threatData.isMalicious()) {
                    incident.addThreatIntelligence(threatData);
                    incident.increaseSeverity();
                    
                    // Take additional protective measures
                    firewallService.blockIpAddress(
                        incident.getSourceIp(), 
                        Duration.ofDays(7)
                    );
                }
                
                return incident;
            } catch (Exception e) {
                logger.warn("Failed to enrich incident with threat intelligence", e);
                return incident;
            }
        }).thenAccept(enrichedIncident -> {
            incidentRepository.update(enrichedIncident);
        });
    }
}
```

---

## 11. Compliance and Regulatory Requirements

### 11.1 Common Compliance Frameworks
**GDPR**: Data protection and privacy regulations.

**PCI DSS**: Payment card industry security standards.

**SOX**: Financial reporting and internal controls.

**HIPAA**: Healthcare information protection.

**ISO 27001**: Information security management systems.

### 11.2 Compliance Implementation Examples

```java
/**
 * @requirement REQ-COMP-GDPR-001 Implement GDPR data subject rights
 * @requirement REQ-COMP-GDPR-002 Data retention and deletion policies
 */
@Service
public class DataPrivacyService {
    
    @Autowired
    private PersonalDataRepository personalDataRepository;
    
    @Autowired
    private AuditLogger auditLogger;
    
    /**
     * GDPR Article 15 - Right of access by the data subject
     */
    public PersonalDataExport exportPersonalData(String userId, String requestId) {
        auditLogger.logDataSubjectRequest("DATA_EXPORT", userId, requestId);
        
        try {
            PersonalDataExport export = PersonalDataExport.builder()
                .userId(userId)
                .requestId(requestId)
                .requestDate(Instant.now())
                .build();
            
            // Collect all personal data
            export.addUserProfile(userService.getUserProfile(userId));
            export.addTransactionHistory(paymentService.getUserTransactions(userId));
            export.addLoginHistory(authService.getUserLoginHistory(userId));
            export.addPreferences(preferenceService.getUserPreferences(userId));
            
            // Create secure download link
            String downloadToken = secureTokenService.generateDownloadToken(userId, requestId);
            export.setDownloadToken(downloadToken);
            
            auditLogger.logDataExport(userId, requestId, export.getDataCategories());
            
            return export;
            
        } catch (Exception e) {
            auditLogger.logDataExportFailure(userId, requestId, e.getMessage());
            throw new DataPrivacyException("Failed to export personal data", e);
        }
    }
    
    /**
     * GDPR Article 17 - Right to erasure (right to be forgotten)
     */
    public DeletionResult deletePersonalData(String userId, String requestId, 
                                           DeletionReason reason) {
        auditLogger.logDataSubjectRequest("DATA_DELETION", userId, requestId);
        
        try {
            DeletionResult result = DeletionResult.builder()
                .userId(userId)
                .requestId(requestId)
                .deletionReason(reason)
                .initiatedAt(Instant.now())
                .build();
            
            // Check if deletion is legally permissible
            if (!isDeletionPermitted(userId, reason)) {
                result.setStatus(DeletionStatus.REJECTED);
                result.setRejectionReason("Legal obligation to retain data");
                return result;
            }
            
            // Perform systematic data deletion
            List<String> deletedCategories = new ArrayList<>();
            
            // Delete user profile data
            userService.deleteUserProfile(userId);
            deletedCategories.add("USER_PROFILE");
            
            // Delete preference data
            preferenceService.deleteUserPreferences(userId);
            deletedCategories.add("USER_PREFERENCES");
            
            // Delete marketing data
            marketingService.deleteUserMarketingData(userId);
            deletedCategories.add("MARKETING_DATA");
            
            // Anonymize transaction data (retain for legal compliance)
            paymentService.anonymizeUserTransactions(userId);
            deletedCategories.add("TRANSACTION_DATA_ANONYMIZED");
            
            // Delete authentication data
            authService.deleteUserAuthData(userId);
            deletedCategories.add("AUTHENTICATION_DATA");
            
            // Notify third-party processors
            notifyThirdPartyProcessors(userId, requestId);
            
            result.setDeletedCategories(deletedCategories);
            result.setStatus(DeletionStatus.COMPLETED);
            result.setCompletedAt(Instant.now());
            
            auditLogger.logDataDeletion(userId, requestId, deletedCategories);
            
            return result;
            
        } catch (Exception e) {
            auditLogger.logDataDeletionFailure(userId, requestId, e.getMessage());
            throw new DataPrivacyException("Failed to delete personal data", e);
        }
    }
    
    /**
     * Data retention policy enforcement
     */
    @Scheduled(cron = "0 0 2 * * ?") // Daily at 2 AM
    public void enforceDataRetentionPolicies() {
        auditLogger.logDataRetentionEnforcement();
        
        try {
            // Delete expired session data (30 days)
            int deletedSessions = sessionService.deleteExpiredSessions(Duration.ofDays(30));
            
            // Delete expired audit logs (7 years for financial data)
            int deletedAuditLogs = auditService.deleteExpiredAuditLogs(Duration.ofDays(2555));
            
            // Anonymize old transaction data (10 years retention)
            int anonymizedTransactions = paymentService.anonymizeOldTransactions(Duration.ofDays(3650));
            
            auditLogger.logDataRetentionStats(deletedSessions, deletedAuditLogs, anonymizedTransactions);
            
        } catch (Exception e) {
            auditLogger.logDataRetentionFailure(e.getMessage());
            logger.error("Data retention policy enforcement failed", e);
        }
    }
    
    private boolean isDeletionPermitted(String userId, DeletionReason reason) {
        // Check legal obligations to retain data
        if (paymentService.hasOngoingLegalHolds(userId)) {
            return false;
        }
        
        if (complianceService.hasRegulatoryRetentionRequirement(userId)) {
            return false;
        }
        
        return true;
    }
    
    private void notifyThirdPartyProcessors(String userId, String requestId) {
        List<String> processors = dataProcessorRegistry.getProcessorsForUser(userId);
        
        for (String processor : processors) {
            try {
                dataProcessorNotificationService.notifyDeletion(processor, userId, requestId);
            } catch (Exception e) {
                logger.warn("Failed to notify processor {} of data deletion for user {}", 
                           processor, userId, e);
                // Continue with other processors
            }
        }
    }
}

/**
 * @requirement REQ-COMP-PCI-001 Implement PCI DSS payment card data protection
 */
@Service
public class PaymentCardSecurityService {
    
    private final EncryptionService encryptionService;
    private final TokenizationService tokenizationService;
    
    /**
     * PCI DSS Requirement 3.4 - Protect stored cardholder data
     */
    public String securelyStoreCardData(PaymentCardData cardData) {
        // Validate card data format
        validateCardData(cardData);
        
        // Tokenize the card number
        String cardToken = tokenizationService.tokenizeCardNumber(cardData.getCardNumber());
        
        // Encrypt sensitive data
        EncryptedCardData encryptedData = EncryptedCardData.builder()
            .cardToken(cardToken)
            .encryptedCvv(encryptionService.encrypt(cardData.getCvv()))
            .cardholderName(cardData.getCardholderName()) // Not encrypted per PCI DSS
            .expiryDate(cardData.getExpiryDate()) // Not sensitive per PCI DSS
            .maskedCardNumber(maskCardNumber(cardData.getCardNumber()))
            .build();
        
        // Store in PCI DSS compliant storage
        String storageId = secureCardDataRepository.store(encryptedData);
        
        // Audit log (without sensitive data)
        auditLogger.logCardDataStorage(storageId, cardData.getCardholderName(), 
                                     cardData.getMaskedCardNumber());
        
        return storageId;
    }
    
    /**
     * PCI DSS Requirement 3.3 - Mask PAN when displayed
     */
    public String maskCardNumber(String cardNumber) {
        if (cardNumber == null || cardNumber.length() < 8) {
            return "****";
        }
        
        // Show only first 6 and last 4 digits
        String firstSix = cardNumber.substring(0, 6);
        String lastFour = cardNumber.substring(cardNumber.length() - 4);
        
        return firstSix + "******" + lastFour;
    }
    
    /**
     * PCI DSS Requirement 8 - Secure access to card data systems
     */
    @PreAuthorize("hasRole('PCI_CARD_DATA_ACCESS')")
    public PaymentCardData retrieveCardData(String storageId, String businessJustification) {
        // Verify access authorization
        if (!pciAccessValidator.isAccessAuthorized(getCurrentUser(), businessJustification)) {
            throw new UnauthorizedAccessException("Access to card data not authorized");
        }
        
        // Retrieve and decrypt data
        EncryptedCardData encryptedData = secureCardDataRepository.retrieve(storageId);
        
        if (encryptedData == null) {
            throw new CardDataNotFoundException("Card data not found: " + storageId);
        }
        
        // Decrypt sensitive fields
        String cvv = encryptionService.decrypt(encryptedData.getEncryptedCvv());
        String cardNumber = tokenizationService.detokenizeCardNumber(encryptedData.getCardToken());
        
        // Audit the access
        auditLogger.logCardDataAccess(getCurrentUser().getId(), storageId, 
                                    businessJustification, "SUCCESS");
        
        return PaymentCardData.builder()
            .cardNumber(cardNumber)
            .cvv(cvv)
            .cardholderName(encryptedData.getCardholderName())
            .expiryDate(encryptedData.getExpiryDate())
            .build();
    }
    
    private void validateCardData(PaymentCardData cardData) {
        if (cardData == null) {
            throw new ValidationException("Card data cannot be null");
        }
        
        if (!isValidCardNumber(cardData.getCardNumber())) {
            throw new ValidationException("Invalid card number format");
        }
        
        if (!isValidCvv(cardData.getCvv())) {
            throw new ValidationException("Invalid CVV format");
        }
        
        if (!isValidExpiryDate(cardData.getExpiryDate())) {
            throw new ValidationException("Invalid or expired card");
        }
    }
}

/**
 * @requirement REQ-COMP-SOX-001 Implement SOX financial reporting controls
 */
@Service
public class FinancialControlsService {
    
    /**
     * SOX Section 404 - Internal controls over financial reporting
     */
    @Transactional
    public void recordFinancialTransaction(FinancialTransaction transaction) {
        // Validate transaction data
        validateFinancialTransaction(transaction);
        
        // Apply segregation of duties
        if (!segregationOfDutiesValidator.isTransactionPermitted(
                getCurrentUser(), transaction)) {
            throw new SoxComplianceException("Segregation of duties violation");
        }
        
        // Record the transaction with immutable audit trail
        String transactionId = financialTransactionRepository.save(transaction);
        
        // Create immutable audit record
        SoxAuditRecord auditRecord = SoxAuditRecord.builder()
            .transactionId(transactionId)
            .userId(getCurrentUser().getId())
            .transactionType(transaction.getType())
            .amount(transaction.getAmount())
            .timestamp(Instant.now())
            .ipAddress(getCurrentRequest().getRemoteAddr())
            .businessJustification(transaction.getBusinessJustification())
            .approvalChain(transaction.getApprovalChain())
            .build();
        
        // Sign the audit record
        String digitalSignature = digitalSigningService.sign(auditRecord.toJson());
        auditRecord.setDigitalSignature(digitalSignature);
        
        // Store in tamper-evident audit log
        soxAuditRepository.save(auditRecord);
        
        // Real-time monitoring for suspicious patterns
        financialMonitoringService.analyzeTransaction(transaction);
    }
    
    /**
     * SOX compliance reporting
     */
    public SoxComplianceReport generateComplianceReport(DateRange dateRange) {
        // Verify report access authorization
        if (!hasReportingPermission(getCurrentUser(), "SOX_COMPLIANCE")) {
            throw new UnauthorizedAccessException("Not authorized to generate SOX reports");
        }
        
        SoxComplianceReport report = SoxComplianceReport.builder()
            .reportingPeriod(dateRange)
            .generatedBy(getCurrentUser().getId())
            .generatedAt(Instant.now())
            .build();
        
        // Internal control testing results
        List<ControlTestResult> controlTests = internalControlsService
            .getControlTestResults(dateRange);
        report.setControlTestResults(controlTests);
        
        // Financial transaction analysis
        FinancialTransactionSummary transactionSummary = 
            financialAnalysisService.summarizeTransactions(dateRange);
        report.setTransactionSummary(transactionSummary);
        
        // Segregation of duties violations
        List<SodViolation> sodViolations = segregationOfDutiesService
            .getViolations(dateRange);
        report.setSodViolations(sodViolations);
        
        // Access control reviews
        List<AccessReview> accessReviews = accessControlService
            .getAccessReviews(dateRange);
        report.setAccessReviews(accessReviews);
        
        // Digital signature for report integrity
        String reportSignature = digitalSigningService.sign(report.toJson());
        report.setDigitalSignature(reportSignature);
        
        // Store report for regulatory compliance
        complianceReportRepository.save(report);
        
        // Audit the report generation
        auditLogger.logComplianceReportGeneration("SOX", getCurrentUser().getId(), 
                                                 report.getId());
        
        return report;
    }
}
