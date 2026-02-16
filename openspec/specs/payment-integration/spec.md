# Payment Integration

## Purpose
Defines requirements for the core PayTR payment integration library, including payment object construction, order management, configuration handling, response parsing, and the end-to-end payment flow from order creation to iframe token retrieval.

## Requirements

### REQ-PI-001: Payment Object Construction
The system SHALL allow creating a Payment instance with an optional configuration array or without arguments.

#### Scenario: Create Payment with config array
- WHEN a developer instantiates `new Payment($configArray)` with a valid config array
- THEN the Payment object MUST create an internal Config object with the provided values
- AND a CurlHttpClient MUST be initialized as the HTTP client

#### Scenario: Create Payment without config
- WHEN a developer instantiates `new Payment()` without arguments
- THEN the Payment object MUST be created with a null config
- AND the developer SHALL be able to call `setConfig()` later

**File:** `src/Payment.php`

### REQ-PI-002: Configuration Management
The system SHALL support configuring the payment gateway with apiUrl, merchantId, merchantKey, merchantSalt, successUrl, and failUrl.

#### Scenario: Config via constructor array
- WHEN a Config object is constructed with a complete config array
- THEN all six properties (apiUrl, merchantId, merchantKey, merchantSalt, successUrl, failUrl) MUST be set
- AND each property SHALL be retrievable via its getter method

#### Scenario: Config via fluent setters
- WHEN a developer creates an empty Config and uses setter methods
- THEN each setter MUST return the Config instance for method chaining
- AND the values SHALL be persisted on the object

**File:** `src/Config.php`

### REQ-PI-003: Order Data Model
The system SHALL represent payment orders with id, amount, currency, basket, installment options, customer info (ip, email, name, address, phone), and an optional transfer flag.

#### Scenario: Order with default values
- WHEN an Order is created without arguments
- THEN currency SHALL default to 'TL'
- AND noInstallment SHALL default to 0
- AND maxInstallment SHALL default to 0
- AND isTransfer SHALL default to false
- AND address and phone SHALL default to null

#### Scenario: Order amount formatting
- WHEN an Order has amount 100.25
- THEN getFormattedAmount() MUST return the integer 10025 (amount multiplied by 100, decimal removed)

#### Scenario: Order basket formatting
- WHEN an Order has a basket array
- THEN getFormattedBasket() MUST return a base64-encoded JSON string of the basket

**File:** `src/Order.php`

### REQ-PI-004: Payment Preparation
The system SHALL prepare POST data containing all required PayTR API fields before making the API call.

#### Scenario: Standard credit card payment preparation
- WHEN prepare() is called on a Payment with a non-transfer Order
- THEN the POST data MUST include merchant_id, user_ip, merchant_oid, email, payment_amount, paytr_token, test_mode, user_basket, debug_on, no_installment, max_installment, user_name, user_address, user_phone, merchant_ok_url, merchant_fail_url, timeout_limit, and currency

#### Scenario: EFT/transfer payment preparation
- WHEN prepare() is called on a Payment with an Order where isTransfer is true
- THEN the POST data MUST include payment_type set to 'eft'
- AND the POST data SHALL NOT include user_name or user_address fields

**File:** `src/Payment.php`

### REQ-PI-005: Payment Execution (make)
The system SHALL send a POST request to the PayTR API and parse the JSON response.

#### Scenario: Successful API call
- WHEN make() is called with valid configuration and order data
- THEN the system MUST send a POST request to the configured apiUrl
- AND the response JSON MUST be decoded and stored in a Response object
- AND the Payment object SHALL be returned for method chaining

#### Scenario: HTTP error during API call
- WHEN the PayTR API returns an HTTP error
- THEN the system MUST throw a ClientException with the error message

#### Scenario: Invalid order data
- WHEN make() is called with incomplete order data (missing required fields)
- THEN the system MUST throw an InvalidOrderDataException

**File:** `src/Payment.php`

### REQ-PI-006: Response Handling
The system SHALL parse PayTR API responses and provide status checking, message extraction, and token retrieval.

#### Scenario: Successful response
- WHEN the API response contains status 'success'
- THEN isSuccess() MUST return true
- AND isError() MUST return false
- AND getToken() SHALL return the payment iframe token

#### Scenario: Error response
- WHEN the API response contains a non-success status
- THEN isSuccess() MUST return false
- AND isError() MUST return true
- AND getMessage() SHALL return the reason string from the response

**File:** `src/Response.php`

### REQ-PI-007: Custom Exception Types
The system SHALL provide typed exceptions for different error scenarios in the payment flow.

#### Scenario: Invalid order data error
- WHEN order data is missing required fields during hash generation
- THEN an InvalidOrderDataException MUST be thrown
- AND it SHALL extend the base PHP Exception class

#### Scenario: HTTP client error
- WHEN the HTTP client encounters an error during the API call
- THEN a ClientException MUST be thrown
- AND it SHALL extend the base PHP Exception class

**File:** `src/Exceptions/InvalidOrderDataException.php`, `src/Exceptions/ClientException.php`

### REQ-PI-008: Test Mode and Debug Mode
The system SHALL support test mode and debug mode flags that are included in API requests.

#### Scenario: Enable test mode
- WHEN setTestMode(true) is called on the Payment object
- THEN isTestMode() MUST return true
- AND the test_mode field in POST data SHALL be set to true

#### Scenario: Enable debug mode
- WHEN setDebug(true) is called on the Payment object
- THEN isDebug() MUST return true
- AND the debug_on field in POST data SHALL be set to true

**File:** `src/Payment.php`

### REQ-PI-009: Timeout Configuration
The system SHALL allow configuring the HTTP request timeout with a default of 30 seconds.

#### Scenario: Default timeout
- WHEN a Payment object is created
- THEN getTimeOutLimit() MUST return 30

#### Scenario: Custom timeout
- WHEN setTimeOutLimit(60) is called
- THEN getTimeOutLimit() MUST return 60
- AND the HTTP request SHALL use the configured timeout value

**File:** `src/Payment.php`

### REQ-PI-010: EFT/Bank Transfer Support
The system SHALL support EFT (bank transfer) payments as an alternative to credit card payments.

#### Scenario: Mark order as transfer
- WHEN setTransfer(true) is called on an Order
- THEN isTransfer() MUST return true
- AND the hash generation logic SHALL use 'eft' as the payment type marker
- AND the prepared POST data SHALL include payment_type set to 'eft'

#### Scenario: Transfer hash differs from card hash
- WHEN generating a hash for a transfer order
- THEN the hash string MUST include 'eft' instead of basket, noInstallment, maxInstallment, and currency fields

**File:** `src/Order.php`, `src/Payment.php`
