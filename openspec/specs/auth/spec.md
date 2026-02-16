# Authentication and Security

## Purpose
Defines requirements for the cryptographic authentication mechanisms used in PayTR payment integration, including HMAC-SHA256 hash generation for outgoing payment requests and hash verification for incoming callback notifications.

## Requirements

### REQ-AU-001: Payment Token Hash Generation
The system SHALL generate an HMAC-SHA256 hash token for authenticating payment requests to the PayTR API.

#### Scenario: Standard payment hash generation
- WHEN generateHash() is called for a non-transfer order
- THEN the hash input string MUST be composed of: merchantId + userIp + orderId + email + formattedAmount + formattedBasket + noInstallment + maxInstallment + currency + testMode
- AND the hash MUST be concatenated in this exact order without separators

#### Scenario: Transfer payment hash generation
- WHEN generateHash() is called for a transfer order (isTransfer = true)
- THEN the hash input string MUST be composed of: merchantId + userIp + orderId + email + formattedAmount + 'eft' + testMode
- AND the basket, installment, and currency fields SHALL NOT be included in the hash

**File:** `src/Payment.php`

### REQ-AU-002: HMAC-SHA256 Token Computation
The system SHALL compute the final paytr_token using HMAC-SHA256 with base64 encoding.

#### Scenario: Token computation
- WHEN getHashToken() is called
- THEN the system MUST compute HMAC-SHA256 of (hashString + merchantSalt) using merchantKey as the secret key
- AND the raw binary output MUST be base64-encoded
- AND the resulting string SHALL be included as paytr_token in the API request

#### Scenario: Hash components
- WHEN the hash is generated
- THEN merchantSalt MUST be appended to the hash string before HMAC computation
- AND merchantKey MUST be used as the HMAC secret key (not included in the hash string)

**File:** `src/Payment.php`

### REQ-AU-003: Callback Hash Verification
The system SHALL verify the authenticity of PayTR callback notifications using HMAC-SHA256 hash comparison.

#### Scenario: Valid callback hash
- WHEN checkHash() is called and the incoming POST contains merchant_oid, status, total_amount, and hash
- THEN the system MUST compute HMAC-SHA256 of (merchant_oid + merchantSalt + status + total_amount) using merchantKey
- AND the base64-encoded result MUST be compared to the incoming hash parameter
- AND if they match, checkHash() SHALL return true

#### Scenario: Invalid callback hash
- WHEN the computed hash does not match the incoming hash parameter
- THEN checkHash() MUST return false
- AND the callback SHALL be treated as potentially fraudulent

**File:** `src/Payment.php`

### REQ-AU-004: Merchant Credentials Security
The system SHALL handle merchant credentials (merchantId, merchantKey, merchantSalt) securely.

#### Scenario: Credentials in configuration
- WHEN merchant credentials are provided via config array
- THEN the Config object MUST store them as private properties
- AND they SHALL only be accessible via getter methods

#### Scenario: No hardcoded credentials
- WHEN the library is distributed
- THEN the default config file MUST contain placeholder values (XXXXXX)
- AND real credentials SHALL be provided by the consuming application

<!-- TODO: update after implementation - add credential rotation and environment variable support -->

**File:** `config/paytr.php`, `src/Config.php`

### REQ-AU-005: Order Data Validation for Hash Integrity
The system SHALL validate that all required order fields are present before generating the payment hash.

#### Scenario: Missing required order fields
- WHEN generateHash() is called and a required field (name, address, phone) is not set on the Order
- THEN the system MUST throw an InvalidOrderDataException
- AND the exception message SHALL indicate which field caused the TypeError

#### Scenario: All required fields present
- WHEN generateHash() is called with all required order fields set
- THEN the hash string MUST be generated successfully without exceptions

**File:** `src/Payment.php`, `src/Exceptions/InvalidOrderDataException.php`
