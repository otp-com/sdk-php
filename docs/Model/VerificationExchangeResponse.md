# VerificationExchangeResponse

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **string** | The OTP this verification belongs to. |
**recipient** | **string** | The recipient that was verified, in full. This is the answer the device could not be trusted to give you. |
**recipient_type** | [**\OtpCom\Sdk\Model\RecipientType**](RecipientType.md) |  |
**channel** | [**\OtpCom\Sdk\Model\Channel**](Channel.md) | Channel the verified code was delivered on. |
**verified_at** | **\DateTime** | When the end user entered the correct code. |

[[Back to Model list]](../../README.md#models) [[Back to API list]](../../README.md#endpoints) [[Back to README]](../../README.md)
