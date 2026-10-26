Phase 1: Choose and fork
Pick one pair: CleanTrack, JobConnect, FarmConnect or LearnHub. Choose the domain you understand best, since you must explain the code later.
On GitHub, fork both the frontend and backend repos of that pair into your account.
Clone your forks:
bash
   git clone https://github.com/<your-username>/<app>-frontend.git
   git clone https://github.com/<your-username>/<app>-backend.git
Record the starting commit of each repo (you need this for the report):
bash
   git log -1 --format="%H %s"
Keep the original attribution (don’t delete the license or README credits).
Phase 2: Set up and verify the original app (20 marks)
Prerequisites: Check package.json (engines field) and the README for Node/npm versions. Install MySQL or MariaDB.
Install dependencies in each repo:
bash
   npm install
Environment files: Copy each .env.example to .env. Set:
DB host, user, password, and database name (use a new, separate learning database)
The API URL in the frontend
The allowed frontend origin (CORS) in the backend
Database: Read the SQL scripts first, because some drop and recreate the database. Then load the schema and seed data:
bash
   mysql -u root -p < path/to/schema.sql
Run both apps in separate terminals (usually npm run dev or npm start; check the README).
Verify: Open the frontend, confirm it loads data from your local API, log in, and complete one full workflow that changes the database. Confirm the change with a SQL query.
Document problems as you hit them (error message, cause, fix). This goes straight into your report.
Make sure .env is in .gitignore, and that .env.example has only placeholder values.
Phase 3: Understand the code

Before designing features, trace one action end to end, for example “user logs in” or “user creates a record”:

Frontend: which component and which API call (fetch/axios)?
Backend: which route, controller, middleware (auth, role checks)?
Database: which query and which table?

Write this trace down. You’ll be asked to explain it in the demo.

Phase 4: Write the proposal

For each of the three features, write:

Item	What to state
User	Who uses it
Problem	What it solves
Behaviour	What happens step by step
Frontend changes	Pages/components
Backend changes	Endpoints, validation, access checks
Database changes	New tables/columns
Acceptance criteria	Testable pass/fail statements

Rules to remember: at least two features need both frontend and backend changes, each feature must be a complete user capability, and cosmetic changes or existing functionality don’t count. Check the app first so you don’t propose something that already exists. Send the proposal for review before building.

Phase 5: Implement (45 marks for features, 15 for code quality)

Do one feature at a time, on its own branch:

bash
git checkout -b feature/<name>

For each feature, follow this order:

Database: Write a migration script (e.g. migrations/001_add_x.sql) and add sample data.
Backend: Add the route, controller, and query. Include input validation, authentication, and ownership/role checks. Return clear error codes (400, 401, 403, 404).
Frontend: Add the UI, with loading state, error messages, and empty state.
Test it manually, then commit with a meaningful message (feat: add saved job alerts endpoint).
Merge into main and move to the next feature.

Make sure the original login and workflows still work after each feature.

Phase 6: Test (10 marks, shared with documentation)

Make a test table for every feature:

Step	Expected	Actual	Pass/Fail

Cover these cases for each feature:

Successful use
Invalid input
Empty state
Unauthorised access (not logged in, or wrong user or role)

Also retest the original login and core workflow. Automated tests (Jest/Supertest for the API) are a bonus where practical.

Phase 7: Prepare the submission
Update the README in both repos: prerequisites, configuration, DB setup commands (including your migrations), and startup commands. Ask a classmate or test on a fresh clone to confirm it works.
Make sure no node_modules, build output, .env, or real data is committed.
Note the final commit IDs for both repos.
Write the report: name and student number, chosen app, original commit IDs, setup issues, each feature with acceptance criteria, test results, known limitations, and an acknowledgement of any AI help, libraries, and references.
Record a screen recording (or take screenshots) showing the original app working and all three features.
Submit through the lecturer’s announced channel.
Phase 8: Prepare for the demo (10 marks)

Be ready to:

Start both apps from scratch.
Demonstrate each feature against its acceptance criteria.
Explain one request from the UI through the API to the database.
Explain your access checks and make a small code change on request.

If you use AI help for code, make sure you read and understand every line, because you will be asked to modify it live.

Next step: tell me which application you picked, and once you’ve cloned it, share the README and project structure. I can then help you find realistic feature ideas that don’t already exist, draft the proposal, and design the database tables and endpoints.

how do you add features

Explaining how to add a feature to an existing app.
How to add a feature: the repeatable loop

Every feature follows the same path: database → backend → frontend → test → commit. Below is one full example. I’m assuming a typical Node/Express + MySQL backend and a JavaScript frontend (React or similar). Your repos may use different file names, so copy the patterns you find in them.

Step 0: Study how existing features are built

Before writing anything, find an existing feature similar to what you want and see how it’s structured:

Where are routes defined? (routes/, app.js)
Where do controllers or queries live?
How does auth middleware work, and how is the logged-in user identified (req.user.id)?
How does the frontend call the API (a shared api.js, axios instance, or plain fetch)?# structuered
Funtions
