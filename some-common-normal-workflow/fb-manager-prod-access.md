69. FB Messenger Prod Access

📘 Messenger AI Agent Setup for n8n (2025)
Follow these exact steps to create a Messenger AI agent using n8n and the Meta Developer platform. This guide covers app creation, webhook verification, receiving messages, replying (static & AI), and common errors you’ll face.

✅ Step 1: Create a Meta App & enable Messenger
Go to: https://developers.facebook.com/apps

Click Create App

Fill these fields:

App Name

Contact Email

Select Other as the use case

Choose Business as the App Type

(Optional) Skip Business Portfolio

Click Create App and enter your Facebook password if prompted

⚠️ Important: Choose Business for messenger product access.

✅ Step 2: Add the Messenger product
From the App Dashboard click Set Up under Messenger.

You’ll see the Messenger product console with Webhooks, Access Tokens, and Subscriptions.

We’ll configure Webhooks first (next step).

✅ Step 3: Configure the Webhook (GET verification)
In the App dashboard → Messenger → Webhooks → Add Callback URL

Provide:

Callback URL = your n8n webhook URL (e.g. https://yourdomain.com/webhook/messenger)

Verify Token = any secret string (e.g. hello)

Do not click Verify & Save yet — we’ll prepare n8n to respond to the GET challenge first.

⚠️ When Facebook verifies a webhook it sends a GET request with hub.mode, hub.verify_token, and hub.challenge. You MUST respond back with the hub.challenge only when hub.mode === 'subscribe' and the token matches your Verify Token.

✅ Step 4: Build the verification flow in n8n
Create a Webhook node in n8n with the same path you entered as Callback URL.

Allow GET and POST methods on the webhook node (Settings → HTTP Methods).

Add a node to check GET verification:

Use a small function/filter to check hub.mode === 'subscribe' and hub.verify_token === 'hello'.

If matched, use Respond to Webhook node to return the hub.challenge value (plain text).

Execute the n8n workflow, then click Verify and Save in the Developer Console.

✔ Once the challenge is correctly echoed, the webhook will show Configured.

✅ Step 5: Allow POST (message) payloads and test incoming messages
Keep GET verification logic but ensure webhook also accepts POST.

Send a test message from a Facebook Page / Messenger user to your Page to see the POST payload arrive in n8n.

The incoming POST contains sender.id and recipient.id — note these IDs (PSID for the user).

Tip: Use the n8n Execute Node to watch incoming payloads during testing.

✅ Step 6: Connect Page & generate Page Access Token
In Developer Console → Messenger → Access Tokens → Select a Page to connect your app.

If multiple pages exist you can choose the exact page or select “current & future pages”.

Click Generate Access Token and copy the token (one-time shown).

Save this token securely (environment variable / vault).

⚠️ This token is required for sending messages via the Messenger Send API.

✅ Step 7: Subscribe to Messenger events
In Developer Console → Messenger → Subscriptions → enable messages (and other events you need like message_deliveries, messaging_postbacks, messaging_optins).

Confirm subscription — now you will receive POST events for messages.

✅ Step 8: Replying to messages (HTTP Request node)
Add an HTTP Request node after your webhook or AI node to send replies.

Use the Messenger Send API endpoint:

https://graph.facebook.com/v24.0/PSID/messages
Replace PSID with the Page-scoped recipient ID (the recipient value from the incoming payload — usually the page ID; the sender.id becomes the recipient when replying).

Query / body parameters:

access_token = YOUR_PAGE_ACCESS_TOKEN

Body JSON example:

{
  "recipient": { "id": "<SENDER_PSID>" },
  "message": { "text": "hello world" }
}
Execute; check Facebook Messenger — you should see the reply.

🔁 Important: when building the request in n8n use expressions to map {{$json["entry"][0]["messaging"][0]["sender"]["id"]}} into recipient.id.

✅ Step 9: Make Your Messenger App Live (Required for Messaging & Production Use)
Go to App Dashboard

On the left sidebar, click App Setting > Basic

Enter the:

Privacy Policy URL

Terms of Service URL

User data deletion URL

Then click on Save changes

Now on top toggle From Development → Live

⚠️ Once the app is Live, your automation can send & receive messages normally.
