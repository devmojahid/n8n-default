

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com | Production Graph API server |

## APIs

| Method | Endpoint |
|--------|----------|
| POST | [/{Version}/{Business-ID}/onboard_partners_to_mm_lite](#post-version-business-id-onboard-partners-to-mm-lite) |

<jumplink id="post-version-business-id-onboard-partners-to-mm-lite"></jumplink>
## POST /{Version}/{Business-ID}/onboard_partners_to_mm_lite

Onboard Partners to MM Lite

Send a request from the partner to onboard an end business to MM Lite.
This creates a business agreement request and sets OBO mobility intents
on eligible WhatsApp Business Accounts.

**Business Flow:**
1. Validates partner business and app permissions
2. Checks end business eligibility for MM Lite
3. Identifies eligible shared WABAs and OBO WABAs
4. Sets mobility intents on eligible OBO WABAs (BSPs only)
5. Creates business agreement request
6. Returns request ID for tracking

**BSP vs TP Behavior:**
- BSPs (Business Solution Providers): Can create OBO WABAs and set mobility intents
- TPs (Technology Partners): Can only create onboarding requests, no OBO WABA management

**Rate Limiting:**
Subject to business partner integrations use case throttling limits.
Use appropriate retry logic with exponential backoff.

**Idempotency:**
If a pending request already exists between the same partner and end business,
the existing request ID is returned instead of creating a duplicate.


### Header Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| User-Agent | string |  | The user agent string identifying the client software making the request. |
| Authorization | string | ✓ | Bearer token for API authentication. This should be a valid access token obtained through the appropriate OAuth flow or system user token. |

### Path Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| Version | string | ✓ | Graph API version to use for this request. Determines the API behavior and available features. |
| Business-ID | string | ✓ | The end business ID that will receive the MM Lite onboarding request. This is the business that the partner wants to establish an MM Lite relationship with. |

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| solution_id | string |  | Optional WhatsApp Business Solution ID to associate with the onboarding request. If provided, the solution will be linked to the business agreement and OBO mobility intents. |

### Responses

**200**

Successfully created MM Lite onboarding request

**Content Type**: `application/json`

**Schema**: [MMLiteOnboardingRequest](#mmliteonboardingrequest)

**400**

Bad Request - Invalid parameters or malformed request

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

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

<jumplink id="mmliteonboardingrequest"></jumplink>
### MMLiteOnboardingRequest

MM Lite onboarding request details

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| request_id | string | ✓ | Unique identifier for the MM Lite onboarding request |

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
