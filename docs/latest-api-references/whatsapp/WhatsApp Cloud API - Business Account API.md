

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com |  |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{Business-ID}](#get-version-business-id) |

<jumplink id="get-version-business-id"></jumplink>
## GET /{Version}/{Business-ID}

Get Business Portfolio

Retrieve business portfolio information including name and timezone.

- Endpoint reference: [Business](https://developers.facebook.com/docs/marketing-api/reference/business/)

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| fields | string |  | Comma-separated list of fields to return (e.g., id,name,timezone_id) |

### Responses

**200**

Business portfolio information retrieved successfully

**Content Type**: `application/json`

**Schema**: [BusinessPortfolioResponse](#businessportfolioresponse)

**400**

Bad Request - Invalid request parameters

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**401**

Unauthorized - Invalid or missing access token

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**403**

Forbidden - Insufficient permissions

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**404**

Not Found - Business not found

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**500**

Internal Server Error

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)


# Components

## Schemas

<jumplink id="businessportfolioresponse"></jumplink>
### BusinessPortfolioResponse

Meta Business Portfolio account information

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  | Unique identifier for the business portfolio |
| name | string |  | Name of the business portfolio |
| timezone_id | integer |  | Timezone identifier for the business portfolio |

## Authentication

| Scheme | Type | Location |
|--------|------|----------|
| bearerAuth | HTTP Bearer | Header: `Authorization` |

### Usage Examples

- **bearerAuth**: Include `Authorization: Bearer your-token-here` in request headers

### Global Authentication Requirements

All endpoints require: bearerAuth
