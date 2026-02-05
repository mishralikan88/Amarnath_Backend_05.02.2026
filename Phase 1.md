# PHASE 1 — 

#### DAY 1

## 1️⃣ What is Backend ? 

Backend is the code that runs on a server and handles logic + data.

Frontend:

UI
buttons
input fields

Backend:

receives data
applies rules
talks to database
sends result back


Real-life example

You press Login:
Frontend sends email + password
Backend checks credentials
Backend replies: success / error

### Backend handles business logic, data processing, and communication with databases.


## 2️⃣ Client vs Server 


Client
Browser
Mobile app
Postman

Server
Node.js app
Express backend

Relationship -
Client  → Request  → Server
Client  ← Response ← Server



### In the client–server model, the client sends requests and the server processes and responds.


## 3️⃣ What is a REQUEST?

A request is what the client sends to backend.


A request has:

Method → what action
URL → where  (The URL tells the server which resource or location the action is for.)

Examples:

GET /health (/health refers to the health endpoint of the server)
POST /login

Common methods - 

GET → Get data
POST → Create / send data
PUT → Update data
DELETE → Remove data

Request = method + url + headers + (optional) body + (optional) query/params


## 4️⃣ What is a Response ?

A response is what backend sends back.

A response has:

Status → success / error
Data → message or JSON

Example: { "message": "OK" }

### The server responds with a status code and data


## 5️⃣ What is an API? (don’t complicate)

API is a backend endpoint that clients can call.

Think: API = backend door

Example APIs:

GET /health
POST /login


### "An API is an endpoint that allows the client to communicate with the backend."


## 6️⃣ Why Express?

Problem - Node.js alone is low-level.

Solution-

Express makes it easy to:

create APIs
handle requests
send responses

### "Express simplifies building backend APIs in Node.js."


## 7️⃣ Your FIRST backend code (tiny)

Create server

```js

import express from "express";

const app = express();

app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```
Meaning -

Create backend app

Start server

Listen on port 3000



### We create an Express app and listen for incoming HTTP requests.




## 8️⃣ Your FIRST API (Health Check)

```js 

app.get("/health", (req, res) => {
  res.send("Server is healthy");
});

```

Meaning -

GET → read request

/health → API path

Response → simple text

Used to check: "Is backend alive?"


### A health endpoint checks whether the backend is running.




## 9️⃣ FULL Day-1 Code (FINAL)

```js

// Import the Express library so we can build a backend server.

import express from "express";


// Create an Express app. (not the server yet)

const app = express();


// Define a GET API at path "/health"

app.get("/health", (req, res) => {

  // Send a response back to the client (browser / frontend)
  res.send("Server is healthy");
});


// Start the server on port 3000.

app.listen(3000, () => {

 // Server is now running

  console.log("Server running on port 3000");
});

```


### Q: What happens when a user hits a backend API?

"The client sends a request, the backend processes it, and returns a response."

### Q: What is Express used for?

"Express is used to build backend APIs and handle HTTP requests easily."



#### DAY 2


## 1️⃣ Why do we need POST?


GET → used to read or check data

POST → used to send data to backend

We use POST when user:

logs in

signs up

submits a form



### POST is used when the client sends data to the server.




## 2️⃣ What is JSON? (JSON - Java Script Object Notation)


JSON is a simple text format used to send data.

Example: { "email": "a@b.com", "password": "1234" }

### Why JSON?

easy to read

easy to send

works in all languages


### JSON is a lightweight format used to exchange data between client and server.



👉 Frontend sends JSON as a STRING, not as an object.

1. On the client side (Frontend) -

```js

JSON.stringify({
  email: "a@b.com",
  password: "1234"
});

```
JSON.stringify() converts a JavaScript object into string

Network can only send text, not objects



2. On the server side (Backend) -

Server receives string data.

It must parse the string to get an object.


```js

JSON.parse(receivedString);

```

➡️ Parsing converts string → object


Simple Flow -

Frontend: Object → JSON.stringify() → STRING

Backend: STRING → JSON.parse() → Object



## 3️⃣ Important truth 

Frontend does NOT send objects directly.

It sends JSON as a STRING.

JSON.stringify({ email, password }) // string


So backend receives:

text

not a JavaScript object

👉 Backend must parse it first.



## 4️⃣ express.json() — the parser

```js

app.use(express.json());

```

### What this does?

reads JSON string from request body

converts it into a JavaScript object

stores it in req.body

Without this: ❌ req.body will be undefined

### express.json() parses JSON request bodies and makes them available on req.body.





## 5️⃣ Day-2 Code

```js

import express from "express";

// Create backend app (server not started yet)
const app = express();

// Parse incoming JSON string into JS object
app.use(express.json());

// Simple GET API (from Day 1)
app.get("/health", (req, res) => {
  res.send("Server is healthy");
});

// POST API to receive data from frontend
app.post("/greet", (req, res) => {
  // Read value sent by frontend
  const name = req.body.name;

  // Send response back
  res.json({
    message: `Hello ${name}`
  });
});

// Start the backend server
app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```



## 6️⃣ What happens STEP-BY-STEP

Frontend sends a POST request

Data is sent as JSON string

express.json() parses the string

Parsed data is stored in req.body

Backend reads req.body.name

Backend sends response




## 7️⃣ How frontend would call this (just for understanding)


```js

fetch("http://localhost:3000/greet", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Rahul" })
});

```

headers → tells backend the data is JSON

JSON.stringify → converts object to string



## 8️⃣ Day-2 Interview Q&A 

### Q: How does backend receive data from frontend?

Frontend sends data using POST as JSON, backend parses it using express.json(), and reads it from req.body.

### Q: Why is express.json() needed?

Because JSON is sent as a string and backend needs to convert it into an object.



## Day-2 CHECKPOINT

If you can explain this without reading, you are perfect: " POST sends JSON, express.json() parses it, and req.body holds the data. "





#### Day 3 

Status Codes · res.send() vs res.json() · Good API Responses

By the end of today, you should confidently say: "Backend sends proper status codes and meaningful responses to the frontend."


## 1️⃣ What is a Status Code?


A status code tells the client what happened with the request.

Backend always sends: a response with a status code

## 2️⃣ Most important status codes (ONLY THESE FOR NOW)

✅ 200 — Success

Request worked correctly.

Example:

login success

data fetched


❌ 400 — Bad Request

Client sent wrong or missing data.

Example:

missing email

invalid input


❌ 500 — Server Error

Backend failed internally.

Example:

crash

bug

database error


### Status codes tell the client whether a request succeeded or failed.


## 3️⃣ How do we send status codes in Express?

Syntax - res.status(200).send("OK");

or

res.status(400).json({ message: "Bad request" });


## 4️⃣ res.send() vs res.json()

res.send() sends text or simple response

Express auto-converts if needed

Example:

res.send("Server is healthy");


res.json()

sends JSON response

best for APIs

Example:

res.json({ message: "Success" });

Rule - 

👉 Use res.json() for APIs
👉 Use res.send() for simple text


### res.json() is preferred for APIs because it sends structured JSON data.


## 5️⃣ Bad API response ❌ (DON'T DO THIS)

res.send("Error");

❌ No status code.
❌ No clear message.
❌ Frontend confused.

## 6️⃣ Good API response ✅ (DO THIS)

```js

res.status(400).json({
  message: "Name is required"
});

```

✅ Clear status
✅ Clear message
✅ Easy for frontend to handle

###  Day-3 Code (with tiny inline comments)

```js

import express from "express";

const app = express();

// Parse JSON body
app.use(express.json());

// GET API (health check)
app.get("/health", (req, res) => {
  res.status(200).send("Server is healthy");
});

// POST API with validation
app.post("/greet", (req, res) => {
  const name = req.body.name;

  // If name is missing → client error
  if (!name) {
    return res.status(400).json({
      message: "Name is required"
    });
  }

  // Success response
  res.status(200).json({
    message: `Hello ${name}`
  });
});

// Start server
app.listen(3000, () => {
  console.log("Server running on port 3000");
});


```

## 8️⃣ What happens STEP-BY-STEP

Case 1: Name is missing

frontend sends POST

backend checks data

backend sends 400

frontend shows error


Case 2: Name is present

backend sends 200

backend sends JSON response

frontend shows success

## 9️⃣ Day-3 Interview Q&A 

### Q: Why do we use status codes?


✅ "Status codes tell the client whether a request was successful or failed."

### Q: Difference between res.send() and res.json()?

✅ "res.send() sends a simple response, while res.json() sends structured JSON data and is preferred for APIs."

### Q: When do we send 400?

✅ "When the client sends invalid or missing data."

✅ Day-3 CHECKPOINT

If you can explain this sentence without reading, you're doing GREAT:

"Backend validates input, sends proper status codes, and returns clear JSON responses."


#### Day 4 

Middleware · Request Flow · Custom Middleware

By the end of today, you should confidently say:

"Middleware runs before the API logic and can modify the request or response."

This is a VERY IMPORTANT interview topic.

## 1️⃣ What is Middleware? (VERY EASY)

Middleware is code that runs BEFORE your API logic.

Think like this:

Request comes in
Middleware runs
API logic runs
Response goes out


Visual flow -

```js

Request
  ↓
Middleware
  ↓
Route / API
  ↓
Response

```

Real-life analogy

Entering an office building:

Security checks you (middleware)

Then you meet the employee (API)


### Middleware is a function that runs between the request and the response.


## 2️⃣ Why do we need middleware?

Middleware is used for:

parsing JSON (express.json())

logging requests

authentication

validation

👉 Without middleware, code becomes messy.

### Middleware helps 'reuse common logic' across APIs.


## 3️⃣ You already used middleware

This line 👇

app.use(express.json());

What it is

✅ Middleware


What it does

runs before every request

parses JSON body

fills req.body

So yes — you already used middleware 🎉

## 4️⃣ How middleware works internally

A middleware function has:

(req, res, next)

req → request

res → response

next → move to next step

👉 If you don't call next() → request stops

## 5️⃣ Create a SIMPLE custom middleware

Example: Request Logger

```js

function logger(req, res, next) {
  console.log("Request received");
  next(); // move to API
}

```
Meaning

logs something

allows request to continue

## 6️⃣ Use middleware in app

app.use(logger);

This means: 👉 middleware runs for every request


## 7️⃣ Day-4 FULL Code (with tiny inline comments)


```js

import express from "express";

const app = express();

// Middleware: parse JSON body
app.use(express.json());

// Custom middleware
function logger(req, res, next) {
  console.log("Incoming request:", req.method, req.url);
  next(); // allow request to continue
}

// Register middleware
app.use(logger);

// API
app.post("/greet", (req, res) => {
  const name = req.body.name;

  if (!name) {
    return res.status(400).json({
      message: "Name is required"
    });
  }

  res.status(200).json({
    message: `Hello ${name}`
  });
});

// Start server

app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```

## 8️⃣ What happens STEP-BY-STEP

Client sends request.

express.json() runs.

logger middleware runs.

API logic runs.

Response is sent.

## 9️⃣ VERY IMPORTANT RULE 

❌ If middleware does NOT call next()

👉 request will hang

👉 API will never execute


## 🔟 Day-4 Interview Q&A 

### Q: What is middleware?

Middleware is a function that runs between the request and the response.

### Q: Why is next() needed?

next() passes control to the next middleware or route handler.

### Q: Is express.json() middleware?

"Yes, it is a built-in middleware that parses JSON request bodies."



✅ Day-4 CHECKPOINT

You should be able to say without reading:

"Middleware runs before API logic and can modify request or response."



#### Day 5 - 

Routes · File Separation · Basic Project Structure

By the end of today, you should confidently say:

"I separate routes from server code to keep the backend clean and scalable."


## 1️⃣ Problem with keeping everything in one file.

So far, we wrote everything in one file:

→ server start

→ middleware

→ APIs

This is OK for learning, but bad for real projects.

### Why it's bad ❌

→ File becomes very large.

→ hard to read.

→ hard to maintain.


### Large applications become hard to maintain if everything is in one file.


## 2️⃣ Solution: Separate ROUTES


Routes = files that only define API paths.


Server file - 

→ starts server.

→ loads middleware.

→ connects routes.


Route file - 

→ defines APIs


### We separate routes to improve readability and maintainability.


## 3️⃣ Basic folder structure (VERY IMPORTANT)

project/
│
├── index.js        ← server start
├── routes.js       ← API routes
└── package.json


This is enough for now.

## 4️⃣ routes.js - define APIs here

```js 

import express from "express";

const router = express.Router();

// Post API

router.post("/greet", (req, res) => {
  const name = req.body.name;

  if (!name) {
    return res.status(400).json({
      message: "Name is required"
    });
  }

  res.status(200).json({
    message: `Hello ${name}`
  });
});

export default router;

```

Explanation -

→ express.Router() → mini Express app

→ router.post() → define route

→ export default router → allow server to use it


## 5️⃣ index.js — main server file

```js

import express from "express";

import routes from "./routes.js";

const app = express();

// Parse JSON body

app.use(express.json());

// Load routes

app.use(routes);

// Start server

app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```


Explanation - 

→ server setup stays here

→ routes are plugged in

→ server starts cleanly



## 6️⃣ Request flow 

```js

Request
 ↓
express.json()
 ↓
routes.js
 ↓
response

```

## 7️⃣ Why this structure matters ?

Benefits - 

→ clean code

→ easy debugging

→ scalable for teams

→ easier testing

### Separating routes helps keep the server clean and improves maintainability.



## 8️⃣ Day-5 Interview Q&A

### Q: Why separate routes into files?

To keep the server clean and make the code easier to maintain.”

### Q: What is express.Router()?

express.Router() is just a container to hold routes separately.

Router is just a box that holds route definitions.

"It creates a mini router to define routes separately."

### Q: Where do you start the server?

"In the main server file using app.listen()."



✅ Day-5 CHECKPOINT

You should be able to say without reading:

"The server file starts the app, and route files only define APIs."



#### Day 6 - 


Query Params · Route Params · Difference

By the end of today, you should confidently say:

"Query params are optional filters, route params identify a specific resource."

This question is very common in interviews.


## 1️⃣ What are Query Parameters?


Query params are extra information sent in the URL.

They come after ? in the URL.

Example URL-  GET   /users?age=25&city=Delhi

Here:

age=25

city=Delhi

These are query parameters.


### How backend reads query params ? 

req.query


Example -

req.query.age

req.query.city


Easy use cases -

→ filtering

→ searching

→ sorting

→ pagination


### "Query parameters are used to filter or modify a request."


## 2️⃣ Query Params — Simple Code Example

```js 

app.get("/users", (req, res) => {
  const age = req.query.age;

  res.json({
    message: `Filtering users with age ${age}`
  });
});

```
URL - /users?age=25

## 3️⃣ What are Route Parameters?

Route params are part of the URL path itself.

They are used to identify one specific resource.

Example URL - GET /users/123

Here: 123 is the user ID

This is a route parameter

### How backend reads route params

req.params

Example:

req.params.id

Route parameters identify a specific resource.

## 4️⃣ Route Params — Simple Code Example

```js

app.get("/users/:id", (req, res) => {
  const userId = req.params.id;

  res.json({
    message: `Fetching user with id ${userId}`
  });
});

```
URL /users/123

## 5️⃣ MOST IMPORTANT DIFFERENCE (MEMORIZE)

Query Params	          Route Params
Optional	              Required
Used for filters	      Used to identify resource
After ?	Part of URL     path
req.query	              req.params


## 6️⃣ When to use WHAT? (INTERVIEW GOLD)

Use Query Params when:

→ filtering users

→ searching data

→ pagination

Example: /products?category=mobile



Use Route Params when:

→ fetching one item

→ updating one item

→ deleting one item

Example: /products/101

## 7️⃣ Day-6 FULL Example Code 


```js

import express from "express";

const app = express();

// GET users with query param

app.get("/users", (req, res) => {
  const age = req.query.age; // query param

  res.json({
    message: `Users filtered by age ${age}`
  });
});

// GET user by ID using route param

app.get("/users/:id", (req, res) => {
  const id = req.params.id; // route param

  res.json({
    message: `User with id ${id}`
  });
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```

## 8️⃣ Day-6 Interview Q&A (MEMORIZE)

### Q: What are query parameters?

"Query parameters are optional values used to filter or modify requests."

### Q: What are route parameters?

"Route parameters identify a specific resource in the URL."

### Q: Difference between query and route params?

"Query params are optional filters, route params are required identifiers."



✅ Day-6 CHECKPOINT

You should be able to explain without reading:

"Query params filter data, route params identify data."


#### Day 7 - 


FULL RECAP · MINI INTERVIEW · CONFIDENCE CHECK


By the end of today, you should confidently say:

"I understand how a request flows through an Express backend."

If you reach this → Phase 1 is SUCCESSFUL.

## 1️⃣ The BIG PICTURE 

Let's see the full backend flow you learned:

```js

Frontend
  ↓
Request (GET / POST)
  ↓
Middleware
  ↓
Route
  ↓
Response (status + JSON)

```


That's it.

Everything in Phase 1 fits into this.

## 2️⃣ What happens when a request comes? (STEP BY STEP)


Example: POST /greet

{ "name": "Rahul" }

Flow:

→ Frontend sends POST request.

→ Express receives request.

→ express.json() parses JSON.

→ Custom middleware (if any) runs.

→ Route logic executes.

→ Backend sends response with status code.

→ Frontend receives response.



## 3️⃣ Concepts YOU now know (CONFIRM)

You now understand:

✅ Backend basics

what backend is

client vs server

request vs response

✅ APIs

what an API is

GET vs POST

route paths

✅ Data handling

JSON

express.json()

req.body

✅ Responses

status codes (200, 400, 500)

res.send() vs res.json()

✅ Middleware

what middleware is

why next() is needed

request flow

✅ Structure

routes separation

express.Router()

✅ URL data

query params (req.query)

route params (req.params)

This is a solid backend foundation.

## 4️⃣ Final Phase-1 Example (COMPLETE CODE)


```js 

import express from "express";

const app = express();

// Middleware: parse JSON
app.use(express.json());

// Custom middleware
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});

// GET with query param
app.get("/users", (req, res) => {
  const age = req.query.age;
  res.json({ message: `Filter users by age ${age}` });
});

// GET with route param
app.get("/users/:id", (req, res) => {
  const id = req.params.id;
  res.json({ message: `User with id ${id}` });
});

// POST with body
app.post("/greet", (req, res) => {
  const name = req.body.name;

  if (!name) {
    return res.status(400).json({ message: "Name is required" });
  }

  res.status(200).json({ message: `Hello ${name}` });
});

// Start server
app.listen(3000, () => {
  console.log("Server running on port 3000");
});

```

👉 This single file uses EVERY Phase-1 concept.

## 5️⃣ MINI INTERVIEW 

### Q1: What happens when frontend calls an API?

"Frontend sends a request, backend processes it, and sends a response."

### Q2: Why do we use POST instead of GET?

"POST is used when the client needs to send data to the server."

### Q3: Why is express.json() needed?

"Because JSON is sent as a string and must be parsed into an object."

### Q4: What is middleware?

"Middleware runs before the API logic and can modify the request or response."

### Q5: Difference between query and route params?

"Query params are optional filters, route params identify a resource."


## 6️⃣ Phase-1 CONFIDENCE CHECK (HONEST)

Answer YES or NO honestly:

Can I explain request → response flow?

Do I know where req.body comes from?

Do I understand middleware execution?

Can I explain my backend code line by line?

If mostly YES → you are READY for Phase 2.



#### Day 8 -

This day exists ONLY to handle interviewer follow-ups.


## 1️⃣ req.headers (VERY SMALL ADDITION)

What you already know

Frontend sends headers:

headers: { "Content-Type": "application/json" }

What backend can do (NEW 1-LINER)

Backend can read headers using:

req.headers["content-type"];

or

req.headers.authorization;

###  Why this matters (simple)

Headers carry metadata, not actual data.

Examples:

content type

auth token

language

client info

"Headers carry metadata like content type and authentication tokens."


## 2️⃣ One IMPORTANT clarification about res.json()

You already know:

res.json({ message: "Hello" });

Add THIS sentence (VERY IMPORTANT)

res.json() automatically sets Content-Type: application/json.

This is a classic interviewer follow-up.


### "res.json() sends JSON data and automatically sets the correct response headers."

Thats enough.

No extra theory.

## 3️⃣ Middleware ORDER matters (EXPLICITLY STATED)

You already saw middleware execution.

Now we say it clearly.

Example

app.use(express.json());
app.use(logger);

Meaning
express.json() runs FIRST
logger runs SECOND

Golden rule (ADD THIS) -

"Middleware executes in the order it is defined — top to bottom."


"Middleware order is important because it executes sequentially."
















