# Part 3 - Reflection

## Q1 — Middleware
Your server uses app.use(express.json()) and app.use(cors()) before all routes, and verifyToken on individual protected routes.

Explain what middleware is in Express, why the order it is registered matters, and describe the difference between global middleware (registered with app.use) and route-level middleware (passed as an argument to a specific route). Use your own code as examples.

## Q2 — Password Security
Explain why passwords must never be stored in plain text. Describe what bcrypt does when you call bcrypt.hash(password, 10) and what the number 10 controls. Then explain how bcrypt.compare() works — why can it verify a password without ever reversing the hash?

## Q3 — JWT Flow
Walk through the complete authentication flow in your QuizBlitz server from a user's perspective: from registering an account, to logging in, to submitting a score. For each step, state what the client sends, what the server does, and what the server returns. Your answer must explain what information is embedded in the JWT and why the server does not need to look up the database to verify a token on each request.

## Q4 — In-Memory vs Database
In Week 9 your scores were stored in a plain JavaScript array (let scores = []). In Week 10 you replaced it with MongoDB. Describe two concrete problems you observed or would observe with the in-memory approach that the database solves. Then explain what would happen to your MongoDB data if you redeployed the server — and why this is different from what happened to the in-memory array on restart.

## Q5 — Route Design Decision
Your server has two separate score routes: GET /api/scores is public and POST /api/scores is protected. Explain why this distinction makes sense for a leaderboard application. Then consider this alternative design: what would break or become problematic if GET /api/scores also required authentication?