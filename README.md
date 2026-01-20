# Lead Qualification + AI Email Generator (n8n)

This n8n workflow receives a form submission via a webhook, normalizes key fields, qualifies the lead based on rules, and returns a response email (AI-generated for qualified leads, fallback message for unqualified).

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
<img width="1864" height="884" alt="Screenshot 2026-01-19 135130" src="https://github.com/user-attachments/assets/190c06ac-27ee-4e68-b848-cbb9518e8aac" />


