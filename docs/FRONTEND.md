# Frontend Layout & Instructions

## Overview
This project’s frontend is built using the Next.js SaaS Starter template. It uses React for the user interface, TailwindCSS for styling, and ShadCN UI for pre-built, accessible components. The application is a **Business Idea Generator** that helps users submit and evaluate business ideas ("nuggets") using a Pay Certainty Test and visualize the results on an interactive Demand Matrix.

The frontend handles:
- **User Authentication:** Allowing users to sign up or log in via Supabase Auth.
- **Idea Submission:** Enabling users to submit their business ideas.
- **Idea Evaluation:** Displaying results from the evaluation (Pay Certainty Score and assigned quadrant).
- **Visualization:** Arranging and displaying the evaluated ideas on a 2x2 grid (Demand Matrix).

## Technologies Used
- **ReactJS & NextJS:** For building the user interface and routing.
- **TailwindCSS:** For styling components with utility classes.
- **ShadCN UI:** For pre-styled, accessible UI components.
- **Supabase Client:** For connecting to the backend (authentication, data fetching, and edge function calls).

## Layout Structure
- **Pages:**
  - **Login/Signup Page:** Where users create an account or log in. This page uses Supabase Auth.
  - **Dashboard/Main Page:** Displays the Idea Submission Form and the Demand Matrix.
  - **Ideas List Page (Optional):** Lists all submitted ideas along with their evaluation results.
- **Components:**
  - **IdeaForm:** A form for users to input and submit their business ideas (nuggets). This is the primary tool for generating new business ideas.
  - **DemandMatrix:** A 2x2 grid component that visually categorizes ideas into four quadrants:
    - **High End:** High price, few customers.
    - **Golden Goose:** High price, many customers.
    - **Mass Market:** Low price, many customers.
    - **Labor of Love:** Low price, few customers.
  - **IdeaCard:** A card to display each idea’s text, score, and quadrant, providing quick feedback on the idea’s potential.

## Setup Instructions
1. **Clone the Repository:**  
   ```bash
   git clone <your-repo-url>
   cd <your-repo-directory>

	2.	Install Dependencies:

npm install


	3.	Environment Variables:
Create a .env.local file in the project root and add:

NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key


	4.	Run the Development Server:

npm run dev

Visit http://localhost:3000 to see the app running.

How It Works
	•	Authentication:
Users are prompted to log in or sign up using Supabase Auth. After successful authentication, they can access the main dashboard.
	•	Idea Submission:
The IdeaForm component allows users to enter their business idea (nugget) into a text area and submit it. When submitted, the form calls a Supabase Edge Function to evaluate the idea using the Pay Certainty Test logic. The result includes a numerical score and the corresponding quadrant (e.g., “Golden Goose”, “Mass Market”).
	•	Demand Matrix Visualization:
The DemandMatrix component arranges submitted ideas into four quadrants. Each quadrant is clearly labeled and displays the business ideas that fall within that category. This helps users quickly understand which ideas are most promising and which might need further refinement.

Deployment
	•	Local Development: Use npm run dev to work locally.
	•	Production Build:

npm run build

Deploy the generated files to your hosting platform (like Vercel or Netlify). Ensure your environment variables are set up on the platform as well.

