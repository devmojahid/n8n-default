

## Base URL

| URL | Description |
|-----|-------------|
| https://graph.facebook.com |  |

## APIs

| Method | Endpoint |
|--------|----------|
| GET | [/{Version}/{Media-URL}](#get-version-media-url) |

<jumplink id="get-version-media-url"></jumplink>
## GET /{Version}/{Media-URL}

Download Media

Download media files using URLs obtained from media retrieval endpoints.
Requires User Access Token with whatsapp_business_messaging permission.
Media URLs expire after 5 minutes and must be re-retrieved if expired.
Returns binary content with appropriate MIME type headers.


### Responses

**200**

Media file downloaded successfully

**Content Type**: `application/octet-stream`

**Schema**: [MediaBinaryResponse](#mediabinaryresponse)

**400**

Bad Request - Invalid request parameters

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**401**

Unauthorized - Invalid or missing access token

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**404**

Not Found - Media not found or URL expired

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)

**500**

Internal Server Error

**Content Type**: `application/json`

**Schema**: [ErrorResponse](#errorresponse)


# Components

## Schemas

<jumplink id="mediabinaryresponse"></jumplink>
### MediaBinaryResponse

Binary content of the requested media file

**Type**: string

**Format**: binary

## Authentication

| Scheme | Type | Location |
|--------|------|----------|
| bearerAuth | HTTP Bearer | Header: `Authorization` |

### Usage Examples

- **bearerAuth**: Include `Authorization: Bearer your-token-here` in request headers

### Global Authentication Requirements

All endpoints require: bearerAuth
