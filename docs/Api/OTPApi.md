# OtpCom\Sdk\OTPApi

Send, verify, resend, and check one-time passwords.

All URIs are relative to https://api.otp.com, except if the operation defines another base path.

| Method | HTTP request | Description |
| ------------- | ------------- | ------------- |
| [**getOtpStatus()**](OTPApi.md#getOtpStatus) | **GET** /api/v1/otp/{otp_id} | Fetch the current status of an OTP. |
| [**resendOtp()**](OTPApi.md#resendOtp) | **POST** /api/v1/otp/resend | Resend a pending OTP, escalating the channel if configured. |
| [**sendOtp()**](OTPApi.md#sendOtp) | **POST** /api/v1/otp/send | Start an OTP: routes a channel and dispatches the code. |
| [**verifyOtp()**](OTPApi.md#verifyOtp) | **POST** /api/v1/otp/verify | Verify a code against a pending OTP. |


## `getOtpStatus()`

```php
getOtpStatus($otp_id): \OtpCom\Sdk\Model\OtpStatusResponse
```

Fetch the current status of an OTP.

### Example

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');


// Configure Bearer authorization: bearerAuth
$config = OtpCom\Sdk\Configuration::getDefaultConfiguration()->setAccessToken('YOUR_ACCESS_TOKEN');


$apiInstance = new OtpCom\Sdk\Api\OTPApi(
    // If you want use custom http client, pass your client which implements `GuzzleHttp\ClientInterface`.
    // This is optional, `GuzzleHttp\Client` will be used as default.
    new GuzzleHttp\Client(),
    $config
);
$otp_id = 'otp_id_example'; // string

try {
    $result = $apiInstance->getOtpStatus($otp_id);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling OTPApi->getOtpStatus: ', $e->getMessage(), PHP_EOL;
}
```

### Parameters

| Name | Type | Description  | Notes |
| ------------- | ------------- | ------------- | ------------- |
| **otp_id** | **string**|  | |

### Return type

[**\OtpCom\Sdk\Model\OtpStatusResponse**](../Model/OtpStatusResponse.md)

### Authorization

[bearerAuth](../../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: Not defined
- **Accept**: `application/json`

[[Back to top]](#) [[Back to API list]](../../README.md#endpoints)
[[Back to Model list]](../../README.md#models)
[[Back to README]](../../README.md)

## `resendOtp()`

```php
resendOtp($resend_request): \OtpCom\Sdk\Model\OtpResponse
```

Resend a pending OTP, escalating the channel if configured.

### Example

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');


// Configure Bearer authorization: bearerAuth
$config = OtpCom\Sdk\Configuration::getDefaultConfiguration()->setAccessToken('YOUR_ACCESS_TOKEN');


$apiInstance = new OtpCom\Sdk\Api\OTPApi(
    // If you want use custom http client, pass your client which implements `GuzzleHttp\ClientInterface`.
    // This is optional, `GuzzleHttp\Client` will be used as default.
    new GuzzleHttp\Client(),
    $config
);
$resend_request = new \OtpCom\Sdk\Model\ResendRequest(); // \OtpCom\Sdk\Model\ResendRequest

try {
    $result = $apiInstance->resendOtp($resend_request);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling OTPApi->resendOtp: ', $e->getMessage(), PHP_EOL;
}
```

### Parameters

| Name | Type | Description  | Notes |
| ------------- | ------------- | ------------- | ------------- |
| **resend_request** | [**\OtpCom\Sdk\Model\ResendRequest**](../Model/ResendRequest.md)|  | |

### Return type

[**\OtpCom\Sdk\Model\OtpResponse**](../Model/OtpResponse.md)

### Authorization

[bearerAuth](../../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`

[[Back to top]](#) [[Back to API list]](../../README.md#endpoints)
[[Back to Model list]](../../README.md#models)
[[Back to README]](../../README.md)

## `sendOtp()`

```php
sendOtp($send_request, $idempotency_key): \OtpCom\Sdk\Model\OtpResponse
```

Start an OTP: routes a channel and dispatches the code.

Routing picks the channel from the app config. When it selects WhatsApp the code is not sent yet: the response returns action_url (a wa.me link) the user opens to receive the code over WhatsApp, and the OTP stays pending until they enter it. On all other channels the code is delivered directly and action_url is null. Either way the user enters the code and you call POST /otp/verify.

### Example

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');


// Configure Bearer authorization: bearerAuth
$config = OtpCom\Sdk\Configuration::getDefaultConfiguration()->setAccessToken('YOUR_ACCESS_TOKEN');


$apiInstance = new OtpCom\Sdk\Api\OTPApi(
    // If you want use custom http client, pass your client which implements `GuzzleHttp\ClientInterface`.
    // This is optional, `GuzzleHttp\Client` will be used as default.
    new GuzzleHttp\Client(),
    $config
);
$send_request = new \OtpCom\Sdk\Model\SendRequest(); // \OtpCom\Sdk\Model\SendRequest
$idempotency_key = 'idempotency_key_example'; // string | Replay the prior response for a repeated request; a reused key with a different body is a 409.

try {
    $result = $apiInstance->sendOtp($send_request, $idempotency_key);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling OTPApi->sendOtp: ', $e->getMessage(), PHP_EOL;
}
```

### Parameters

| Name | Type | Description  | Notes |
| ------------- | ------------- | ------------- | ------------- |
| **send_request** | [**\OtpCom\Sdk\Model\SendRequest**](../Model/SendRequest.md)|  | |
| **idempotency_key** | **string**| Replay the prior response for a repeated request; a reused key with a different body is a 409. | [optional] |

### Return type

[**\OtpCom\Sdk\Model\OtpResponse**](../Model/OtpResponse.md)

### Authorization

[bearerAuth](../../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`

[[Back to top]](#) [[Back to API list]](../../README.md#endpoints)
[[Back to Model list]](../../README.md#models)
[[Back to README]](../../README.md)

## `verifyOtp()`

```php
verifyOtp($verify_request): \OtpCom\Sdk\Model\VerifyResponse
```

Verify a code against a pending OTP.

### Example

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');


// Configure Bearer authorization: bearerAuth
$config = OtpCom\Sdk\Configuration::getDefaultConfiguration()->setAccessToken('YOUR_ACCESS_TOKEN');


$apiInstance = new OtpCom\Sdk\Api\OTPApi(
    // If you want use custom http client, pass your client which implements `GuzzleHttp\ClientInterface`.
    // This is optional, `GuzzleHttp\Client` will be used as default.
    new GuzzleHttp\Client(),
    $config
);
$verify_request = new \OtpCom\Sdk\Model\VerifyRequest(); // \OtpCom\Sdk\Model\VerifyRequest

try {
    $result = $apiInstance->verifyOtp($verify_request);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling OTPApi->verifyOtp: ', $e->getMessage(), PHP_EOL;
}
```

### Parameters

| Name | Type | Description  | Notes |
| ------------- | ------------- | ------------- | ------------- |
| **verify_request** | [**\OtpCom\Sdk\Model\VerifyRequest**](../Model/VerifyRequest.md)|  | |

### Return type

[**\OtpCom\Sdk\Model\VerifyResponse**](../Model/VerifyResponse.md)

### Authorization

[bearerAuth](../../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`

[[Back to top]](#) [[Back to API list]](../../README.md#endpoints)
[[Back to Model list]](../../README.md#models)
[[Back to README]](../../README.md)
