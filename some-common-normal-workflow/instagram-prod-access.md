68. Instagram Prod

📘 Instagram Credentials Setup for n8n (2025)
✅ Step 1: Create a Meta App
Go to https://developers.facebook.com/apps

Click Create App

Enter:

App Name

Contact Email

Select Other as the use case

Choose Business as the App Type

Skip Business Portfolio (optional)

Click Create App and enter Facebook password if prompted

⚠️ Important: Choosing “Business” as App Type is required to access Instagram Graph API

✅ Step 2: Configure App Permissions
From the Dashboard, click Set Up under Instagram

If it is showing “The content is no longer available”, don’t worry try below things

Refresh page and check again

Click on App Roles > Roles, then come back to Dashboard, click Set Up under Instagram

If nothing work try after sometime

Go to sidebar: App Roles > Roles

Click Add People

Choose IG Tester

Search for the Instagram username you want to link

Click Add

⚠️ Make sure the IG account is a Professional (Business/Creator) account

✅ This sends a tester invite to the IG user

✅ Step 3: Accept the IG Tester Invite
Go to https://instagram.com and log in

Visit: https://www.instagram.com/accounts/manage_access

Accept the IG Tester invite shown there

⚠️ You cannot proceed until this invite is accepted!

✅ Step 4: Link IG Account to the App
Return to the app dashboard

Navigate to Instagram > API Setup with Instagram Login

Expand "Generate Access Token" section

Look for your IG account

✅ Step 5: Make Your Instagram App Live (Required for Messaging & Production Use)
Go to App Dashboard

On the left sidebar, click App Setting > Basic

Enter the:

Privacy Policy URL

Terms of Service URL

User data deletion URL

Then click on Save changes

Now on top toggle From Development → Live

⚠️ Once the app is Live, your automation can send & receive messages normally.

✅ Step 6: Configure the Webhook (For Messages, Comments, Events)
Go to: App Dashboard > Instagram

Click API Setup with Instagram Login

Go to Configure Webhook:

Enter your Webhook URL from your server or n8n
Example:

https://yourdomain.com/webhook/instagram
Enter a Verify Token (any string you choose)

Save the configuration

On your server/n8n, make sure you handle the GET verification challenge

Subscribe to these Webhook fields:

messages

comments

mentions

story_insights

live_comments (optional)

⚠️ The webhook will not work until:
✔ The IG account is a Professional account
✔ App is in Live Mode
✔ Permissions are approved

✅  Step 7: Keep Your Long-Lived Token Alive Forever (Auto-Refresh Token)
Once you have your 60-day Long-Lived Token, you must refresh it regularly so it never expires.

Use the following API endpoint:

https://graph.instagram.com/refresh_access_token
  ?grant_type=ig_refresh_token
  &access_token=YOUR_LONG_LIVED_TOKEN
💡 This returns a new Long-Lived Token with a fresh 60-day expiry.

You can automate this in n8n using the HTTP Request node:

Method: GET

URL:

https://graph.instagram.com/refresh_access_token
Query Parameters:

grant_type = ig_refresh_token

access_token = YOUR_LONG_LIVED_TOKEN

✔ Schedule this workflow to run every 30 days
✔ Each run will refresh your token
✔ Your token will never expire as long as this automation runs

⚠️ Important Notes:

The refreshed token you receive must replace the old token everywhere you use it

Never expose your token publicly

Keep your automation running even if your server restarts (enable “Always On” or Cron Jobs)

Resources
Business Login for Instagram - https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login/business-login
