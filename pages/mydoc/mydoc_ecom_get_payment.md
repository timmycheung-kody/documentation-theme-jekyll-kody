---
title: Get Payments
tags: [GetPayments]
keywords: labels, buttons, bootstrap, api methods
last_updated: July 3, 2025
sidebar: mydoc_sidebar
permalink: mydoc_ecom_get_payment.html
folder: mydoc
---


### Get Payments

`KodyEcomPaymentsService.GetPayments` 

Get a paginated list of payments:

```java
rpc GetPayments(GetPaymentsRequest) returns (GetPaymentsResponse);

message GetPaymentsRequest {
  string store_id = 1;
  PageCursor page_cursor = 2;
  Filter filter = 3;

  message Filter {
    optional string order_id = 1;
    optional google.protobuf.Timestamp created_before = 2;
  }

  message PageCursor {
    int64 page = 1;
    int64 page_size = 2;
  }
}

message GetPaymentsResponse {
  oneof result {
    Response response = 1;
    Error error = 2;
  }

  message Response {
    repeated PaymentDetails payments = 1;
    int64 total = 2;

    message PaymentDetails {
      string payment_id = 1; // The unique identifier created by Kody
      string payment_reference = 2; // Your unique payment reference that was set during the initiation
      string order_id = 3; // Your identifier of the order. It doesn't have to be unique, for example when the same order has multiple payments.
      optional string order_metadata = 4; // A data set that can be used to store information about the order and used in the payment details. For example a JSON with checkout items. It will be useful as evidence to challenge chargebacks or any risk data.
      PaymentStatus status = 5;
      optional string payment_data_json = 6; // json blob containing payment data

      google.protobuf.Timestamp date_created = 7;
      optional google.protobuf.Timestamp date_paid = 8;
      optional string psp_reference = 9;
      optional string payment_method = 10;

      enum PaymentStatus {
        PENDING = 0;
        SUCCESS = 1;
        FAILED = 2;
        CANCELLED = 3;
        EXPIRED = 4;
      }
    }
  }

  message Error {
    Type type = 1;
    string message = 2;

    enum Type {
      UNKNOWN = 0;
      NOT_FOUND = 1;
      INVALID_ARGUMENT = 2;
    }
  }
}
```

### Examples

Java

[https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleGetPayments.java](https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleGetPayments.java)