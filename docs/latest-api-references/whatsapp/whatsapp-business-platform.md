@read those all links fetch all data from this -> https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform,
https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens, https://developers.facebook.com/documentation/business-messaging/whatsapp/permissions, https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started, https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/get-started

Demo the API
Preview an interactive experience showing how Jasper’s Market (our demo retail business) connects with customers using the WhatsApp Business Platform.
Demo retail business
Download the sample app
Select language

cURL

curl -X POST 'https://graph.facebook.com/<API_VERSION>/<WHATSAPP_BUSINESS_PHONE_NUMBER_ID>/messages' \
  --header 'Authorization: Bearer <ACCESS_TOKEN>' \
  --header 'Content-Type: application/json' \
  --data '{
    "messaging_product": "whatsapp",
    "to": "<WHATSAPP_USER_PHONE_NUMBER>",
    "type": "template",
    "template": {
      "name": "hello_world",
      "language": { "code": "en_US" }
    }
  }'
  
Essentials
Start building conversational experiences that delight your customers


Docs
Overview
About the WhatsApp Business Platform
About the WhatsApp Business Platform
Updated: Jun 2, 2026
The WhatsApp Business Platform enables businesses to communicate with customers at scale.
This documentation is intended for developers using our APIs. If you are looking for information on other ways to use WhatsApp for your business see the WhatsApp Business site⁠.
Core APIs and capabilities
WhatsApp Business Platform Cloud API
The WhatsApp Business Platform Cloud API enables you to programmatically message and call on WhatsApp. You can use Cloud API to send users a variety of messages, from simple text messages to rich media and interactive messages.
Cloud API includes:
Messaging: Send text messages, rich media, and interactive messages
Calling: Make and receive calls to customers
Groups: Create, manage, and message WhatsApp group conversations
WhatsApp messaging lets you send encrypted messages to customers. Use Cloud API to:
Send order confirmations and shipping updates
Share appointment availability and other reminders
Send messages that recommend related or additional products
Facilitate end-to-end transactions, from product discovery to payment
Enable multi-factor authentication or one-time passwords to verify accounts and users
Deliver custom interactive conversational experiences
Learn more about message types on Cloud API.
WhatsApp Business Platform Business Management API
The WhatsApp Business Management API enables you to programmatically manage a WhatsApp Business account and its associated assets.
Manage account assets with Business Management API like:
Business phone numbers: Add and remove phone numbers associated with your business.
Templates: Create and modify message templates for scalable messaging.
Business Management API also gives you access to account analytics like:
Messaging analytics: The number and type of messages sent and delivered.
Pricing analytics: Granular pricing breakdowns for delivered messages.
Template analytics: Sent/read/delivered template metrics, alongside template message button clicks.
Learn more about message templates.
Learn more about managing business phone numbers.
Learn more about account analytics.
Marketing Messages API for WhatsApp
MM API for WhatsApp is an API for sending optimized marketing messages on WhatsApp.
When you send marketing messages through the MM API for WhatsApp, you can access new features not available on Cloud API and get automatic optimizations, so high engagement messages can reach more customers.
The MM API for WhatsApp includes:
Quality-based delivery: Up to 9% higher marketing message deliveries over Cloud API for high engagement content.
Automated creative optimizations: Automatic enhancements to marketing creative to increase message performance.
Performance benchmarks and recommendations: Comparison of read and click rates versus similar templates from businesses in your region.
Conversion metrics: Measure marketing messages that lead users to perform app events such as ‘Add to Cart’, ‘Checkout Initiated’, or ‘Purchase’.
Learn more about the Marketing Messages API for WhatsApp.
Webhooks
Webhooks deliver JSON payloads to your server for message status updates, incoming messages, asynchronous error handling, and many other notification utilities.
The platform relies heavily on webhooks, as the contents of any message sent from a WhatsApp user to your business phone number is communicated via webhook, and all outgoing message delivery status updates are reported via webhook.
Learn more about webhooks.
Meta Business Agent
Meta Business Agent lets you configure and operate AI-powered agents on WhatsApp. Agents handle conversations with WhatsApp users autonomously, using knowledge sources you provide (FAQs, business info, product catalogs, files, and websites) and custom connectors to your external APIs.
Meta Business Agent includes:
Agent configuration: Set agent behavior, persona, language, and system instructions
Knowledge management: Connect FAQs, business info, catalogs, files, and websites for the agent to reference
Custom connectors: Integrate your external APIs so the agent can take actions like checking order status or booking appointments
Thread control: Manage handoffs between the AI agent and human agents
Evaluation and testing: Test agent responses and evaluate performance
Learn more about Meta Business Agent.
Technical foundations
HTTP protocol and API requests
The WhatsApp Business Platform is built on Graph API and uses HTTP protocol. API requests include path, body, and header parameters.
Example: Sending a text message using cURL
curl 'https://graph.facebook.com/v17.0/106540352242922/messages' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer EAAJB...' \
  -d '{
    "messaging_product": "whatsapp",
    "recipient_type": "individual",
    "to": "+16505555555",
    "type": "text",
    "text": {
      "preview_url": true,
      "body": "Here's the info you requested! https://www.meta.com/quest/quest-3/"
    }
  }'
Learn more about the Graph API.
JSON responses
API responses are formatted in JSON.
Example: Requesting a business phone number’s metadata
{
  "verified_name": "Lucky Shrub",
  "code_verification_status": "VERIFIED",
  "display_phone_number": "15550783881",
  "quality_rating": "GREEN",
  "platform_type": "CLOUD_API",
  "throughput": {
    "level": "STANDARD"
  },
  "webhook_configuration": {
    "application": "https://www.luckyshrub.com/webhooks"
  },
  "id": "106540352242922"
}
Authentication and authorization
Authentication uses OAuth (not 2.0) access tokens, while permissions restrict access to specific resources.
Learn more about access tokens.
Learn more about permissions.
Key resources
Business portfolios
A business portfolio allows organizations to bring all their Meta business assets together so they can be managed in one place. On the WhatsApp Business Platform, a business portfolio mainly serves as a container for WhatsApp Business Accounts (WABA). You must have a business portfolio to use the platform.
Business portfolios can be verified, and verification status factors into improved functionality, such as higher throughput and Official Business Account status.
Learn more about business portfolios.⁠
WhatsApp Business Accounts (WABA)
A WhatsApp Business account represents your business and contains phone numbers, usernames, and analytics.
Learn more about WhatsApp Business Accounts.
Business phone numbers
Business phone numbers, real, or virtual, are used for sending and receiving WhatsApp messages. They can have display names and earn Official Business Account status.
Learn more about business phone numbers.
Message templates
Templates are customizable messages that you can construct in advance of sending them. Template messages generally require approval before you can send them.
Templates are useful for messaging at scale. They are also the only type of message that can be sent to WhatsApp users outside of a customer service window.
Templates have quality scores and are subject to various messaging limits.
Learn more about message templates.
Test resources
When you get started with Cloud API, a test WhatsApp Business account and test business phone number are automatically created for you. Test WhatsApp Business accounts and test phone numbers are useful for testing purposes, as they have relaxed messaging limits and don’t require a payment method on file in order to send template messages.
You can delete your business portfolio and its test resources if:
You are an admin on the business portfolio associated with the app
No other apps are associated with the business portfolio
The business portfolio is not associated with any other WhatsApp Business Accounts
The WhatsApp Business account is not associated with any other business phone numbers.
To delete your business portfolio and its test resources:
Go to the App Dashboard > WhatsApp > Configuration panel.
Locate the Test Account section.
Click the Delete button.
Use the API Playground when testing endpoints. Find the playground in the “API Reference” section on the left sidebar of this page. In each reference, click the “Try it” button to open the playground.
Also helpful for testing is our Postman collection⁠.
Tools and integrations
WhatsApp Manager
WhatsApp Manager is a web app for managing WhatsApp Business Accounts, Messaging Accounts, phone numbers, templates, and reviewing analytics.
Access WhatsApp Manager⁠
Third-party SDKs
Some SDKs, like PyWa⁠ (Python wrapper), are available but are not maintained or endorsed by Meta.
Postman collection
The official Postman collection lets you execute common API queries.
Access the WhatsApp Business Platform Postman collection⁠.
Security and performance
Throughput
Business phone numbers can send up to 80 messages per second by default, with capacity upgrades available.
Learn more about throughput.
Encryption
With Cloud API, every WhatsApp message continues to be protected by Signal protocol encryption that secures messages before they leave the device. Signal protocol encryption ensures that messages with a WhatsApp Business account are securely delivered to the destination chosen by each business.
Cloud API uses industry standard encryption techniques to protect data in transit and at rest. Cloud API uses Graph API for sending messages and Webhooks for receiving events, and both operate over industry standard HTTPS, protected by TLS.
See the WhatsApp Encryption Overview technical whitepaper⁠ for additional details.
Scaling
The Cloud API automatically scales usage within your rate limits.
Rate limits
Requests made by your app on your WhatsApp Business Account (WABA) are counted against your app’s request count. An app’s request count is the number of requests it can make during a rolling one hour.
For the following endpoints, your app can make 200 requests per hour, per app, per WABA, by default. For active WABAs with at least one registered phone number, your app can make 5000 requests per hour, per app, per active WABA.
Type of request	Endpoint
GET
/<WHATSAPP_BUSINESS_ACCOUNT_ID>
GET, POST, and DELETE
/<WHATSAPP_BUSINESS_ACCOUNT_ID>/assigned_users
GET
/<WHATSAPP_BUSINESS_ACCOUNT_ID>/phone_numbers
GET, POST, and DELETE
/<WHATSAPP_BUSINESS_ACCOUNT_ID>/message_templates
GET, POST, and DELETE
/<WHATSAPP_BUSINESS_ACCOUNT_ID>/subscribed_apps
GET
/<WHATSAPP_BUSINESS_ACCOUNT_TO_NUMBER_CURRENT_STATUS_ID>
For the following Credit Line API requests, your app can make 5000 requests per hour.
Type of request	Endpoint
GET
/<BUSINESS_ID>/extendedcredits
POST
/<EXTENDED_CREDIT_ID>/whatsapp_credit_sharing_and_attach
GET and DELETE
/<ALLOCATION_CONFIG_ID>
GET
/<EXTENDED_CREDIT_ID>/owning_credit_allocation_configs
For more information on how to get your current rate usage, see Headers.
In addition, the platform applies several message rate limits:
Test message rate limit (for unverified WABAs)
Quality rating and messaging limits (for verified WABAs)
Capacity rate limit (for all accounts)
Business phone rate limit (per phone number)
Pair rate limits
Business phone numbers can send 1 message every 6 seconds to the same WhatsApp user (0.17 messages/second), which equals about 10 messages per minute or 600 per hour. Exceeding this limit triggers error code 131056 until you are back within the allowed rate.
You may send up to 45 messages in a 6-second burst, but this “borrows” from your future quota. After a burst, you must wait the equivalent time it would take to send those messages at the normal rate (for example, a burst of 20 requires a ~2-minute wait before sending more to that user).
To manage post-burst throttling, if a send request fails, retry after 4^X seconds (starting with X=0 and increasing X by 1 after each failure) until successful.
Terms and policies
User opt-in
You must obtain user opt-in before sending message templates. Opt-in must clarify your business name and intent.
Learn more about the WhatsApp Business Messaging Policy.⁠
Terms and policies
All WhatsApp Business Platform use must comply with our terms and policies. Using unauthorized third-party tools is prohibited.
Learn more about terms and policies.⁠
Next steps
Get started with the WhatsApp Business Platform.
Learn more
Display names
Phone numbers
Pricing
Webhooks


Docs
Overview
WhatsApp Cloud API Get Started
WhatsApp Cloud API Get Started
Updated: Jun 16, 2026
This documentation is for developers building on the WhatsApp Business Platform. If you are a WhatsApp user experiencing issues with your personal account, visit the WhatsApp Help Center⁠ for support.
This guide helps developers quickly get started with the WhatsApp Cloud API. It covers the basic setup steps, including registering as a developer, creating a Meta app, sending your first message, and setting up a test webhook endpoint. You’ll also learn how to generate secure access tokens and send both template and non-template messages. Advanced features and further resources are introduced for deeper exploration.
Download the sample app
The Jasper’s Market sample app contains all of the messages and code used in the Jasper’s Market demo. You can use this sample app to learn how to build an application that sends and handles WhatsApp Cloud API data.
Prerequisites
Facebook account or managed Meta account
Developer registration
If not yet registered, visit the developer registration page and follow the prompts.
WhatsApp-enabled device for sending and receiving test messages
Step 1. Create a new Meta app with WhatsApp
Open the Meta App Dashboard to create a new Meta app with the WhatsApp use case.
Click Create App.
Add your app’s name and your email.
Select the Connect with customers through WhatsApp use case and click Next.
Select an existing business portfolio or create a new one.
A list of publishing requirements are listed. You may not have any at this point. Click Next.
Confirm your details, use case, and business portfolio. Click Previous to make changes or Create app to complete app creation.
After creating your app with the WhatsApp use case you are redirected to the Customize use case > Connect on WhatsApp > Quickstart page in the dashboard.
Step 2. Start using the API
Click the Start using the API button to set up the API by adding a phone number and sending your first message. You are redirected to the API Setup page.
In the API Setup section, connect your app to a WhatsApp Business account. This connection allows your app to access the WhatsApp Cloud API and send messages on behalf of your business. Select an existing WhatsApp Business account or create a new one:
To use an existing account: Select the WhatsApp Business account from the dropdown menu.
To create a new account: Click Create a WhatsApp Business account and follow the prompts to set up your business profile.
Once connected, you will see your WhatsApp Business account ID displayed in the API Setup panel.
Save this ID for use in API calls.
Note: If you created a new Meta Business Portfolio during app creation, a WhatsApp Business account may have been automatically created for you. Verify the connection in the API Setup section before proceeding.
Step 3. Send and receive messages
Click Generate access token to generate a temporary access token to send a test message.
Select a From phone number, or add a new one, from the dropdown menu.
Add a To phone number that will receive the test message.
Click the Send message button to send your first message.
Make sure to retain both your test phone number ID and WhatsApp Business account ID for later use.
Once you receive the message you sent, make sure to reply back to keep the conversation going.
The left-side menu lists ways in which you can customize use case settings and permissions to make your app work the way you want it to. You can update these settings at any time.
Permissions and features - View required and optional permissions for this use case and add them to an App Review submission, if applicable.
Quickstart - Start using the API and learn how to scale your business, improve ROI, and manage your WhatsApp Business account.
API Setup - Generate access tokens, send and receive messages, and configure webhooks and the WhatsApp SDK.
Configuration - Configure webhooks and the WhatsApp SDK.
Resources – View the WhatsApp developer documentation, Meta Blueprint courses, and support resources.
Tech Provider onboarding – Start scaling the WhatsApp Business Platform for your business.
Partner Solutions – Create a partner solution.
Embedded Signup Builder – Integrate Embedded Signup flow into your website or client portal.
Step 4. Set up the test webhook app
You will need to set up a webhook endpoint in order to receive notifications about message statuses, such as read and delivered.
Use the sample webhook server for testing purposes by following the Using a test webhook app guide.
Once your test webhook application is established, respond in the WhatsApp chat thread you created with yourself. You will see the webhook payload in your test application like this:
{
  "object": "whatsapp_business_account",
  "entry": [
    {
      "id": "215589313241560883",
      "changes": [
        {
          "value": {
            "messaging_product": "whatsapp",
            "metadata": {
              "display_phone_number": "15551797781",
              "phone_number_id": "7794189252778687"
            },
            "contacts": [
              {
                "profile": {
                  "name": "Jessica Laverdetman"
                },
                "wa_id": "13557825698"
              }
            ],
            "messages": [
              {
                "from": "17863559966",
                "id": "wamid.HBgLMTc4NjM1NTk5NjYVAGHAYWYET688aASGNTI1QzZFQjhEMDk2QQA=",
                "timestamp": "1758254144",
                "text": {
                  "body": "Hi!"
                },
                "type": "text"
              }
            ]
          },
          "field": "messages"
        }
      ]
    }
  ]
}
Step 5. Create a system user and generate a permanent access token
The temporary access token you created to send the hello_world template message expires quickly and is not suitable for development purposes. So you should create a permanent token for use across the WhatsApp Business Platform.
Navigate to Business Settings⁠ and click System users in the sidebar.
Click the Add+ button in the upper-right corner and follow the prompts to create a new system user.
Select the new system user you created, and click Assign Assets.
Select your app and toggle Manage app under Full control.
Select your WhatsApp account and toggle Manage WhatsApp Business accounts under Full control.
Click the Assign assets button.
Click Generate token.
Follow the prompts to generate your token.
Add the following permissions to the token:
business_management
whatsapp_business_messaging
whatsapp_business_management
Copy the token and store it in a secure place to be used in the later steps.
Step 6. Send a non-template message
When you responded to your earlier test message, you triggered what is known as a customer service window. This 24-hour window allows you to send non-template messages to users on WhatsApp. With the customer service window now open, you can send a non-template message to yourself. To do this, insert your test phone number ID, the system user access token, and your phone number in the code sample below, then paste the code into your terminal and run it.
curl 'https://graph.facebook.com/v23.0/<TEST_BUSINESS_PHONE_NUMBER_ID>/messages' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <SYSTEM_USER_ACCESS_TOKEN>' \
-d '
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<WHATSAPP_USER_PHONE_NUMBER>",
  "type": "text",
  "text": {
    "body": "Hello!"
  }
}'
After successfully sending your message, check your test webhook application to view the webhook event confirming the message receipt.
Step 7. Finish
The WhatsApp Cloud API enables you to send messages and receive webhooks—these are the fundamental building blocks for messaging integration. Beyond these basics, the API offers additional features such as group creation and management, as well as support for calling. To explore these advanced capabilities, check out the “Learn more” section below.
Learn more
Learn about the different types of non-template messages
Learn how to create and send template messages
Learn how to create and manage WhatsApp groups via API
Learn how to send and receive calls on WhatsApp via API
Learn how to add a business phone number
Learn how to set up your own webhook server
Onboard WhatsApp Business app users — allow businesses already using the WhatsApp Business app to connect their existing account and phone number to Cloud API via Embedded Signup
Become a partner
View WhatsApp API OpenAPI Specification⁠

Docs
Overview
Get started
Get started
Updated: May 21, 2026
Learn how to send a template message with the Marketing Messages API for WhatsApp (MM API for WhatsApp).
Requirements
You have an active WhatsApp Business Account and are in a country eligible for MM API for WhatsApp.
You have an approved marketing template message.
You are subscribed to the messages webhook.
Step 1: Accept Terms of Service
Navigate to the App Dashboard > WhatsApp > Quickstart panel.
Locate the “Improve ROI with marketing messages with optimizations” module and click the “Get started” button.
Click on “Continue to integration guide” and accept the Terms of Service.
Step 2: Send a marketing template message
Use the marketing_messages endpoint to send a template message to yourself.
curl 'https://graph.facebook.com/<API_VERSION>/<WHATSAPP_BUSINESS_PHONE_NUMBER_ID>/marketing_messages' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <ACCESS_TOKEN>' \
-d
'{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<WHATSAPP_USER_PHONE_NUMBER>",
  "type": "template",
  "template": {
      "name": "<TEMPLATE_NAME>",
      "language": {
          "code": "<LANGUAGE_AND_LOCALE_CODE>"
      },
      "components": [
          {
              "type": "body",
              "parameters": [
                  {
                      "type": "text",
                      "text": "text-string"
                  }
              ]
          }
      ]
  }
}'
Step 3: Verify message was sent through the status webhook
MM API for WhatsApp triggers status messages webhooks for events such as sent, delivered, and read. When a message is sent via MM API for WhatsApp, the webhook payload will have category and conversation.origin.type set to marketing_lite.
{
  "conversation": {
    "id": "<CONVERSATION_ID>",
    "origin": {
      "type": "marketing_lite"
    }
  },
  "pricing": {
    "billable": true,
    "pricing_model": "PMP",
    "category": "marketing_lite"
  }
}
Geographic availability of features
Some advanced features and reporting capabilities of MM API for WhatsApp are available only in particular geographies due to Meta policy and/or local regulation.
European Economic Area, United Kingdom, Japan, South Korea, Nigeria, South Africa
Messages sent from a business phone number in these countries, or to a WhatsApp user in these countries, will not receive delivery optimizations. Note that per-user marketing message template limits are also not active in these countries, so a lack of delivery optimizations will not have any effect on message delivery.
Messages sent from a business phone number in these countries, or to a WhatsApp user in these countries, will not have click and conversion reporting metrics available.
For businesses in these countries, metrics are not available on Ads Manager UI or Insights API. As with Cloud API, metrics will be available via Business Management API and WhatsApp Manager UI ‘conversation’ metrics.
United States
Starting April 1, 2025, marketing messages sent to WhatsApp users in the United States will not be delivered (error code 131049). Note that this policy is not specific to MM API for WhatsApp - it is in place across all Business Messaging APIs (including Cloud API, see per-user marketing message template limits).
Business phone numbers in the US can still use MM API for WhatsApp to message users outside of the United States.
Cuba, Iran, North Korea, Syria, Venezuela, and three sanctioned regions in the Ukraine (Crimea, Donetsk, Luhansk)
Businesses in these regions are not eligible to onboard, and messages cannot be sent to a WhatsApp user in these regions. This policy is not specific to MM API for WhatsApp - it is in place across all Business Messaging APIs (including Cloud API, see country restrictions).
Russia, Belarus
Starting June 20, 2025, businesses in Russia and Belarus will be able to use MM API for WhatsApp with the following feature exceptions:
Messages sent by a business with a Meta business profile in Russia or Belarus, or using a payment method with a Russia or Belarus address, will not receive delivery optimizations.
Messages sent from a business phone number in these countries, or to a WhatsApp user in these countries, will not have click and conversion reporting metrics available. For businesses in these countries, metrics are not available on Ads Manager UI or Insights API. As with Cloud API, metrics will continue to be available via Business Management API and WhatsApp Manager UI conversation metrics.
Messages sent to a WhatsApp user in these countries will not use optimization features such as max pricing.
All other features of MM API for WhatsApp continue to be available.
Learn more
Learn about additional marketing message formats
Onboard WhatsApp Business app users — businesses already using the WhatsApp Business app can also be onboarded to use MM API for WhatsApp
