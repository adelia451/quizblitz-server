# Part 4 - Back-End Quiz

## Q1 — Multiple Choice
B) `app.use(express.json())` middleware is missing or registered after the route

## Q2 — Short Answer
`400 Bad Request` means the server cannot process the request because of the client's badly formed request, so things like syntax issues or incomplete requests. An example would be in the register route, when the email or password is missing it sends this error.

`401 Unauthorized` means the server cannot fulfill the request because it doesn't have the authorization credential needed. An example would be in the login route. When the password doesn't match, `bcrypt.compare()` returns false and the server returns 401.

`404 Not Found` means the server can't find the requested page. Something that might happen if someone requests `GET /api/users/exampleUser` and that user does not exist.

## Q3 — Code Analysis
The student's code returns `{ message: 'done' }` immediately since it does not wait for the scores. This is because it does not use `async`/`await`. Since they don't use these, it does not wait for the database to respond and immediately runs `res.json({ message: 'done' })` before scores come back, which is why it doesn't return the score either. 

This is the corrected code I used:
```js
// GET /api/scores — MILESTONE 4
app.get('/api/scores', async (req, res) => {
  try {
    const scores = await Score.find()
      .sort({ score: -1 })
      .limit(10)
    res.json(scores)
  } catch (error) {
    console.error('Error fetching scores:', error.message)
    res.status(500).json({ error: 'Failed to fetch scores' })
  }
})
```

## Q4 — Multiple Choice
B) A schema defines the shape and validation rules for documents; a model is the class you use to query and save documents based on that schema

## Q5 — Design Question
An advantage of using cookies is that the browser sends the JWT automatically on every request, so we wouldn't have to add it to our code.

An advantage of the header approach is that it would work everywhere, including mobile apps, since cookies are browser-specific. 

The Authorization header approach is more appropriate for QuizBlitz because the app is mobile-accessible and cookies behave differently across mobile browsers. Since the Vue store already manually sends the token in `submitScore()`, the header approach works consistently across all platforms.