# OtpResponse

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **string** |  |
**status** | [**\OtpCom\Sdk\Model\Status**](Status.md) |  |
**channel** | [**\OtpCom\Sdk\Model\Channel**](Channel.md) | Channel the OTP was dispatched on; null until routed. |
**masked_recipient** | **string** | Recipient with the middle digits masked. |
**action_url** | **string** | WhatsApp link the user opens to receive the code: they send us the prefilled message and we reply with the code over WhatsApp, then they enter it and you call POST /otp/verify. Present only when dispatched on the whatsapp channel; null otherwise. | [optional]

[[Back to Model list]](../../README.md#models) [[Back to API list]](../../README.md#endpoints) [[Back to README]](../../README.md)
