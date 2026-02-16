# API Endpoints

## Purpose
Defines requirements for the PayTR external API communication, including the token request endpoint, callback notification endpoint, and the iframe integration used to display the payment form to end users.

## Requirements

### REQ-AE-001: Token Request Endpoint
The system SHALL communicate with the PayTR token API at the configured apiUrl to obtain a payment iframe token.

#### Scenario: POST token request
- WHEN the Payment::make() method is called
- THEN the system MUST send an HTTP POST request to the apiUrl (default: `https://www.paytr.com/odeme/api/get-token`)
- AND the request body MUST contain all required PayTR fields as form-encoded data
- AND the request timeout SHALL match the configured timeOutLimit

#### Scenario: Token response parsing
- WHEN the PayTR API returns a JSON response with status 'success'
- THEN the system MUST extract and store the token from the response
- AND the token SHALL be usable to construct the iframe URL `https://www.paytr.com/odeme/guvenli/{token}`

**File:** `src/Payment.php`

### REQ-AE-002: Callback Notification Endpoint
The system SHALL support processing payment result callbacks sent by PayTR to the merchant's server.

#### Scenario: Valid callback notification
- WHEN PayTR sends a POST request to the callback URL with merchant_oid, status, total_amount, and hash
- THEN the system MUST verify the hash using checkHash()
- AND if the hash is valid, the callback handler SHALL respond with 'OK'

#### Scenario: Invalid callback notification
- WHEN the callback hash does not match the computed hash
- THEN the system MUST reject the notification
- AND SHALL NOT process the payment as successful

**File:** `src/Payment.php`, `example/callback.php`

### REQ-AE-003: Success and Fail URL Configuration
The system SHALL support configuring merchant_ok_url and merchant_fail_url for post-payment redirects.

#### Scenario: Successful payment redirect
- WHEN a payment completes successfully on the PayTR iframe
- THEN PayTR MUST redirect the user to the configured successUrl (merchant_ok_url)

#### Scenario: Failed payment redirect
- WHEN a payment fails on the PayTR iframe
- THEN PayTR MUST redirect the user to the configured failUrl (merchant_fail_url)

**File:** `src/Config.php`, `config/paytr.php`

### REQ-AE-004: PayTR Iframe Integration
The example application SHALL embed the PayTR payment iframe using the token obtained from the API.

#### Scenario: Iframe rendering
- WHEN a valid token is obtained from the PayTR API
- THEN the example page MUST render an iframe pointing to `https://www.paytr.com/odeme/guvenli/{token}`
- AND the iframe MUST use the PayTR iframeResizer script for responsive sizing

#### Scenario: Payment form submission
- WHEN the user submits the example payment form via POST
- THEN the system MUST create a Payment and Order object from the form data
- AND call make() to obtain the iframe token
- AND display the payment iframe to the user

**File:** `example/payment.php`, `example/index.php`
