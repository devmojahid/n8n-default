

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com | Production Graph API server |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{User-ID}/assigned_whatsapp_business_accounts](#get-version-user-id-assigned-whatsapp-business-accounts) |

<jumplink id="get-version-user-id-assigned-whatsapp-business-accounts"></jumplink>
## GET /{Version}/{User-ID}/assigned_whatsapp_business_accounts

Get Assigned WhatsApp Business Accounts

Retrieve WhatsApp Business Accounts that have been assigned to a specific user.
This endpoint provides information about account assignments, permissions, and
current status corresponding to the GraphAssignedWhatsAppBusinessAccountsEdge node.

**Use Cases:**
- Retrieve all WhatsApp Business Accounts assigned to a user
- Check user permissions for specific accounts
- Monitor account assignment status and changes
- Validate user access before performing business operations

**Rate Limiting:**
Standard Graph API rate limits apply. Use appropriate retry logic with exponential backoff.

**Caching:**
Assignment information can be cached for short periods, but permissions and status
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
| User-ID | string | ✓ | The user ID for whom to retrieve assigned WhatsApp Business Accounts. This must be a valid user ID within your solution or partnership. |

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| fields | string |  | Comma-separated list of fields to include in the response. If not specified, default fields will be returned (id, name). Available fields: id, name |
| limit | integer [min: 1, max: 100] |  | Maximum number of accounts to return per page. Default is 25, maximum is 100. |
| after | string |  | Cursor for pagination. Use this to get the next page of results. |
| before | string |  | Cursor for pagination. Use this to get the previous page of results. |

### Responses

**200**

Successfully retrieved assigned WhatsApp Business Accounts

**Content Type**: `application/json`

**Schema**: [AssignedAccountsResponse](#assignedaccountsresponse)

**400**

Bad Request - Invalid parameters or malformed request

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "Invalid parameter: user_id must be a valid numeric string",
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
        "message": "Your app doesn't have permission to access assigned WhatsApp Business Accounts for this user",
        "type": "OAuthException",
        "code": 200,
        "error_subcode": 1349174,
        "fbtrace_id": "AXsgnV2Cm3ZMGF3dF_cfYIn",
        "error_user_title": "Permission Denied",
        "error_user_msg": "Your app doesn't have permission to access this resource"
    }
}\n```

**404**

Not Found - User ID does not exist or is not accessible

**Content Type**: `application/json`

**Schema**: [GraphAPIError](#graphapierror)

**Example**:\n```json\n{
    "error": {
        "message": "User not found",
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
        "message": "The requested fields are not available for this user",
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

<jumplink id="assignedwhatsappbusinessaccount"></jumplink>
### AssignedWhatsAppBusinessAccount

WhatsApp Business Account assigned to a user

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Unique identifier for the WhatsApp Business Account |
| name | string |  | Display name of the WhatsApp Business Account |

<jumplink id="assignedaccountsresponse"></jumplink>
### AssignedAccountsResponse

Response containing assigned WhatsApp Business Accounts

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| data | array of [AssignedWhatsAppBusinessAccount](#assignedwhatsappbusinessaccount) | ✓ | List of assigned WhatsApp Business Accounts |
| paging | [PagingInfo](#paginginfo) |  |  |

<jumplink id="paginginfo"></jumplink>
### PagingInfo

Pagination information for the response

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
| before | string |  | Cursor for the previous page |
| after | string |  | Cursor for the next page |

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
