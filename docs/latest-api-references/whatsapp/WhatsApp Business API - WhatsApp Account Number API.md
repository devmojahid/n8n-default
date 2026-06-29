

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com | Production Graph API server |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{WhatsApp-Account-Number-ID}](#get-version-whatsapp-account-number-id) |

<jumplink id="get-version-whatsapp-account-number-id"></jumplink>
## GET /{Version}/{WhatsApp-Account-Number-ID}

Get WhatsApp Account Number Details

Retrieve comprehensive details about a WhatsApp Account Number, including its current status,
verification information, quality rating, and configuration settings.

**Use Cases:**
- Monitor account number status and quality rating
- Verify account number configuration before messaging operations
- Check verification and approval status
- Retrieve display name and business profile information

**Rate Limiting:**
Standard Graph API rate limits apply. Use appropriate retry logic with exponential backoff.

**Caching:**
Account number details can be cached for short periods, but status information may change
frequently. Implement appropriate cache invalidation strategies.


### Header Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| User-Agent | string |  | The user agent string identifying the client software making the request. |
| Authorization | string | ✓ | Bearer token for API authentication. This should be a valid access token obtained through the appropriate OAuth flow or system user token. |

### Path Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| Version | string | ✓ | Graph API version to use for this request. Determines the API behavior and available features. |
| WhatsApp-Account-Number-ID | string | ✓ | Your WhatsApp Account Number ID. This ID represents the account number entity and can be obtained from your WhatsApp Business Account phone numbers list. |

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| fields | string |  | Comma-separated list of fields to include in the response. If not specified, default fields will be returned (id, display_phone_number, status). Available fields: id, display_phone_number, verified_name, status, quality_rating, country_code, country_dial_code, code_verification_status, name_status, messaging_limit_tier, account_mode, certificate, is_official_business_account, host_platform, is_on_biz_app, is_preverified_number, last_onboarded_time, new_certificate, new_display_name, new_name_status, platform_type |

### Responses

**200**

Successfully retrieved WhatsApp Account Number details

**Content Type**: `application/json`

**Schema**: [WhatsAppAccountNumber](#whatsappaccountnumber)

**400**

Bad Request - Invalid parameters or malformed request

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Invalid parameter: account_number_id must be a valid numeric string",
        "type": "OAuthException",
        "code": 100,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn"
    }
}\n```

**401**

Unauthorized - Invalid or missing access token

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Invalid OAuth access token",
        "type": "OAuthException",
        "code": 190,
        "error_subcode": 463,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn"
    }
}\n```

**403**

Forbidden - Insufficient permissions or access denied

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Your app doesn't have permission to access this WhatsApp Account Number",
        "type": "OAuthException",
        "code": 200,
        "error_subcode": 1349174,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn",
        "error_user_title": "Permission Denied",
        "error_user_msg": "Your app doesn't have permission to access this resource"
    }
}\n```

**404**

Not Found - Account Number ID does not exist or is not accessible

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "WhatsApp Account Number not found",
        "type": "GraphMethodException",
        "code": 803,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn"
    }
}\n```

**422**

Unprocessable Entity - Request parameters are valid but cannot be processed

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "The requested fields are not available for this account number",
        "type": "GraphMethodException",
        "code": 100,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn"
    }
}\n```

**500**

Internal Server Error - Unexpected server error

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "An unexpected error occurred. Please retry your request",
        "type": "GraphMethodException",
        "code": 2,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn",
        "is_transient": true
    }
}\n```


# Components

## Schemas

<jumplink id="whatsappaccountnumber"></jumplink>
### WhatsAppAccountNumber

WhatsApp Account Number details and configuration

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Unique identifier for the WhatsApp Account Number |
| display_phone_number | string | ✓ | Phone number in international format for display purposes |
| verified_name | string |  | Business name verified for this phone number |
| status | [WhatsAppAccountNumberStatus](#whatsappaccountnumberstatus) | ✓ |  |
| quality_rating | [WhatsAppPhoneNumberQualityRating](#whatsappphonenumberqualityrating) |  |  |
| country_code | string |  | ISO 3166-1 alpha-2 country code |
| country_dial_code | string |  | Country dialing code |
| code_verification_status | [WhatsAppCodeVerificationStatus](#whatsappcodeverificationstatus) |  |  |
| name_status | [WhatsAppDisplayNameStatus](#whatsappdisplaynamestatus) |  |  |
| messaging_limit_tier | One of "TIER_100K", "TIER_10K", "TIER_1K", "TIER_250", "TIER_50", "TIER_NOT_SET", "TIER_UNLIMITED" |  | Current messaging limit tier for the account number |
| account_mode | [WhatsAppBusinessSandboxEligibility](#whatsappbusinesssandboxeligibility) |  |  |
| certificate | string |  | Business certificate information for the account number |
| is_official_business_account | boolean |  | Whether this is an official business account |
| host_platform | One of "CLOUD_API", "ON_PREMISE" |  | Platform hosting the WhatsApp Business Account phone number |
| is_on_biz_app | boolean |  | Whether this phone number is connected to the WhatsApp Business App |
| is_preverified_number | boolean |  | Whether this phone number was pre-verified |
| last_onboarded_time | integer (int64) |  | Unix timestamp of the last onboarding event for this phone number |
| new_certificate | string |  | New business certificate pending approval |
| new_display_name | string |  | New display name pending approval |
| new_name_status | [WhatsAppDisplayNameStatus](#whatsappdisplaynamestatus) |  |  |
| platform_type | One of "CLOUD_API", "ON_PREMISE" |  | Type of platform for the phone number |

<jumplink id="whatsappaccountnumberstatus"></jumplink>
### WhatsAppAccountNumberStatus

Current status of the WhatsApp Account Number

**Type**: string

**Enum Values**: "BANNED", "CONNECTED", "DELETED", "DISCONNECTED", "FLAGGED", "MIGRATED", "PENDING", "RATE_LIMITED", "RESTRICTED", "UNKNOWN", "UNVERIFIED"

<jumplink id="whatsappphonenumberqualityrating"></jumplink>
### WhatsAppPhoneNumberQualityRating

Quality rating based on message delivery and user feedback

**Type**: string

**Enum Values**: "GREEN", "NA", "RED", "UNKNOWN", "YELLOW"

<jumplink id="whatsappcodeverificationstatus"></jumplink>
### WhatsAppCodeVerificationStatus

Two-step verification status for the phone number

**Type**: string

**Enum Values**: "EXPIRED", "NOT_VERIFIED", "VERIFIED"

<jumplink id="whatsappdisplaynamestatus"></jumplink>
### WhatsAppDisplayNameStatus

Status of the display name associated with the phone number

**Type**: string

**Enum Values**: "APPROVED", "AVAILABLE_WITHOUT_REVIEW", "DECLINED", "EXPIRED", "NON_EXISTS", "NONE", "PENDING_REVIEW"

<jumplink id="whatsappbusinesssandboxeligibility"></jumplink>
### WhatsAppBusinessSandboxEligibility

Account mode indicating sandbox or live environment

**Type**: string

**Enum Values**: "LIVE", "SANDBOX"

<jumplink id="graphapierror"></jumplink>
### GraphAPIError

Standard Graph API error response

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| error | [Error](#object-error-1) | ✓ |  |

## Inline Object Definitions

<jumplink id="object-error-1"></jumplink>
### Error

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| message | string | ✓ | Human-readable error message |
| type | string | ✓ | Error category type |
| code | integer | ✓ | Numeric error code |
| error_subcode | integer |  | More specific error subcode when available |
| fbtrace_id | string |  | Unique identifier for debugging and support requests with Meta |
| is_transient | boolean |  | Indicates whether this error is temporary and the request should be retried |
| error_user_title | string |  | User-friendly error title for display purposes |
| error_user_msg | string |  | User-friendly error message for display purposes |

## Authentication

| Scheme | Type | Location |
|--------|------|----------|
| bearerAuth | HTTP Bearer | Header: `Authorization` |

### Usage Examples

- **bearerAuth**: Include `Authorization: Bearer your-token-here` in request headers

### Global Authentication Requirements

All endpoints require: bearerAuth
