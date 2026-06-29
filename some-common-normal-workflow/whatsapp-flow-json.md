74. WhatsApp Flow JSON

Progressive Example: Lead Generation/Consultation Booking Form
📋 TUTORIAL OVERVIEW
Target Audience: JSON beginners who want to learn WhatsApp Flow JSON Platform: Meta Flow Builder (JSON editor on left, preview on right) Version: Flow JSON 7.0 Approach: One example that evolves from beginner → intermediate → advanced

🔑 CRITICAL RULE TO REMEMBER
Display Components (Static Only!)
TextHeading, TextBody, TextSubheading, TextCaption → ONLY static hardcoded text

❌ Cannot use ${form.field}, ${data.field}, or any dynamic references

These are purely for static labels and instructions

Input Components (Dynamic Allowed!)
TextInput, TextArea, Dropdown, RadioButtonsGroup, CheckboxGroup, DatePicker, OptIn

✅ Can use init-value to pre-fill with dynamic data

✅ Can use data-source for dynamic dropdown options

✅ Can use in action payloads

Where Dynamic References Work:
Input component init-value → Pre-fill fields with ${data.field} or ${screen.SCREEN_ID.form.field}

Dropdown data-source → Dynamically populate options

Action payloads → Pass data between screens or to endpoint

Component visible property → Show/hide based on conditions

📚 CHAPTER BREAKDOWN
JSON Basics for Absolute Beginners
What to Cover:

What is JSON? (JavaScript Object Notation)

Key-value pairs: "name": "value"

Objects: { } - contain key-value pairs

Arrays: [ ] - contain lists of items

Nesting: Objects inside objects

Common mistakes: Missing commas, wrong brackets

Visual Demo:

{
  "name": "John",
  "age": 25,
  "hobbies": ["reading", "coding"],
  "address": {
    "city": "Mumbai",
    "country": "India"
  }
}

Key Tip: "Flow JSON is just JSON with specific keys that WhatsApp understands"

🟢 BEGINNER - Build Your First Screen 
What We're Building: Single-screen contact form with name, phone, email, and service dropdown.

Live Coding - Type & Explain Each Key:

{
  "version": "7.3",
  "screens": [
    {
      "id": "CONTACT_INFO",
      "title": "Get a Consultation",
      "terminal": true,
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Book Your Free Consultation"
          },
          {
            "type": "TextBody",
            "text": "Fill in your details and we'll get back to you within 24 hours."
          },
              {
                "type": "TextInput",
                "name": "customer_name",
                "label": "Your Name",
                "input-type": "text",
                "required": true,
                "helper-text": "Enter your full name"
              },
              {
                "type": "TextInput",
                "name": "phone",
                "label": "Phone Number",
                "input-type": "phone",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "email",
                "label": "Email Address",
                "input-type": "email",
                "required": true
              },
              {
                "type": "Dropdown",
                "name": "service",
                "label": "Service Interested",
                "required": true,
                "data-source": [
                  { "id": "web", "title": "Web Development" },
                  { "id": "app", "title": "Mobile App" },
                  { "id": "automation", "title": "AI Automation" },
                  { "id": "consulting", "title": "Tech Consulting" }
                ]
              },
            {
            "type": "Footer",
            "label": "Submit",
            "on-click-action": {
                "name": "complete",
                "payload": {
                    "name": "${form.customer_name}",
                    "phone": "${form.phone}",
                    "email": "${form.email}",
                    "service": "${form.service}"
                }
            }
        }
        ]
      }
    }
  ]
}

Key Concepts Explained :

version Flow JSON version (use "7.0" for latest features)

screens Array of all screens in your flow 

id Unique identifier for the screen (UPPERCASE convention) 

title Appears in the header bar 

terminal If true, this screen can end the flow 

layout Container for all components 

type: "SingleColumnLayout" Only layout type available 

children Array of components inside layout 

name Identifier used in payloads with ${form.name} 

on-click-action What happens when button is clicked 

complete Action that ends the flow and sends data 

payload Data sent when action is triggered

Quick Reference - All Component Types
Display Components (Static Text Only):

{ "type": "TextHeading", "text": "Main Title" }
{ "type": "TextSubheading", "text": "Subtitle" }
{ "type": "TextBody", "text": "Regular paragraph text" }
{ "type": "TextCaption", "text": "Small helper text" }
{ "type": "Image", "src": "https://example.com/image.jpg", "height": 200 }
{ "type": "EmbeddedLink", "text": "Click here", "on-click-action": { "name": "navigate", "next": { "name": "NEXT_SCREEN" } } }

Input Components (Inside Form):

{ "type": "TextInput", "name": "field_name", "label": "Label", "input-type": "text|email|phone|number|password|passcode" }
{ "type": "TextArea", "name": "message", "label": "Message", "max-length": 500 }
{ "type": "Dropdown", "name": "choice", "label": "Select", "data-source": [...] }
{ "type": "RadioButtonsGroup", "name": "option", "label": "Choose One", "data-source": [...] }
{ "type": "CheckboxGroup", "name": "options", "label": "Select Multiple", "data-source": [...] }
{ "type": "DatePicker", "name": "date", "label": "Select Date" }
{ "type": "OptIn", "name": "agree", "label": "I agree to terms" }

Action Component:

{ "type": "Footer", "label": "Button Text", "on-click-action": { ... } }

Pro Tip: "Display components = static labels. Input components = user interaction. Footer = the main action button."

🟡 INTERMEDIATE - Add Second Screen with Navigate 
What We're Adding: Second screen for project details (budget, timeline, message). Use navigate action to pass data forward.

Updated JSON - Screen 1 Changes:

{
  "version": "7.3",
  "screens": [
    {
      "id": "CONTACT_INFO",
      "title": "Get a Consultation",
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Book Your Free Consultation"
          },
          {
            "type": "TextBody",
            "text": "Fill in your details and we'll get back to you within 24 hours."
          },
              {
                "type": "TextInput",
                "name": "customer_name",
                "label": "Your Name",
                "input-type": "text",
                "required": true,
                "helper-text": "Enter your full name"
              },
              {
                "type": "TextInput",
                "name": "phone",
                "label": "Phone Number",
                "input-type": "phone",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "email",
                "label": "Email Address",
                "input-type": "email",
                "required": true
              },
              {
                "type": "Dropdown",
                "name": "service",
                "label": "Service Interested",
                "required": true,
                "data-source": [
                  { "id": "web", "title": "Web Development" },
                  { "id": "app", "title": "Mobile App" },
                  { "id": "automation", "title": "AI Automation" },
                  { "id": "consulting", "title": "Tech Consulting" }
                ]
              },
            {
            "type": "Footer",
            "label": "Submit",
            "on-click-action": {
                "name": "navigate",
                "next": {
                    "name": "PROJECT_DETAILS",
                    "type": "screen"
                },
                "payload": {
                    "customer_name": "${form.customer_name}",
                    "phone": "${form.phone}",
                    "email": "${form.email}",
                    "service": "${form.service}"
                }
            }
        }
        ]
      }
    },
    {
      "id": "PROJECT_DETAILS",
      "title": "Project Details",
      "terminal": true,
      "data": {
        "customer_name": { "type": "string", "__example__": "John Doe" },
        "phone": { "type": "string", "__example__": "+919876543210" },
        "email": { "type": "string", "__example__": "john@example.com" },
        "service": { "type": "string", "__example__": "web" }
      },
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Tell Us About Your Project"
          },
          {
            "type": "TextBody",
            "text": "Step 2 of 2: Project Requirements"
          },
              {
                "type": "Dropdown",
                "name": "budget",
                "label": "Budget Range",
                "required": true,
                "data-source": [
                  { "id": "10k", "title": "₹10,000 - ₹50,000" },
                  { "id": "50k", "title": "₹50,000 - ₹1,00,000" },
                  { "id": "1L", "title": "₹1,00,000 - ₹5,00,000" },
                  { "id": "5L+", "title": "₹5,00,000+" }
                ]
              },
              {
                "type": "Dropdown",
                "name": "timeline",
                "label": "Expected Timeline",
                "required": true,
                "data-source": [
                  { "id": "urgent", "title": "ASAP (Within 2 weeks)" },
                  { "id": "1month", "title": "1 Month" },
                  { "id": "3months", "title": "1-3 Months" },
                  { "id": "flexible", "title": "Flexible" }
                ]
              },
              {
                "type": "TextArea",
                "name": "message",
                "label": "Describe Project",
                "required": false,
                "helper-text": "Tell us what you want to build"
              },
              {
                "type": "Footer",
                "label": "Submit Request",
                "on-click-action": {
                  "name": "complete",
                  "payload": {
                    "name": "${data.customer_name}",
                    "phone": "${data.phone}",
                    "email": "${data.email}",
                    "service": "${data.service}",
                    "budget": "${form.budget}",
                    "timeline": "${form.timeline}",
                    "message": "${form.message}"
                  }
                }
              }
            ]
      }
    }
  ]
}
Key Concepts Explained:

Concept Explanation 

navigate action - Moves to another screen instead of completing 

next.name The screen ID to navigate to 

payload in navigate Data passed to the next screen 

data property on screen Declares what data this screen expects to receive 

__example__ Sample value for preview (required for preview to work) 

${data.field} Access data passed from previous screen 

${form.field} Access current screen's form inputs Combining data sources Final payload uses both ${data.*} and ${form.*}

Important: Remove terminal: true from first screen when adding navigation!

All 5 Actions Explained 
1. complete - End the Flow

{
  "name": "complete",
  "payload": {
    "collected_data": "${form.field}"
  }
}

Terminates the flow

Sends payload to your webhook

Must be on a terminal: true screen

2. navigate - Go to Another Screen

{
  "name": "navigate",
  "next": {
    "name": "SCREEN_ID",
    "type": "screen"
  },
  "payload": {
    "data_to_pass": "${form.field}"
  }
}

Moves user to specified screen

Payload becomes available as ${data.*} on next screen

3. data_exchange - Call Your Backend (Dynamic Flows)

{
  "name": "data_exchange",
  "payload": {
    "action": "validate_user",
    "email": "${form.email}"
  }
}

Sends data to your endpoint (defined in data_channel_uri)

Your server responds with data/navigation instructions

Used for: validation, fetching data, conditional logic

Requires backend setup (covered in separate video)

4. open_url - Open External Link

{
  "name": "open_url",
  "url": "https://example.com/terms"
}

Opens URL in device's default browser

Great for: Terms & Conditions, Privacy Policy links

Can be used with OptIn component

5. update_data - Update Screen State Dynamically

{
  "name": "update_data",
  "payload": {
    "show_extra_field": true
  }
}

Updates screen's data without leaving the screen

Used with visible property to show/hide components

Great for: conditional fields, dynamic forms

🔴 ADVANCED - Review Screen with Global Properties 
What We're Adding:

Third "Review" screen where user can see and edit their entries

OptIn with open_url action for Terms & Conditions

update_data action to show/hide a field based on selection

Complete Final JSON:

{
  "version": "7.3",
  "screens": [
    {
      "id": "CONTACT_INFO",
      "title": "Get a Consultation",
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Book Your Free Consultation"
          },
          {
            "type": "TextBody",
            "text": "Step 1 of 3: Your Contact Information"
          },
          {
            "type": "Form",
            "name": "contact_form",
            "children": [
              {
                "type": "TextInput",
                "name": "customer_name",
                "label": "Your Name",
                "input-type": "text",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "phone",
                "label": "Phone Number",
                "input-type": "phone",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "email",
                "label": "Email Address",
                "input-type": "email",
                "required": true
              },
              {
                "type": "Dropdown",
                "name": "service",
                "label": "Service Interested",
                "required": true,
                "data-source": [
                  { "id": "web", "title": "Web Development" },
                  { "id": "app", "title": "Mobile App" },
                  { "id": "automation", "title": "AI Automation" },
                  { "id": "consulting", "title": "Tech Consulting" }
                ]
              },
              {
                "type": "Footer",
                "label": "Next →",
                "on-click-action": {
                  "name": "navigate",
                  "next": {
                    "name": "PROJECT_DETAILS",
                    "type": "screen"
                  },
                  "payload": {
                    "customer_name": "${form.customer_name}",
                    "phone": "${form.phone}",
                    "email": "${form.email}",
                    "service": "${form.service}",
                    "show_referral": false
                  }
                }
              }
            ]
          }
        ]
      }
    },
    {
      "id": "PROJECT_DETAILS",
      "title": "Project Details",
      "data": {
        "customer_name": { "type": "string", "__example__": "John Doe" },
        "phone": { "type": "string", "__example__": "+919876543210" },
        "email": { "type": "string", "__example__": "john@example.com" },
        "service": { "type": "string", "__example__": "web" },
        "show_referral": { "type": "boolean", "__example__": false }
      },
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Tell Us About Your Project"
          },
          {
            "type": "TextBody",
            "text": "Step 2 of 3: Project Requirements"
          },
          {
            "type": "Form",
            "name": "project_form",
            "children": [
              {
                "type": "Dropdown",
                "name": "budget",
                "label": "Budget Range",
                "required": true,
                "data-source": [
                  { "id": "10k", "title": "₹10,000 - ₹50,000" },
                  { "id": "50k", "title": "₹50,000 - ₹1,00,000" },
                  { "id": "1L", "title": "₹1,00,000 - ₹5,00,000" },
                  { "id": "5L+", "title": "₹5,00,000+" }
                ]
              },
              {
                "type": "Dropdown",
                "name": "timeline",
                "label": "Expected Timeline",
                "required": true,
                "data-source": [
                  { "id": "urgent", "title": "ASAP (Within 2 weeks)" },
                  { "id": "1month", "title": "1 Month" },
                  { "id": "3months", "title": "1-3 Months" },
                  { "id": "flexible", "title": "Flexible" }
                ]
              },
              {
                "type": "TextArea",
                "name": "message",
                "label": "Describe Project",
                "required": false,
                "helper-text": "Tell us what you want to build"
              },
              {
                "type": "OptIn",
                "name": "has_referral",
                "label": "I was referred by someone",
                "required": false,
                "on-select-action": {
                  "name": "update_data",
                  "payload": {
                    "show_referral": true
                  }
                },
                "on-unselect-action": {
                  "name": "update_data",
                  "payload": {
                    "show_referral": false
                  }
                }
              },
              {
                "type": "TextInput",
                "name": "referral_code",
                "label": "Referral Code",
                "input-type": "text",
                "required": false,
                "visible": "${data.show_referral}"
              },
              {
                "type": "Footer",
                "label": "Review Details →",
                "on-click-action": {
                  "name": "navigate",
                  "next": {
                    "name": "REVIEW",
                    "type": "screen"
                  },
                  "payload": {
                    "budget": "${form.budget}",
                    "timeline": "${form.timeline}",
                    "message": "${form.message}",
                    "has_referral": "${form.has_referral}",
                    "referral_code": "${form.referral_code}"
                  }
                }
              }
            ]
          }
        ]
      }
    },
    {
      "id": "REVIEW",
      "title": "Review & Submit",
      "terminal": true,
      "data": {
        "budget": { "type": "string", "__example__": "50k" },
        "timeline": { "type": "string", "__example__": "1month" },
        "message": { "type": "string", "__example__": "I need a website" },
        "has_referral": { "type": "boolean", "__example__": false },
        "referral_code": { "type": "string", "__example__": "" }
      },
      "layout": {
        "type": "SingleColumnLayout",
        "children": [
          {
            "type": "TextHeading",
            "text": "Review Your Request"
          },
          {
            "type": "TextBody",
            "text": "Step 3 of 3: Please verify your information"
          },
          {
            "type": "Form",
            "name": "review_form",
            "init-values": {
                "review_name": "${screen.CONTACT_INFO.form.customer_name}",
                "review_phone": "${screen.CONTACT_INFO.form.phone}",
                "review_email": "${screen.CONTACT_INFO.form.email}",
                "review_message": "${data.message}"

            },
            "children": [
              {
                "type": "TextInput",
                "name": "review_name",
                "label": "Name",
                "input-type": "text",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "review_phone",
                "label": "Phone",
                "input-type": "phone",
                "required": true
              },
              {
                "type": "TextInput",
                "name": "review_email",
                "label": "Email",
                "input-type": "email",
                "required": true
              },
              {
                "type": "TextArea",
                "name": "review_message",
                "label": "Project Description",
                "required": false
              },
              {
                "type": "OptIn",
                "name": "agree_terms",
                "label": "I agree to the Terms & Conditions",
                "required": true,
                "on-click-action": {
                  "name": "open_url",
                  "url": "https://example.com/terms"
                }
              },
              {
                "type": "Footer",
                "label": "Submit Request ✓",
                "on-click-action": {
                  "name": "complete",
                  "payload": {
                    "name": "${form.review_name}",
                    "phone": "${form.review_phone}",
                    "email": "${form.review_email}",
                    "service": "${screen.CONTACT_INFO.form.service}",
                    "budget": "${screen.PROJECT_DETAILS.form.budget}",
                    "timeline": "${screen.PROJECT_DETAILS.form.timeline}",
                    "message": "${form.review_message}",
                    "referral_code": "${data.referral_code}",
                    "agreed_terms": "${form.agree_terms}"
                  }
                }
              }
            ]
          }
        ]
      }
    }
  ]
}
Key Advanced Concepts:

init-value - Pre-fills input with data: "init-value": "${data.field}" 

Global Properties - Access ANY screen's data: ${screen.SCREEN_ID.form.field} 

update_data - Dynamically update screen state without navigation 

visible property - Show/hide component: "visible": "${data.show_field}" 

open_url - Opens browser to external URL 

on-select-action - Triggered when OptIn/Dropdown item is selected 

on-unselect-action - Triggered when OptIn is unchecked

Explain These Patterns:

Conditional Field (Show/Hide):

User checks "I was referred" OptIn

on-select-action triggers update_data → sets show_referral: true

TextInput with visible: "${data.show_referral}" appears

User unchecks → on-unselect-action hides it

Review Screen with Editable Pre-filled Fields:

Use init-value with global property: ${screen.CONTACT_INFO.form.customer_name}

Fields are editable (not disabled) so user can modify

Final complete action uses the current form values

Terms & Conditions Pattern:

OptIn with open_url action

Clicking the label opens the terms in browser

User must check to enable submit

Global Properties Deep Dive
Two Ways to Reference Data:

1. Local/Scoped Reference (within same screen or from navigate payload):

"${form.field_name}"      // Current screen's form input
"${data.field_name}"      // Data received via navigate payload

2. Global Reference (access ANY screen's form data):

"${screen.SCREEN_ID.form.field_name}"   // Any screen's form input
"${screen.SCREEN_ID.data.field_name}"   // Any screen's data property

When to Use Which:

Use ${form.*} for current screen inputs

Use ${data.*} for data passed via navigate

Use ${screen.*.form.*} when you need data from a screen that wasn't directly before (like in review screen)

Example - Review Screen accessing Screen 1's data:

// On REVIEW screen (3rd screen), accessing CONTACT_INFO (1st screen):
"init-value": "${screen.CONTACT_INFO.form.customer_name}"

// In final complete payload:
"payload": {
  "name": "${form.review_name}",                    // Current screen
  "service": "${screen.CONTACT_INFO.form.service}", // Screen 1
  "budget": "${screen.PROJECT_DETAILS.form.budget}" // Screen 2
}

Expression Concatenation & Advanced Payloads
String Concatenation in Payloads:

"payload": {
  "full_info": "${form.name} - ${form.phone}",
  "summary": "Service: ${screen.CONTACT_INFO.form.service}, Budget: ${form.budget}"
}

Nested Expressions (useful for combining data):

"payload": {
  "contact": {
    "name": "${form.name}",
    "phone": "${form.phone}",
    "email": "${form.email}"
  },
  "project": {
    "service": "${form.service}",
    "budget": "${form.budget}"
  },
  "metadata": {
    "submitted_via": "whatsapp_flow",
    "version": "1.0"
  }
}

Quick Reference & Next Steps (28:30 - 30:00)
Cheat Sheet Recap:

Action                         Purpose                      Key Properties 

complete -           End flow, send data -    payload 

navigate -           Go to screen -                next.name, payload 

data_exchange - Call backend -               payload (needs endpoint) 

open_url -           Open browser -             url 

update_data -     Update screen state - payload (updates data)

Component Quick Reference:

Type                             Purpose                                    Dynamic? 

TextHeading               Big title                                      ❌ Static only 

TextBody                     Paragraph                                 ❌ Static only 

TextInput                     Single line input                       ✅ init-value 

TextArea                      Multi-line input                         ✅ init-value 

Dropdown                    Select one                                 ✅ data-source, init-value 

RadioButtonsGroup   Select one (visible)                  ✅ data-source 

CheckboxGroup          Select multiple                         ✅ data-source 

OptIn                             Checkbox with label                ✅ with actions 

DatePicker                   Date selection                           ✅ init-value 

Footer                           Action button                             N/A

Resources:

Meta Flow JSON Docs: https://developers.facebook.com/docs/whatsapp/flows/reference/flowjson

Components Reference: https://developers.facebook.com/docs/whatsapp/flows/reference/components
