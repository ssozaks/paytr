# Testing

## Purpose
Defines requirements for the test suite of the PayTR payment integration library. Currently no tests exist in the codebase; this spec serves as a skeleton for planned test coverage.

<!-- TODO: update after implementation -->

## Requirements

### REQ-TS-001: Unit Tests for Config Class
The system SHALL have unit tests covering the Config class construction and property access.

<!-- TODO: update after implementation -->

#### Scenario: Config construction with valid array
- WHEN a unit test creates a Config with all required keys
- THEN all getter methods MUST return the expected values
- AND the test SHALL verify each of the six config properties

#### Scenario: Config fluent setter chaining
- WHEN a unit test creates an empty Config and chains setter calls
- THEN the final Config object MUST contain all set values
- AND each setter MUST return the Config instance

**File:** `tests/Unit/ConfigTest.php` <!-- TODO: create this file -->

### REQ-TS-002: Unit Tests for Order Class
The system SHALL have unit tests covering Order data model construction, defaults, and formatting methods.

<!-- TODO: update after implementation -->

#### Scenario: Order default values
- WHEN a unit test creates an Order without arguments
- THEN currency MUST be 'TL', noInstallment MUST be 0, maxInstallment MUST be 0, isTransfer MUST be false

#### Scenario: Amount formatting
- WHEN a unit test sets an amount of 100.25 on an Order
- THEN getFormattedAmount() MUST return 10025

#### Scenario: Basket formatting
- WHEN a unit test sets a basket array on an Order
- THEN getFormattedBasket() MUST return the base64-encoded JSON of that array

**File:** `tests/Unit/OrderTest.php` <!-- TODO: create this file -->

### REQ-TS-003: Unit Tests for Response Class
The system SHALL have unit tests covering Response status checking, message extraction, and token retrieval.

<!-- TODO: update after implementation -->

#### Scenario: Success response
- WHEN a unit test sets content with status 'success' and a token
- THEN isSuccess() MUST return true, isError() MUST return false, getToken() MUST return the token

#### Scenario: Error response
- WHEN a unit test sets content with a non-success status and a reason
- THEN isSuccess() MUST return false, isError() MUST return true, getMessage() MUST return the reason

**File:** `tests/Unit/ResponseTest.php` <!-- TODO: create this file -->

### REQ-TS-004: Unit Tests for Hash Generation
The system SHALL have unit tests verifying HMAC-SHA256 hash generation for both standard and transfer payments.

<!-- TODO: update after implementation -->

#### Scenario: Standard payment hash
- WHEN a unit test generates a hash for a non-transfer order with known inputs
- THEN the resulting hash token MUST match the expected HMAC-SHA256 base64 output

#### Scenario: Transfer payment hash
- WHEN a unit test generates a hash for a transfer order with known inputs
- THEN the hash input string MUST include 'eft' instead of basket/installment/currency
- AND the resulting token MUST differ from the standard payment token

**File:** `tests/Unit/PaymentHashTest.php` <!-- TODO: create this file -->

### REQ-TS-005: Integration Tests for Payment Flow
The system SHALL have integration tests covering the end-to-end payment flow with mocked HTTP responses.

<!-- TODO: update after implementation -->

#### Scenario: Successful payment flow
- WHEN a test executes the full payment flow with a mocked PayTR API returning success
- THEN the Response object MUST indicate success
- AND the token SHALL be extractable from the response

#### Scenario: Failed payment flow
- WHEN a test executes the payment flow with a mocked PayTR API returning an error
- THEN the Response object MUST indicate error
- AND getMessage() SHALL return the error reason

**File:** `tests/Integration/PaymentFlowTest.php` <!-- TODO: create this file -->

### REQ-TS-006: Unit Tests for Callback Hash Verification
The system SHALL have unit tests verifying the callback hash checking mechanism.

<!-- TODO: update after implementation -->

#### Scenario: Valid callback hash verification
- WHEN a test simulates a callback request with a correctly computed hash
- THEN checkHash() MUST return true

#### Scenario: Invalid callback hash verification
- WHEN a test simulates a callback request with an incorrect hash
- THEN checkHash() MUST return false

**File:** `tests/Unit/CallbackHashTest.php` <!-- TODO: create this file -->
