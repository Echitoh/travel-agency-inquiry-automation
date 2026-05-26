# Travel Agency Inquiry Automation

> AI-powered inquiry triage and response system for travel agencies — reads incoming customer emails, understands the request, drafts a tailored response, and tracks the conversation. Built with n8n, Gemini AI, Gmail, and Google Sheets.

## The Problem

Travel agencies receive inquiries that are wildly different from each other: "What's the cheapest way to get to Cancún in March?", "I need a quote for a family of 5 to Europe in summer", "Can you help me with a corporate trip for 12 people?". Each requires a different response, different documentation, and a different sales path.

Handling this manually means:

- Slow first response — and customers shop multiple agencies, so the first to reply often wins
- Inconsistent quality depending on which agent picks up the message
- Lost inquiries that fall through the cracks during busy seasons
- No data on what people are actually asking about

This workflow uses an AI model to read each incoming inquiry, classify it, extract key data (destination, dates, group size, budget), draft a starter response, and log everything to a tracking sheet.

## How It Works

1. **Capture** — New emails arriving at the agency's inbox trigger the workflow.
2. **AI parsing** — Gemini reads the message and extracts structured data: type of trip, destination, dates, number of travelers, budget hints, and special requests.
3. **Classify** — The inquiry is tagged as `Quote Request`, `General Info`, `Booking Confirmation`, `Complaint`, or `Other` to route it correctly.
4. **Draft response** — Based on the classification, the AI drafts a personalized first response with relevant info pulled from a knowledge base (popular packages, FAQs, pricing tiers).
5. **Log to sheet** — The inquiry, extracted data, classification, and draft response go into a Google Sheet for the human agent to review.
6. **Human in the loop** — An agent reviews the draft in the sheet, edits if needed, and approves sending — or the workflow auto-sends low-risk responses (FAQs, info requests).

## Stack

- **n8n** — workflow orchestration
- **Gemini AI** — natural language understanding, data extraction, response drafting
- **Gmail** — email trigger and send
- **Google Sheets** — inquiry log and agent review board

## Screenshots

See [`/screenshots`](./screenshots/) — workflow diagram, sample AI extraction output, and sheet view.

## How to Replicate

1. Import [`workflow.json`](./workflow.json) into your n8n instance.
2. Set up a Google Sheet called `Inquiries` with columns:
   `Date`, `From`, `Subject`, `Original Message`, `Type`, `Destination`, `Dates`, `Travelers`, `Budget`, `Draft Response`, `Status`, `Agent Notes`.
3. Configure n8n credentials:
   - **Gmail OAuth2** — must include the inbox where inquiries arrive
   - **Google Sheets OAuth2** — pointing to the `Inquiries` sheet
   - **Google Gemini (PaLM)** — API key from Google AI Studio
4. Update the system prompt in the `Gemini` node with your agency's tone, popular destinations, and pricing tiers.
5. Adjust the classification labels in the `Switch` node if you want different categories.
6. Decide which classifications auto-send vs require human approval (default: only FAQs auto-send).
7. Activate the workflow.

## Customization Ideas

- Plug in your booking system's API so the AI can pull real availability and pricing.
- Add WhatsApp as an alternative channel — same logic, different trigger.
- Build a small dashboard with the sheet data to see most-requested destinations, seasonal trends, and conversion rates.
- Train a fine-tuned response style by feeding the AI a sample of your top agent's past replies.

---

Part of the [AI Automation portfolio](https://github.com/Echitoh) by Ezequiel Andrade.
