# Lead Qualification + AI Email Generator (n8n)

This n8n workflow receives a form submission via a webhook, normalizes key fields, qualifies the lead based on rules, and returns a response email (AI-generated for qualified leads, fallback message for unqualified).

## Workflow Screenshot
<img width="1864" height="884" alt="Screenshot 2026-01-19 135130" src="https://github.com/user-attachments/assets/3539e51c-afcc-47b1-aba3-ccf7ecbadcce" />

## Flow Overview

1. **Webhook Trigger (POST)**
   - Real-time trigger: runs immediately when a form is submitted.

2. **Code Node (Normalize Data)**
   - Converts `monthlyRevenue` from a formatted string into a numeric value.
   - Cleans `BiggestChallenge` (trim) and computes its length.

3. **IF Node (Qualification Logic)**
   - Qualified if:
     - `MonthlyRevenueNumber >= 10000`
     - `ChallengeLength > 50`

4. **AI Node (Qualified Path)**
   - Uses Gemini (or your model) to generate a personalized email.

5. **Set Node (Qualified Path)**
   - Returns: name, email, status=qualified, responseEmail=AI output

6. **Set Node (Unqualified Path)**
   - Returns: name, email, status=not_qualified, responseEmail=fallback text

7. **Respond to Webhook**
   - Sends the final JSON response to the form submitter.

## Input Payload (Example)

Expected fields (adjust names to your form fields):
- `name` (string)
- `email` (string)
- `monthlyRevenue` (string, e.g. "$12,500" or "12,500 USD")
- `BiggestChallenge` (string)

## Output (Example)

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "qualificationStatus": "qualified",
  "responseEmail": "Hi John, ... (AI-generated email) ..."
}
```

## Testing with Postman
1. Method: POST  
2. URL: <your n8n webhook URL>  
3. Header: Content-Type: application/json  
4. Body: raw JSON (see sample_payloads/)

Expected response includes:
- qualificationStatus: qualified | not_qualified
- responseEmail: generated (qualified) or fallback (not qualified)

## Demo Video
Loom walkthrough: LOOM_https://www.loom.com/share/e5790334927742eaacdd285453cf6b49LINK>

