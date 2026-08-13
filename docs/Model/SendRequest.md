# SendRequest

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**recipient** | **string** | Phone number (E.164) or email address to deliver the OTP to. |
**locale** | **string** | BCP-47 locale for the message template; falls back to the app default. | [optional]
**client_ip** | **string** | IP address of the end user who triggered this OTP (IPv4 or IPv6). Strongly recommended: requests without it share a much tighter per-app rate limit, and it feeds abuse protection for your own traffic. Private/reserved addresses count as absent. | [optional]

[[Back to Model list]](../../README.md#models) [[Back to API list]](../../README.md#endpoints) [[Back to README]](../../README.md)
