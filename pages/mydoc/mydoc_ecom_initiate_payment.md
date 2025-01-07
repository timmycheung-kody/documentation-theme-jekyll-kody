---
title: Initiate Payment
tags: [InitiatePayment]
keywords: labels, buttons, bootstrap, api methods
last_updated: July 3, 2025
sidebar: mydoc_sidebar
permalink: mydoc_ecom_initiate_payment.html
folder: mydoc
---


### Initiate Payment

**Service call:** `KodyEcomPaymentsService.InitiatePayment`

Sets up a new payment to be paid online by the user on a browser, according to the [sequence diagram](https://www.notion.so/Kody-Payments-API-Documentation-e26a60bace804508a3a30bda512a29c8?pvs=21) above. 

The response includes a `payment_url` that the user browser should open. After the user pays, cancels or expires the payment page, it will redirect to the defined `return_url` in the payment initiation request.

If the `return_url` was not called because of the user closing the browser, user network problem, or any other reason, the `payment_id` can be used to [get the payment details](https://www.notion.so/Kody-Payments-API-Documentation-e26a60bace804508a3a30bda512a29c8?pvs=21) and poll its the status.

```protobuf
rpc InitiatePayment(PaymentInitiationRequest) returns (PaymentInitiationResponse);

message PaymentInitiationRequest {
  string store_id = 1; // Your Kody store id
  string payment_reference = 2; // Your unique reference of this payment request.
  uint64 amount = 3; // Amount in minor units. For example, 2000 means GBP 20.00.
  string currency = 4; // ISO 4217 three letter currency code
  string order_id = 5; // Your identifier of the order. It doesn't have to be unique, for example when the same order has multiple payments.
  optional string order_metadata = 6; // A data set that can be used to store information about the order and used in the payment details. For example a JSON with checkout items. It will be useful as evidence to challenge chargebacks or any risk data.
  string return_url = 7; // The URL that your client application will be redirected to after the payment is authorised. You can include additional query parameters, for example, the user id or order reference.
  optional string payer_statement = 8; // The text to be shown on the payer's bank statement. Maximum 22 characters, otherwise banks might truncate the string. If not set it will use the store's terminals receipt printing name. Allowed characters: a-z, A-Z, 0-9, spaces, and special characters . , ' _ - ? + * /
  optional string payer_email_address = 9; // We recommend that you provide this data, as it is used in velocity fraud checks. Required for 3D Secure 2 transactions.
  optional string payer_ip_address = 10; // The payer IP address used for risk checks, also required for 3D Secure 2 transactions.
  optional string payer_locale = 11; // The language code and country code to specify the language to display the payment pages. It will default to en_GB if not set.
  optional bool tokenise_card = 12; // defaults false
  optional ExpirySettings expiry = 13; // Nested message for expiry settings

  message ExpirySettings {
    bool show_timer = 1; // Display a countdown timer to the user in the payment page, default is false
    uint64 expiring_seconds = 2; // Timeout duration in seconds, defaults to 1800 seconds (30 minutes)
  }
}

// Payment Initiation Response
message PaymentInitiationResponse {
  oneof result {
    Response response = 1;
    Error error = 2;
  }

  message Response {
    string payment_id = 1; // The unique identifier created by Kody
    string payment_url = 2; // The URL to send the user to from your application
  }

  message Error {
    Type type = 1;
    string message = 2;

    enum Type {
      UNKNOWN = 0;
      DUPLICATE_ATTEMPT = 1;
      INVALID_REQUEST = 2;
    }
  }
}
```

### Examples

Java

[https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleNewPayment.java](https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleNewPayment.java)