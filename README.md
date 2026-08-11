# otp.com PHP SDK

PHP client for the [otp.com](https://otp.com) OTP API: send a one-time password, verify the code the
user entered, resend it on another channel.

Requires PHP 8.1+ and Guzzle 7.

- **API contract:** [otp-com/sdk](https://github.com/otp-com/sdk) (`openapi.yaml`)
- **Method and model reference:** [`docs/`](./docs)
- **Other languages:** [Node.js](https://github.com/otp-com/sdk-node) ·
  [Go](https://github.com/otp-com/sdk-go) · [Python](https://github.com/otp-com/sdk-python) ·
  [MCP server](https://github.com/otp-com/mcp)

## Install

```sh
composer require otp-com/sdk-php
```

## Quickstart

Get an API key from the otp.com panel under **API Keys**. `otp_live_…` sends for real, `otp_test_…`
runs in sandbox. Keep it server-side; it is a bearer credential.

```php
<?php

use OtpCom\Sdk\Api\OtpApi;
use OtpCom\Sdk\Configuration;
use OtpCom\Sdk\Model\SendRequest;
use OtpCom\Sdk\Model\VerifyRequest;

$config = Configuration::getDefaultConfiguration()
    ->setAccessToken(getenv('OTP_API_KEY'));

$otp = new OtpApi(new GuzzleHttp\Client(), $config);

// 1. Send. You pass the recipient; your account routing picks the channel.
$sent = $otp->sendOtp(new SendRequest([
    'recipient' => '+14155552671',
    'locale' => 'en',
]));

$sent->getOtpId();           // keep this: you verify against it
$sent->getChannel();         // 'sms' | 'whatsapp' | 'email' | 'telegram'
$sent->getMaskedRecipient(); // '+14****71', safe to show the user
$sent->getActionUrl();       // WhatsApp only, see below

// 2. Verify whatever the user typed in.
$result = $otp->verifyOtp(new VerifyRequest([
    'otp_id' => $sent->getOtpId(),
    'code' => '123456',
]));

if ($result->getMatched()) {
    // The code was correct; $result->getStatus() is 'approved'.
}
```

The code itself is never returned by the API. `recipient` is a phone number in E.164 or an email
address; which one is valid depends on the channels enabled for your app.

Model constructors take the wire field names (`otp_id`, not `otpId`).

### Retries that must not double-send

Pass an idempotency key and a repeat of the same call replays the first response instead of sending
a second code. Reusing a key with a different body is a `409`.

```php
$otp->sendOtp(new SendRequest(['recipient' => $recipient]), "signup:{$userId}");
```

## WhatsApp: the code comes back to the user

Verification is identical on every channel, but WhatsApp delivery has one extra step. When routing
picks WhatsApp, the code has **not** been sent yet and the response carries an action URL:

```php
$sent = $otp->sendOtp(new SendRequest(['recipient' => $recipient]));

if ($sent->getActionUrl() !== null) {
    // Open it for the user. They send us the prefilled message from their own WhatsApp,
    // we reply with the code, and the OTP stays 'pending' until they enter it.
    redirect($sent->getActionUrl());
}
```

Then call `verifyOtp` exactly as on SMS. The action URL is `null` on every other channel. Don't poll
for a WhatsApp OTP to approve itself: nothing leaves `pending` without a `verifyOtp` call. If the
user has no WhatsApp, resend on a channel they do have.

## Resending

```php
use OtpCom\Sdk\Model\ResendRequest;

// Advance to the next channel in your routing order.
$otp->resendOtp(new ResendRequest(['otp_id' => $sent->getOtpId()]));

// Or move it onto a specific channel, e.g. the user has no WhatsApp.
$otp->resendOtp(new ResendRequest(['otp_id' => $sent->getOtpId(), 'channel' => 'sms']));
```

A resend before the cooldown elapses is a `429`; a channel that isn't enabled for your app or the
recipient is a `409`.

## Checking status

```php
$current = $otp->getOtpStatus($sent->getOtpId());
$current->getStatus(); // 'pending' | 'approved' | 'failed' | 'expired'
```

Useful for reconciliation and support tooling. It is not a substitute for `verifyOtp`, which is what
actually approves an OTP.

## Errors

Any non-2xx response throws `OtpCom\Sdk\ApiException`. The body is always
`{"error": {"type", "message", "details"?}}`, where `type` is a stable machine-readable class.

```php
use OtpCom\Sdk\ApiException;

try {
    $otp->sendOtp(new SendRequest(['recipient' => $recipient]));
} catch (ApiException $e) {
    $error = json_decode($e->getResponseBody(), true)['error'];
    error_log("{$e->getCode()} {$error['type']} {$error['message']}");
    throw $e;
}
```

| Status | When |
| --- | --- |
| `401` | Missing or invalid API key, disabled app, or suspended company |
| `404` | Unknown `otp_id` (also returned for another company's OTP, to avoid probing) |
| `409` | No enabled channel, channel not enabled, resend not allowed, or idempotency-key conflict |
| `422` | Request body failed validation |
| `429` | Resend cooldown has not elapsed |

## Configuration

```php
$config = Configuration::getDefaultConfiguration()
    ->setAccessToken(getenv('OTP_API_KEY'))          // required
    ->setHost('https://api.otp.com/api/v1')          // default
    ->setUserAgent('acme-checkout/2.1');

$client = new GuzzleHttp\Client(['timeout' => 10]);
$otp = new OtpApi($client, $config);
```

Pass your own Guzzle client to control timeouts, proxies, retries, and logging middleware.

## API reference

| Method | Endpoint | Returns |
| --- | --- | --- |
| [`sendOtp`](./docs/Api/OtpApi.md#sendotp) | `POST /otp/send` | [`OtpResponse`](./docs/Model/OtpResponse.md) |
| [`verifyOtp`](./docs/Api/OtpApi.md#verifyotp) | `POST /otp/verify` | [`VerifyResponse`](./docs/Model/VerifyResponse.md) |
| [`resendOtp`](./docs/Api/OtpApi.md#resendotp) | `POST /otp/resend` | [`OtpResponse`](./docs/Model/OtpResponse.md) |
| [`getOtpStatus`](./docs/Api/OtpApi.md#getotpstatus) | `GET /otp/{otp_id}` | [`OtpStatusResponse`](./docs/Model/OtpStatusResponse.md) |

## Regenerating

Everything in this repo except this README is generated from
[`openapi.yaml`](https://github.com/otp-com/sdk) by
[OpenAPI Generator](https://openapi-generator.tech). Fix the contract, not `lib/`; a pull request
against generated files will be overwritten by the next regeneration.

- **In CI:** run the **Regenerate from spec** workflow, or let `otp-com/sdk` dispatch it.
- **Locally:** `./update-sdk.sh sdk-php` from a checkout of `otp-com/sdk`.

`README.md` is listed in `.openapi-generator-ignore` so it survives regeneration. When the contract
changes, update it by hand.

## License

MIT
