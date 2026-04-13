# Part 4 - Back-End Quiz

## Q1 — Multiple Choice

A student builds an Express route:

app.post('/api/data', (req, res) => {
  const { name } = req.body
  res.json({ received: name })
})
When tested with Thunder Client sending { "name": "Alice" } as a JSON body, received is undefined. The route is registered correctly. What is the most likely cause?

A) res.json() cannot serialize strings — it must use res.send()
B) app.use(express.json()) middleware is missing or registered after the route
C) POST routes cannot access req.body — only PUT routes support request bodies
D) The property should be accessed as req.query.name for POST requests

## Q2 — Short Answer
Explain the difference between 400 Bad Request, 401 Unauthorized, and 404 Not Found. For each status code, give one specific example from your QuizBlitz server where returning that code would be correct.

## Q3 — Code Analysis
A student writes the following route:

app.get('/api/scores', (req, res) => {
  Score.find().sort({ score: -1 }).limit(10)
  res.json({ message: 'done' })
})
The route always returns { message: 'done' } immediately and never returns any scores. Identify the problem and write the corrected version of the route.

## Q4 — Multiple Choice
Which of the following correctly describes the relationship between a Mongoose schema and a Mongoose model?

A) A schema is a running instance of a model that connects to a specific MongoDB collection
B) A schema defines the shape and validation rules for documents; a model is the class you use to query and save documents based on that schema
C) A model and a schema are interchangeable terms for the same concept in Mongoose
D) A schema stores the actual data; a model defines the structure
Q5 — Design Question
Your verifyToken middleware reads the token from the Authorization header and attaches the decoded payload to req.user. A classmate suggests storing the JWT in a cookie instead of having the client send it in a header, arguing it is simpler.

Give one genuine advantage of the cookie approach and one genuine advantage of the Authorization header approach. Then state which approach is more appropriate for a mobile-accessible game like QuizBlitz and explain why in two to three sentences.

## Q5 — Design Question
Your verifyToken middleware reads the token from the Authorization header and attaches the decoded payload to req.user. A classmate suggests storing the JWT in a cookie instead of having the client send it in a header, arguing it is simpler.

Give one genuine advantage of the cookie approach and one genuine advantage of the Authorization header approach. Then state which approach is more appropriate for a mobile-accessible game like QuizBlitz and explain why in two to three sentences.