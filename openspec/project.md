# PayTR Payment Library - Architecture Overview

## Project Summary

PayTR (`mews/paytr`) is a PHP Composer library that provides integration with the PayTR Turkish payment gateway. It enables merchants to generate payment tokens via the PayTR API and embed a secure payment iframe in their applications. The library supports both credit card payments and EFT (bank transfer) payments.

## Tech Stack

| Layer         | Technology                                    |
|---------------|-----------------------------------------------|
| Language      | PHP >= 7.1.3 (strict_types)                  |
| HTTP Client   | Symfony HttpClient (CurlHttpClient)           |
| HTTP Request  | Symfony HttpFoundation (Request, Response)     |
| Autoloading   | PSR-4 via Composer                            |
| Package Type  | Composer library                              |
| License       | MIT                                           |

## Architecture

```
src/
  Payment.php              # Main entry point - orchestrates payment flow
  Config.php               # Configuration value object (merchant credentials, URLs)
  Order.php                # Order data model (amount, basket, customer info)
  Response.php             # API response wrapper (status, token, message)
  Exceptions/
    InvalidOrderDataException.php   # Thrown when order data is incomplete
    ClientException.php             # Thrown on HTTP client errors

config/
  paytr.php                # Default configuration template (placeholder values)

example/
  index.php                # Payment form (HTML/Bootstrap)
  payment.php              # Payment processing (creates Payment, gets token, shows iframe)
  callback.php             # PayTR callback handler (hash verification)
  view/
    header.php             # Shared HTML header (Bootstrap 4 CDN)
    footer.php             # Shared HTML footer (jQuery, Bootstrap JS, iframeResizer)
  css/
    app.css                # Minimal form styling
```

## Payment Flow

```
1. Merchant creates Payment + Config + Order objects
2. Payment::make() is called
   a. Payment::prepare() generates HMAC-SHA256 hash token
   b. POST request sent to PayTR API (get-token endpoint)
   c. Response parsed and stored
3. If successful, iframe token is returned
4. Merchant embeds PayTR iframe with token URL
5. Customer completes payment in iframe
6. PayTR sends callback POST to merchant callback URL
7. Merchant verifies callback hash via Payment::checkHash()
8. PayTR redirects customer to success or fail URL
```

## Security Model

- **Outgoing requests:** HMAC-SHA256 hash of order data + merchantSalt, signed with merchantKey
- **Incoming callbacks:** HMAC-SHA256 hash of (merchant_oid + merchantSalt + status + total_amount), signed with merchantKey
- **Credentials:** merchantId, merchantKey, merchantSalt stored in config (not hardcoded)
- **Hash generation differs** between standard card payments and EFT transfers

## Spec Categories

| Category              | Spec ID Prefix | Description                                      |
|-----------------------|----------------|--------------------------------------------------|
| payment-integration   | REQ-PI-*       | Core payment flow, order model, config, response |
| api-endpoints         | REQ-AE-*       | PayTR API communication, callbacks, iframe       |
| auth                  | REQ-AU-*       | HMAC hash generation, callback verification      |
| testing               | REQ-TS-*       | Unit and integration test requirements           |
