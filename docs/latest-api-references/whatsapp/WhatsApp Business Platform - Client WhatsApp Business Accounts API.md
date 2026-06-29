

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com | Production Graph API server |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{Business-ID}/client_whatsapp_business_accounts](#get-version-business-id-client-whatsapp-business-accounts) |

<jumplink id="get-version-business-id-client-whatsapp-business-accounts"></jumplink>
## GET /{Version}/{Business-ID}/client_whatsapp_business_accounts

Get Client WhatsApp Business Accounts

Retrieve a list of WhatsApp Business Accounts that have been shared with the specified business.

**Use Cases:**
- Monitor shared WABA relationships and permissions
- Verify WABA configuration and status information
- Retrieve WABA details for business integrations
- Manage multi-business WhatsApp messaging setups

**Rate Limiting:**
Standard Graph API rate limits apply. Use appropriate retry logic with exponential backoff.

**Caching:**
WABA information can be cached for moderate periods, but status information may change
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
| Business-ID | string | ✓ | Your Business ID. This ID can be found in your Meta Business Suite URL or through business management APIs. |

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| fields | string |  | Comma-separated list of fields to include in the response. If not specified, default fields will be returned (id, name, currency, timezone_id). Available fields: id, name, account_review_status, purchase_order_number, audiences, ownership_type, subscribed_apps, business_verification_status, country, currency, timezone_id, on_behalf_of_business_info, schedules, is_enabled_for_insights, dcc_config, message_templates, phone_numbers |
| business_type | array of One of "STANDARD", "PREMIUM", "ENTERPRISE" |  | Filter results by WhatsApp Business Account type |
| limit | integer [min: 1, max: 100] |  | Maximum number of WhatsApp Business Accounts to return per page. Default is 25, maximum is 100. |
| after | string |  | Cursor for pagination. Use this to get the next page of results. |
| before | string |  | Cursor for pagination. Use this to get the previous page of results. |
| find | string |  | Find a specific WhatsApp Business Account by ID |

### Responses

**200**

Successfully retrieved client WhatsApp Business Accounts

**Content Type**: `application/json`

**Schema**: [ClientWhatsAppBusinessAccountsResponse](#clientwhatsappbusinessaccountsresponse)

**400**

Bad Request - Invalid parameters or malformed request

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Invalid parameter: business_id must be a valid numeric string",
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
        "message": "Your app doesn't have permission to access client WhatsApp Business Accounts for this business",
        "type": "OAuthException",
        "code": 200,
        "error_subcode": 1349174,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn",
        "error_user_title": "Permission Denied",
        "error_user_msg": "Your app doesn't have permission to access this resource"
    }
}\n```

**404**

Not Found - Business ID does not exist or is not accessible

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Business not found",
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
        "message": "The requested fields are not available for this business",
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

<jumplink id="whatsappbusinessaccount"></jumplink>
### WhatsAppBusinessAccount

WhatsApp Business Account details and configuration

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Unique identifier for the WhatsApp Business Account |
| name | string | ✓ | Human-readable name of the WhatsApp Business Account |
| account_review_status | [AccountReviewStatus](#accountreviewstatus) |  |  |
| purchase_order_number | string |  | Purchase order number associated with the account |
| audiences | array of string |  | List of audience segments associated with the account |
| ownership_type | [OwnershipType](#ownershiptype) |  |  |
| subscribed_apps | array of [SubscribedApp](#subscribedapp) |  | List of applications subscribed to this WABA |
| business_verification_status | [BusinessVerificationStatus](#businessverificationstatus) |  |  |
| country | string |  | Country code where the WABA is registered |
| currency | string |  | Currency code for the WABA |
| timezone_id | string |  | Timezone identifier for the WABA |
| on_behalf_of_business_info | [OnBehalfOfBusinessInfo](#onbehalfofbusinessinfo) |  |  |
| schedules | array of [BusinessSchedule](#businessschedule) |  | Business hours and scheduling information |
| is_enabled_for_insights | boolean |  | Whether insights are enabled for this WABA |
| dcc_config | [DCCConfig](#dccconfig) |  |  |
| message_templates | array of [MessageTemplate](#messagetemplate) |  | Message templates associated with the WABA |
| phone_numbers | array of [PhoneNumber](#phonenumber) |  | Phone numbers associated with the WABA |

<jumplink id="accountreviewstatus"></jumplink>
### AccountReviewStatus

Review status of the WhatsApp Business Account

**Type**: string

**Enum Values**: "APPROVED", "PENDING", "REJECTED", "RESTRICTED"

<jumplink id="ownershiptype"></jumplink>
### OwnershipType

Type of ownership for the WhatsApp Business Account

**Type**: string

**Enum Values**: "SELF_OWNED", "CLIENT_OWNED", "AGENCY_OWNED"

<jumplink id="businessverificationstatus"></jumplink>
### BusinessVerificationStatus

Verification status of the business associated with the WABA

**Type**: string

**Enum Values**: "VERIFIED", "UNVERIFIED", "PENDING", "REJECTED"

<jumplink id="subscribedapp"></jumplink>
### SubscribedApp

Application subscribed to the WABA

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  | Application ID |
| name | string |  | Application name |

<jumplink id="onbehalfofbusinessinfo"></jumplink>
### OnBehalfOfBusinessInfo

Information about the business on whose behalf the WABA operates

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  | Business ID |
| name | string |  | Business name |

<jumplink id="businessschedule"></jumplink>
### BusinessSchedule

Business hours schedule information

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| day_of_week | One of "MONDAY", "TUESDAY", "WEDNESDAY", "THURSDAY", "FRIDAY", "SATURDAY", "SUNDAY" |  |  |
| open_time | string |  | Opening time in HH:MM format |
| close_time | string |  | Closing time in HH:MM format |

<jumplink id="dccconfig"></jumplink>
### DCCConfig

Data and Content Control configuration

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| enabled | boolean |  | Whether DCC is enabled |
| policy_url | string |  | URL to the DCC policy |

<jumplink id="messagetemplate"></jumplink>
### MessageTemplate

Message template information

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  | Template ID |
| name | string |  | Template name |
| status | One of "APPROVED", "PENDING", "REJECTED", "DISABLED" |  |  |

<jumplink id="phonenumber"></jumplink>
### PhoneNumber

Phone number associated with the WABA

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  | Phone number ID |
| display_phone_number | string |  | Formatted phone number for display |
| verified_name | string |  | Verified business name for the phone number |

<jumplink id="clientwhatsappbusinessaccountsresponse"></jumplink>
### ClientWhatsAppBusinessAccountsResponse

Response containing list of client WhatsApp Business Accounts

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| data | array of [WhatsAppBusinessAccount](#whatsappbusinessaccount) |  | Array of client WhatsApp Business Accounts |
| paging | [CursorPaging](#cursorpaging) |  |  |

<jumplink id="cursorpaging"></jumplink>
### CursorPaging

Cursor-based pagination information

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| cursors | [Cursors](#object-cursors-1) |  |  |
| previous | string |  | URL for the previous page of results |
| next | string |  | URL for the next page of results |

<jumplink id="graphapierror"></jumplink>
### GraphAPIError

Standard Graph API error response

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| error | [Error](#object-error-2) | ✓ |  |

## Inline Object Definitions

<jumplink id="object-cursors-1"></jumplink>
### Cursors

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| before | string |  | Cursor pointing to the start of the page of data |
| after | string |  | Cursor pointing to the end of the page of data |

<jumplink id="object-error-2"></jumplink>
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
