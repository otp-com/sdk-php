# OtpResponse

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **string** |  |
**status** | [**\OtpCom\Sdk\Model\Status**](Status.md) |  |
**channel** | [**\OtpCom\Sdk\Model\Channel**](Channel.md) |  |
**masked_recipient** | **string** | Recipient with the middle masked, e.g. +14****71. |
**action_url** | **string** | WhatsApp link the user opens to receive the code: they send us the prefilled message and we reply with the code over WhatsApp. Present only when the OTP was dispatched on the whatsapp channel; null otherwise. Verification is the same on every channel: the user enters the code and you call &#x60;/otp/verify&#x60;. | [optional]

[[Back to Model list]](../../README.md#models) [[Back to API list]](../../README.md#endpoints) [[Back to README]](../../README.md)
