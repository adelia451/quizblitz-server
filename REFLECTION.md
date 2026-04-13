# Part 3 - Reflection

## Q1 — Middleware

Middleware functions have access to the request object `req` and the response object `res` and are used to process requests and modify the responses before they reach the route handler.

The order middleware is registered matters because it runs from top to bottom. We structure our code middleware, routes, and then `POST` requests because the middleware has to run first to prepare the request before the route handler uses it. An example is that if `app.use(express.json())` were registered after the routes, `req.body` would be undefined when a `POST` request hits a route because the body hasn't been parsed yet

Global middleware is registered with `app.use()`, like the code below, and runs on every request to the server regardless of which route is hit. This makes sense for `cors` and `json` because every request needs them.

```js
app.use(cors({
  origin: allowedOrigins
}))

app.use(express.json())
```
Route-level middleware like `verifyToken`, in the code below, only runs for its specific route. Since `verifyToken` only needs to run on `POST /api/scores` we use route-level middleware.

```js
app.post('/api/scores', verifyToken, async (req, res) => {
    ...
}
```

## Q2 — Password Security
Passwords should not be stored in plain text because if the database is ever breached it is very easy to find passwords. Hashing is better because if someone breaches the database they'd get unreadable strings.

When you call `bcrypt.hash(password, 10)`, `bcrypt` takes the plain text password and runs it through a function that produces a scrambled string, and 10 is the amount of times `bcrypt` is run. These are called salt rounds and more rounds means slower computation which makes attacks harder. 

`bcrypt.hash()` is a one-way function, which means you cannot reverse it to get the original password. Since it is a one-way function, when we use `bcrypt.compare()`, instead of reversing the hash, `bcrypt` just takes the plain text password and runs it through the same hashing process. If they match then the password is correct. 

## Q3 — JWT Flow
- Registering account:
The client sends `{ email, password }`. The server validates the input by checking no accounts match the email and hashes the password with `bcrypt`. Then it saves a new user document with `email` and `passwordHash`. The server returns `{ message, userId, email }`.

- Logging in:
The client sends `{ email, password }`. The server finds the user by email and uses `bcrypt.compare()` to check if the password matches the stored hash. Then it creates a JWT token with `jwt.sign()` that stores `{ userId, email }`. The server returns `{ token, userId, email }`, where `token` is what the client saves in `localStorage`.

- Submitting score:
The client sends `{ score, totalQuestions }` and the JWT token in the authorization header. The server's `verifyToken` middleware function intercepts this request first, verifies the token's signature using `JWT_SECRET`, and then gets the `{ userId, email }` from it and attaches it to `req.user`. Then the route handler saves the score using `req.user.userId` and `req.user.email`. The server returns the saved score document.

Since the user info (`userId` and `email`) are stored inside the token, that is checked with `verifyToken` and `jwt.verify()`, there is no need to check the database to confirm who the user is.

## Q4 — In-Memory vs Database
I recall that every time someone submitted a score it was pushed into that array `(let scores = [])` and the array only existed in the server's memory while it was running. This causes an issue with having a leaderboard because if every time I stop the server, the array goes empty, then it's not possible to save players' scores or ranking or any record of who has played. Another problem is that if multiple people use the app at the same time and the server has to handle all those requests, the in-memory array cannot be shared across multiple server instances.

 I also recall that since `Score.create()` saves to MongoDB and `Score.find()` reads from MongoDB, the data would not be affected when redeploying since it lives in Atlas, a completely separate service from the server process. 
 
 The difference is that the in-memory array ceases to exist every time the server is stopped since it is living inside the Node.js process. I can recall this issue during the Thunder Client testing, where after restarting the server, `GET /api/scores` returned an empty array. MongoDB on the other hand exists independently of the server. 


## Q5 — Route Design Decision
`GET /api/scores` is public and has no middleware, meaning anyone can instantly view the leaderboard. This makes sense for the leaderboard since it is meant to be seen by everyone.

`POST /api/scores` is protected with `verifyToken` because when you submit a score it is tied to an account. Without `verifyToken` we could not verify that it was from someone in our database, leading to the possibility that people can send fake `POST`requests and spam the leaderboard. 

If `GET /api/scores` also required authentication then only logged-in users could see the leaderboard and `LeaderboardView.vue` would have a 401 error for those not logged in. Both these issues defeat the purpose of a public leaderboard. 



