# ErrorBody

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**type** | **string** | Machine-readable error class, e.g. \&quot;OtpNotFoundError\&quot;. |
**message** | **string** | Human-readable message. Safe to log; never contains the OTP code. |
**details** | **array<string,mixed>** | Structured context, present on validation errors ({loc, msg, type} per field) and a few domain errors. | [optional]

[[Back to Model list]](../../README.md#models) [[Back to API list]](../../README.md#endpoints) [[Back to README]](../../README.md)
