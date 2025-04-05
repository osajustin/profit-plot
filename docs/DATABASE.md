# Database Layout & Instructions

## Overview
The project uses Supabase’s PostgreSQL database to store user-submitted ideas and manage user data through Supabase Auth. The main table for our app is the `ideas` table.

## Tables and Schema

### 1. Users
- **Managed by Supabase Auth:**  
  Supabase automatically creates and manages user records when users sign up or log in.
- **Key Fields:**
  - `id` (UUID): Unique identifier for each user.
  - `email`: User’s email address.
- **Access:**  
  User data is secured with Row Level Security (RLS) to ensure users can only access their own information.

### 2. Ideas
- **Table Name:** `ideas`
- **Purpose:** Stores each submitted business idea along with its evaluation data.
- **Schema:**
  - `id`: Unique identifier (primary key).
  - `user_id`: References the `id` from the Users (or Supabase Auth) table.
  - `idea_text`: Text content of the submitted idea.
  - `pay_certainty_score`: Numeric score (e.g., 0–100) resulting from the Pay Certainty Test.
  - `quadrant`: Text or enum indicating the Demand Matrix quadrant:
    - "High End"
    - "Golden Goose"
    - "Mass Market"
    - "Labor of Love"
  - `created_at`: Timestamp of when the idea was submitted (default to current time).

## Setting Up the Database
1. **Create the `ideas` Table:**
   - In the Supabase dashboard, navigate to the SQL Editor.
   - Run a SQL command similar to:
     ```sql
     create table ideas (
       id uuid primary key default uuid_generate_v4(),
       user_id uuid references auth.users(id) not null,
       idea_text text not null,
       pay_certainty_score numeric not null,
       quadrant text not null,
       created_at timestamp with time zone default timezone('utc', now())
     );
     ```
   - Ensure that the function `uuid_generate_v4()` is enabled (install the `uuid-ossp` extension if needed).

2. **Row Level Security (RLS):**
   - Enable RLS on the `ideas` table:
     ```sql
     alter table ideas enable row level security;
     ```
   - Create policies to ensure users can only access their own ideas. For example:
     ```sql
     create policy "Allow logged in users to insert their ideas"
     on ideas for insert using (auth.uid() = user_id);
     
     create policy "Allow logged in users to view their ideas"
     on ideas for select using (auth.uid() = user_id);
     ```
   - This ensures that each user can only insert or select rows where `user_id` matches their own Supabase Auth ID.

## Data Flow
- **Idea Submission:**  
  When a user submits an idea, the frontend sends the data to the edge function or directly to the `ideas` table. The row is inserted with the computed `pay_certainty_score` and assigned `quadrant`.
- **Idea Retrieval:**  
  The frontend fetches ideas using queries that filter on `user_id`, ensuring users only see their own ideas.
- **Security:**  
  RLS ensures that even if someone tries to access data directly, they can only retrieve rows that belong to them.

## Maintenance
- **Backup:**  
  Regularly export your database or use Supabase’s backup features.
- **Monitoring:**  
  Monitor your database usage and logs in the Supabase dashboard.
- **Schema Updates:**  
  If new features are added, update the schema carefully and migrate existing data as needed.
