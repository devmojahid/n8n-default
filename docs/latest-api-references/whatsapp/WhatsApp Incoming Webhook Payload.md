

## Base URL

No base URL specified.

## APIs

| Method | Endpoint |
|--------|----------|
| POST | [/whatsapp/webhooks](#post-whatsapp-webhooks) |

<jumplink id="post-whatsapp-webhooks"></jumplink>
## POST /whatsapp/webhooks

Receive incoming WhatsApp messages

Endpoint for receiving webhook payloads for diverse incoming WhatsApp message types.

### Request Body (Required)

Webhook payload containing incoming WhatsApp messages.

**Content Type**: `application/json`

**Schema**: [WebhookPayload](#webhookpayload)

### Responses

**200**

Webhook received successfully


# Components

## Schemas

<jumplink id="webhookpayload"></jumplink>
### WebhookPayload

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| object | string | ✓ | Always 'whatsapp_business_account' for these webhooks. |
| entry | array of [Entry](#entry) | ✓ |  |

<jumplink id="entry"></jumplink>
### Entry

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | WhatsApp Business Account ID. |
| changes | array of [Change](#change) | ✓ |  |

<jumplink id="change"></jumplink>
### Change

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| value | Must be one of: [IncomingMessageValueGeneral](#incomingmessagevaluegeneral), [IncomingMessageValueSystem](#incomingmessagevaluesystem), [StatusMessageValue](#statusmessagevalue), [GroupValue](#groupvalue) | ✓ |  |
| field | One of "messages", "group_lifecycle_update", "group_settings_update", "group_participant_update" | ✓ | The field indicate to what object is the webhook related: - messages: the webhook is related to messages from consumer or status of message sent by business to consumer. - group_lifecycle_update: the webhook is related to group creation and deletion. - group_settings_update: the webhook is related to group settings update. - group_participant_update: the webhook is related to participants joining and leaving the groups. |

<jumplink id="metadata"></jumplink>
### Metadata

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| display_phone_number | string | ✓ | Business display phone number. |
| phone_number_id | string | ✓ | Business phone number ID. |

<jumplink id="contactprofile"></jumplink>
### ContactProfile

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| profile | [Profile](#object-profile-1) | ✓ |  |
| wa_id | string |  | WhatsApp user ID. Note that a WhatsApp user's ID and phone number may not always match. |

<jumplink id="incomingmessagevaluegeneral"></jumplink>
### IncomingMessageValueGeneral

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| messaging_product | string | ✓ | Always 'whatsapp'. |
| metadata | [Metadata](#metadata) | ✓ |  |
| contacts | array of [ContactProfile](#contactprofile) | ✓ | Array of contact profiles for the sender. Included for all non-system incoming messages. |
| messages | array of [IncomingMessage](#incomingmessage) | ✓ | Array of message objects. The structure varies based on the 'type' property. |

<jumplink id="incomingmessagevaluesystem"></jumplink>
### IncomingMessageValueSystem

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| messaging_product | string | ✓ | Always 'whatsapp'. |
| metadata | [Metadata](#metadata) | ✓ |  |
| messages | array of [SystemMessage](#systemmessage) | ✓ | Array containing only 'system' message objects. |

<jumplink id="statusmessagevalue"></jumplink>
### StatusMessageValue

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| messaging_product | string | ✓ | Always 'whatsapp'. |
| metadata | [Metadata](#metadata) | ✓ |  |
| statuses | array of [Statuses](#statuses) | ✓ | Array of status objects. |

<jumplink id="statuses"></jumplink>
### Statuses

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Unique WhatsApp message ID the status is associated with. |
| status | One of "sent", "delivered", "read", "failed" | ✓ |  |
| timestamp | string | ✓ |  |
| recipient_id | string | ✓ | Recipient phone number. |
| group_id | string |  | Group ID if the message was sent to a group. |
| conversation | [Conversation](#conversation) |  |  |
| pricing | [Pricing](#pricing) |  |  |
| errors | array of [StatusError](#statuserror) |  |  |

<jumplink id="conversation"></jumplink>
### Conversation

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string |  |  |
| expiration_timestamp | string |  |  |
| origin | [ConversationOrigin](#conversationorigin) |  |  |

<jumplink id="conversationorigin"></jumplink>
### ConversationOrigin

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| type | string |  |  |

<jumplink id="pricing"></jumplink>
### Pricing

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| billable | boolean |  |  |
| pricing_model | One of "CBP", "PMP" |  |  |
| category | string |  |  |

<jumplink id="statuserror"></jumplink>
### StatusError

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| code | integer |  |  |
| title | string |  |  |
| message | string |  |  |
| error_data | [ErrorData](#errordata) |  |  |
| href | string |  |  |

<jumplink id="errordata"></jumplink>
### ErrorData

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| details | string |  |  |

<jumplink id="groupvalue"></jumplink>
### GroupValue

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| messaging_product | string | ✓ | Always 'whatsapp'. |
| metadata | [Metadata](#metadata) | ✓ |  |
| groups | array of [Groups](#groups) | ✓ | Array of group objects. |

<jumplink id="groups"></jumplink>
### Groups

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| timestamp | integer | ✓ | Unix timestamp of the group event |
| group_id | string | ✓ | Unique identifier for the group |
| type | One of "group_create", "group_delete", "group_settings_update", "group_add_participants", "group_remove_participants" | ✓ | Type of group event |
| request_id | string | ✓ | Unique identifier for the request |
| subject | string |  | Group subject/name |
| description | string |  | Group description |
| added_participants | array of [GroupParticipant](#groupparticipant) |  | List of participants added to the group |
| removed_participants | array of [GroupParticipant](#groupparticipant) |  | List of participants removed from the group |
| profile_picture | [GroupProfilePicture](#groupprofilepicture) |  | Group profile picture information |

<jumplink id="groupparticipant"></jumplink>
### GroupParticipant

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| input | string |  | Input phone number or WhatsApp ID |
| wa_id | string |  | WhatsApp ID of the participant |

<jumplink id="groupprofilepicture"></jumplink>
### GroupProfilePicture

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| mime_type | string |  | MIME type of the profile picture |
| sha256 | string |  | SHA256 hash of the profile picture |

<jumplink id="basemessageproperties"></jumplink>
### BaseMessageProperties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| from | string | ✓ | WhatsApp user phone number. Note that a WhatsApp user's phone number and ID may not always match. |
| id | string | ✓ | Unique WhatsApp message ID. |
| timestamp | string | ✓ | Unix timestamp indicating when the webhook was triggered. |

<jumplink id="incomingmessage"></jumplink>
### IncomingMessage

**Type**: object

<jumplink id="textmessage"></jumplink>
### TextMessage

<jumplink id="contextobject"></jumplink>
### ContextObject

Only included if message via a "Message business" button.

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| from | string | ✓ | Business display phone number. |
| id | string | ✓ | WhatsApp message ID of the message the user used to access the "Message business" button. |
| referred_product | [Referred_product](#object-referred_product-2) | ✓ |  |

<jumplink id="referralobject"></jumplink>
### ReferralObject

Only included if message via a Click to WhatsApp ad.

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| source_url | string | ✓ | Ad URL. |
| source_id | string | ✓ | Ad ID. |
| source_type | One of "ad", "post" | ✓ |  |
| body | string | ✓ | Ad primary text. |
| headline | string | ✓ | Ad headline. |
| media_type | One of "image", "video" | ✓ |  |
| image_url | string |  | Only included for image media_type. |
| video_url | string |  | Only included for video media_type. |
| thumbnail_url | string |  | Only included for video media_type. |
| ctwa_clid | string | ✓ | Ad click ID. |
| welcome_message | [Welcome_message](#object-welcome_message-3) |  |  |

<jumplink id="reactionmessage"></jumplink>
### ReactionMessage

<jumplink id="mediamessageproperties"></jumplink>
### MediaMessageProperties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| mime_type | string | ✓ | Media asset MIME type. |
| sha256 | string | ✓ | Media asset SHA-256 hash. |
| id | string | ✓ | Media asset ID. A GET request on this ID can provide the asset URL. |

<jumplink id="audiomessage"></jumplink>
### AudioMessage

<jumplink id="documentmessage"></jumplink>
### DocumentMessage

<jumplink id="imagemessage"></jumplink>
### ImageMessage

<jumplink id="stickermessage"></jumplink>
### StickerMessage

<jumplink id="videomessage"></jumplink>
### VideoMessage

<jumplink id="locationmessage"></jumplink>
### LocationMessage

<jumplink id="contactsharingmessage"></jumplink>
### ContactSharingMessage

<jumplink id="contactobject"></jumplink>
### ContactObject

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| addresses | array of [Addresses](#object-addresses-4) |  |  |
| birthday | string (date) |  | Contact birthday (YYYY-MM-DD). |
| emails | array of [Emails](#object-emails-5) |  |  |
| name | [Name](#object-name-6) |  |  |
| org | [Org](#object-org-7) |  |  |
| phones | array of [Phones](#object-phones-8) |  |  |
| urls | array of [Urls](#object-urls-9) |  |  |

<jumplink id="unsupportedmessage"></jumplink>
### UnsupportedMessage

<jumplink id="interactivemessagereply"></jumplink>
### InteractiveMessageReply

<jumplink id="interactivelistreplycontent"></jumplink>
### InteractiveListReplyContent

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| list_reply | [List_reply](#object-list_reply-10) | ✓ |  |

<jumplink id="interactivebuttonreplycontent"></jumplink>
### InteractiveButtonReplyContent

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| button_reply | [Button_reply](#object-button_reply-11) | ✓ |  |

<jumplink id="ordermessage"></jumplink>
### OrderMessage

<jumplink id="systemmessage"></jumplink>
### SystemMessage

## Inline Object Definitions

<jumplink id="object-profile-1"></jumplink>
### Profile

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| name | string | ✓ | WhatsApp user's name as it appears in their profile in the WhatsApp client. |

<jumplink id="object-referred_product-2"></jumplink>
### Referred_product

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| catalog_id | string | ✓ | Product catalog ID. |
| product_retailer_id | string | ✓ | Product ID. |

<jumplink id="object-welcome_message-3"></jumplink>
### Welcome_message

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| text | string | ✓ | Ad greeting text. |

<jumplink id="object-addresses-4"></jumplink>
### Addresses

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| city | string |  | City mentioned in the contact address |
| country | string |  | Country mentioned in the contact address |
| country_code | string |  | ISO country code for the contact address |
| state | string |  | State mentioned in the contact address |
| street | string |  | Street mentioned in the contact address |
| type | string |  | Type of address, such as home or work |
| zip | string |  | Zip code in the contact address |

<jumplink id="object-emails-5"></jumplink>
### Emails

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| email | string (email) |  | Email address of the contact |
| type | string |  | Type of email, such as personal or work |

<jumplink id="object-name-6"></jumplink>
### Name

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| formatted_name | string | ✓ | Contact's formatted name |
| first_name | string |  | Contact’s first name |
| last_name | string |  | Contact’s last name |
| middle_name | string |  | Contact’s middle name |
| suffix | string |  | Contact’s name suffix |
| prefix | string |  | Contact’s name prefix |

<jumplink id="object-org-7"></jumplink>
### Org

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| company | string |  | Name of the company where the contact works |
| department | string |  | Name of the department where the contact works |
| title | string |  | Contact's job title |

<jumplink id="object-phones-8"></jumplink>
### Phones

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| phone | string |  | Contact’s Phone number |
| wa_id | string |  | Contact's WhatsApp Number. Note that a WhatsApp user's ID and phone number may not always match. |
| type | string |  | Type of phone number. For example, cell, mobile, main, iPhone, home, work, etc. |

<jumplink id="object-urls-9"></jumplink>
### Urls

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| url | string (uri) |  | Website URL associated with the contact or their company |
| type | string |  | Type of website. For example, company, work, personal, Facebook Page, Instagram, etc. |

<jumplink id="object-list_reply-10"></jumplink>
### List_reply

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Row ID. |
| title | string | ✓ | Row title. |
| description | string | ✓ | Row description. |

<jumplink id="object-button_reply-11"></jumplink>
### Button_reply

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| id | string | ✓ | Button ID. |
| title | string | ✓ | Button label text. |

## Authentication

| Scheme | Type | Location |
|--------|------|----------|
| bearerAuth | HTTP Bearer | Header: `Authorization` |

### Usage Examples

- **bearerAuth**: Include `Authorization: Bearer your-token-here` in request headers

### Global Authentication Requirements

All endpoints require: bearerAuth
