

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com | Production Graph API server |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{Business-ID}/owned_whatsapp_business_accounts](#get-version-business-id-owned-whatsapp-business-accounts) |

<jumplink id="get-version-business-id-owned-whatsapp-business-accounts"></jumplink>
## GET /{Version}/{Business-ID}/owned_whatsapp_business_accounts

Get Owned WhatsApp Business Accounts

Retrieve WhatsApp Business Accounts owned by the specified business. This endpoint
provides comprehensive information about all WABAs owned by the business, including
account details, configuration, and status information.

**Use Cases:**
- Retrieve all WhatsApp Business Accounts owned by a business
- Filter accounts by business type
- Find specific accounts by ID
- Monitor business portfolio of WhatsApp Business Accounts
- Manage account access and permissions across multiple WABAs

**Rate Limiting:**
Standard Graph API rate limits apply. Use appropriate retry logic with exponential backoff.

**Caching:**
Account information can be cached for short periods, but status and configuration
may change frequently. Implement appropriate cache invalidation strategies.


### Header Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| User-Agent | string |  | The user agent string identifying the client software making the request. |
| Authorization | string | ✓ | Bearer token for API authentication. This should be a valid access token obtained through the appropriate OAuth flow or system user token. |

### Path Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| Version | string | ✓ | Graph API version to use for this request. Determines the API behavior and available features. |
| Business-ID | string | ✓ | Your Business ID. This ID represents the business portfolio that owns the WhatsApp Business Accounts and can be found in your Business Manager settings. |

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| business_type | array of [WhatsAppBusinessType](#whatsappbusinesstype) |  | Filter accounts by business type. Can specify multiple types as comma-separated values. Use this to filter between enterprise and small-medium business accounts. |
| after | string |  | Cursor for forward pagination. Use the cursor from the previous response to get the next page of results. |
| first | integer [min: 1, max: 100] |  | Number of results to return in forward pagination. Maximum value is 100. Use with 'after' cursor for forward pagination. |
| before | string |  | Cursor for backward pagination. Use the cursor from the previous response to get the previous page of results. |
| last | integer [min: 1, max: 100] |  | Number of results to return in backward pagination. Maximum value is 100. Use with 'before' cursor for backward pagination. |
| find | string |  | Find a specific WhatsApp Business Account by ID within the owned accounts. Use this to quickly locate a specific account. |

### Responses

**200**

Successfully retrieved owned WhatsApp Business Accounts

**Content Type**: `application/json`

**Schema**: [WhatsAppBusinessAccountsConnection](#whatsappbusinessaccountsconnection)

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
        "message": "Your app doesn't have permission to access this business's WhatsApp Business Accounts",
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
        "message": "The requested filter combination is not supported",
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

WhatsApp Business Account owned by the business

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Unique identifier for the WhatsApp Business Account |
| name | string | ✓ | Human-readable name of the WhatsApp Business Account |
| message_template_namespace | string | ✓ | Namespace identifier for message templates associated with this account |
| timezone_id | string | ✓ | Timezone identifier for the WhatsApp Business Account |

<jumplink id="whatsappbusinesstype"></jumplink>
### WhatsAppBusinessType

Type of WhatsApp Business Account

**Type**: string

**Enum Values**: "ENTERPRISE", "SMB"

<jumplink id="whatsappbusinessaccountonboardingfilter"></jumplink>
### WhatsAppBusinessAccountOnboardingFilter

Filter for accounts based on onboarding status

**Type**: string

**Enum Values**: "ONBOARDED", "NOT_ONBOARDED", "ALL"

<jumplink id="whatsappbusinessaccountsconnection"></jumplink>
### WhatsAppBusinessAccountsConnection

Paginated collection of owned WhatsApp Business Accounts

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| data | array of [WhatsAppBusinessAccount](#whatsappbusinessaccount) | ✓ | Array of owned WhatsApp Business Account records |
| paging | [CursorPaging](#cursorpaging) |  |  |

<jumplink id="cursorpaging"></jumplink>
### CursorPaging

Cursor-based pagination information

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| cursors | [Cursors](#object-cursors-1) |  |  |
| next | string |  | URL for the next page of results |
| previous | string |  | URL for the previous page of results |

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
| before | string |  | Cursor pointing to the start of the page of data that has been returned |
| after | string |  | Cursor pointing to the end of the page of data that has been returned |

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
