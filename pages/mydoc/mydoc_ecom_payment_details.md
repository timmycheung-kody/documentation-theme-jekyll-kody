---
title: Payment Details
tags: [PaymentDetails]
keywords: labels, buttons, bootstrap, api methods
last_updated: July 3, 2025
sidebar: mydoc_sidebar
permalink: mydoc_ecom_payment_details.html
folder: mydoc
---


### Payment Details

`KodyEcomPaymentsService.PaymentDetails`

Get the payment details with the following parameters:

```java
rpc PaymentDetails(PaymentDetailsRequest) returns (PaymentDetailsResponse);

message PaymentDetailsRequest {
  string store_id = 1; // Your Kody store id
  oneof payment_identifier {
    string payment_id = 2; // The unique identifier created by Kody
    string payment_reference = 3; // Your unique payment reference that was set during the initiation
  }
}

message PaymentDetailsResponse {
  oneof result {
    Response response = 1;
    Error error = 2;
  }

  message Response {
    string payment_id = 1; // The unique identifier created by Kody
    string payment_reference = 2; // Your unique payment reference that was set during the initiation
    string order_id = 3; // Your identifier of the order. It doesn't have to be unique, for example when the same order has multiple payments.
    optional string order_metadata = 4; // A data set that can be used to store information about the order and used in the payment details. For example a JSON with checkout items. It will be useful as evidence to challenge chargebacks or any risk data.
    PaymentStatus status = 5;
    optional string payment_data_json = 6; // json blob containing payment data

    google.protobuf.Timestamp date_created = 7;
    optional google.protobuf.Timestamp date_paid = 8;

    enum PaymentStatus {
      PENDING = 0;
      SUCCESS = 1;
      FAILED = 2;
      CANCELLED = 3;
      EXPIRED = 4;
    }
  }
  
  message Error {
    Type type = 1;
    string message = 2;
    enum Type {
      UNKNOWN = 0;
      NOT_FOUND = 1;
      INVALID_REQUEST = 2;
    }
  }
}
```

### Examples

Java

[https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleGetPaymentDetails.java](https://github.com/KodyPay/kody-clientsdk-java/blob/main/samples/ecom/src/main/java/com/kody/ExampleGetPaymentDetails.java)