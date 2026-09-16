# OtpCom\Sdk\VerificationsApi

Exchange a token from the mobile SDKs for the recipient it proves.

All URIs are relative to https://api.otp.com, except if the operation defines another base path.

| Method | HTTP request | Description |
| ------------- | ------------- | ------------- |
| [**exchangeVerification()**](VerificationsApi.md#exchangeVerification) | **POST** /api/v1/verifications/exchange | Exchange a verification token for the recipient it proves. |


## `exchangeVerification()`

```php
exchangeVerification($verification_exchange_request): \OtpCom\Sdk\Model\VerificationExchangeResponse
```

Exchange a verification token for the recipient it proves.

Call this from YOUR backend, with a server key from your API Keys page, using the verification_token your app received from POST /client/otp/verify. It returns the recipient that was actually verified. This is the only trustworthy answer to \"did this user prove they control this number\": the `matched` field the device saw is a UI hint, read off a device you do not control, and an app can claim anything. Exchanging is idempotent for the same API key within the token lifetime, so a retry after a network failure returns the same result instead of losing the verification. Any other key, a second use, or an expired token gets a 404.

### Example

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');


// Configure Bearer authorization: bearerAuth
$config = OtpCom\Sdk\Configuration::getDefaultConfiguration()->setAccessToken('YOUR_ACCESS_TOKEN');


$apiInstance = new OtpCom\Sdk\Api\VerificationsApi(
    // If you want use custom http client, pass your client which implements `GuzzleHttp\ClientInterface`.
    // This is optional, `GuzzleHttp\Client` will be used as default.
    new GuzzleHttp\Client(),
    $config
);
$verification_exchange_request = new \OtpCom\Sdk\Model\VerificationExchangeRequest(); // \OtpCom\Sdk\Model\VerificationExchangeRequest

try {
    $result = $apiInstance->exchangeVerification($verification_exchange_request);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling VerificationsApi->exchangeVerification: ', $e->getMessage(), PHP_EOL;
}
```

### Parameters

| Name | Type | Description  | Notes |
| ------------- | ------------- | ------------- | ------------- |
| **verification_exchange_request** | [**\OtpCom\Sdk\Model\VerificationExchangeRequest**](../Model/VerificationExchangeRequest.md)|  | |

### Return type

[**\OtpCom\Sdk\Model\VerificationExchangeResponse**](../Model/VerificationExchangeResponse.md)

### Authorization

[bearerAuth](../../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`

[[Back to top]](#) [[Back to API list]](../../README.md#endpoints)
[[Back to Model list]](../../README.md#models)
[[Back to README]](../../README.md)
