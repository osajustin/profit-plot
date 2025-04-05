# Backend Layout & Instructions

## Overview
The backend is powered by **Supabase**, which provides:
- A PostgreSQL database for storing user data and ideas.
- Authentication services via Supabase Auth.
- Edge Functions for custom backend logic, such as the Pay Certainty Test evaluation.

## Technologies Used
- **Supabase:** Managed backend service (database, auth, edge functions).
- **Edge Functions:** Small server-side functions written in TypeScript/JavaScript that execute custom logic.

## Key Backend Components
1. **Authentication:**
   - Supabase Auth handles user sign-up, login, and session management.
   - Users are automatically added to the auth system when they sign up.
   
2. **Edge Function – Pay Certainty Evaluator:**
   - Receives a business idea ("nugget") from the frontend.
   - Runs the Pay Certainty Test logic to evaluate:
     - **Ability to pay:** Do customers have the money?
     - **Willingness to pay:** Do customers want to pay?
   - Computes a `pay_certainty_score` (e.g., 0–100) and assigns a quadrant:
     - **Golden Goose:** High ability & willingness.
     - **High End:** Ability is high but willingness is low.
     - **Mass Market:** Willingness is high but ability is low.
     - **Labor of Love:** Both ability and willingness are low.
   - Optionally inserts the evaluated idea into the database.
   - Returns the computed data (score and quadrant) to the frontend.

## Setup Instructions
1. **Create a Supabase Project:**
   - Go to [Supabase](https://supabase.com/) and create a new project.
   - Obtain your project’s URL and anon key from the dashboard.

2. **Set Up Environment Variables in Your Frontend:**
   - Add `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in your `.env.local` file.

3. **Write & Deploy Edge Functions:**
   - Write your edge function (e.g., `evaluateIdea`) to process the idea evaluation logic.
   - Use the Supabase CLI or dashboard to deploy the function.
   - Make sure the function uses the authenticated user’s token for security.

## Integration with the Frontend
- **Calling the Function:**  
  The frontend uses the Supabase client to invoke the edge function:
  ```js
  const { data, error } = await supabase.functions.invoke("evaluateIdea", {
    body: { ideaText: "Your business idea here" },
  });

	•	Security:
Edge functions are secured by ensuring only authenticated users can call them. Supabase passes the user’s JWT along with requests.

Maintenance
	•	Logs:
Monitor edge function logs from the Supabase dashboard to troubleshoot any issues.
	•	Updates:
Update the function logic as needed based on feedback or new requirements.
	•	Scaling:
Supabase manages backend scaling automatically, but keep an eye on usage quotas if the app grows.
