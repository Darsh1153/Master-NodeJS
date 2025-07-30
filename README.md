# Master-NodeJS

## Do you know what happens when you pass the code to V8 engine?

![5B944096-C5AA-4997-8EB0-EC1081714BC1](https://github.com/user-attachments/assets/7ed4ae71-cba5-48b3-b900-99e242604298)


## Do you know what happens when you do an async task in libUV?

🧠 How Libuv Manages Tasks in Node.js (Simple Explanation):
Node.js uses Libuv to handle things that take time — like reading files, making API calls, or setting timeouts — without blocking the rest of your code.

In the code shown:

js
Copy
Edit
https.get("https://api.fbi.com", ...);
setTimeout(..., 5000);
fs.readFile("./gossip.txt", ...);
These are asynchronous tasks (they take time). Here’s how Libuv helps:

Call Stack (V8 Engine)
The normal JavaScript code runs here (e.g., multiplication logic).

Libuv Steps In
When something like setTimeout, fs.readFile, or https.get is called, it's given to Libuv, which handles:

Timers

File System

Network

Libuv Uses:

Thread Pool to handle file reading (like fs.readFile).

Event Loop to check when tasks are done.

Callback Queue to store the results once ready.

Back to JS
Once done, the callback (e.g., console.log("File Data", data)) goes back to the JavaScript engine (V8) and runs.

✅ Summary:
Libuv makes sure that slow tasks (like file reading or HTTP requests) run in the background, so your main program doesn't stop or wait. That's what we call Non-Blocking I/O.

![54FD3A77-5C7E-4C32-8D29-2CDE764DB15B](https://github.com/user-attachments/assets/68959fcb-cf01-43c1-abb3-32de5aff86f6)



## Do you know how the event loop works behind the scenes?

![D057E0A3-73F5-4890-A169-0D7E4994B3B5_1_105_c](https://github.com/user-attachments/assets/dd625ea3-9ac0-493b-9eca-70d4d490561b)

Here's a simple explanation of how the Event Loop is managing the code shown in the image and how the console output is printed in that specific order.

🧠 What’s Happening Here?
The code:

![DC8C16CC-009C-43B9-A102-A2E692EBBE0C](https://github.com/user-attachments/assets/51ab47f2-e1ec-4a0c-8784-8d30690b5bdb)




🔁 How the Event Loop Manages This:
Synchronous Code Runs First (in the Call Stack):

printA() logs: a = 100

console.log("Last line of the file.");

✅ So the console shows:

a = 100
Last line of the file.
Now Event Loop Kicks In:

setTimeout(..., 0) goes to the Timer Phase.

setImmediate(...) goes to the Check Phase.

fs.readFile(...) is offloaded to the Libuv Thread Pool, and its callback comes back during the Poll Phase (after I/O completes).

Execution Order After Call Stack is Empty:

Even though setTimeout(..., 0) has 0 ms delay, the timer callback runs only after current sync tasks are done.

In Node.js, Timer callbacks are usually prioritized before setImmediate().

setImmediate() is part of the Check phase.

fs.readFile (I/O) callback comes after the poll phase ends.

✅ Final Console Output:
arduino
Copy
Edit
a = 100
Last line of the file.
Timer expired
setImmediate
File Reading CB
📌 In Simple Words:
First, normal code runs.

Then, the Event Loop picks up delayed and async tasks in a specific order:

Timers (setTimeout)

Check Phase (setImmediate)

Poll Phase (fs.readFile callback)


# WHAT IS THE DIFFERENCE BETWEEN SQL AND NOSQL
![94E6121D-D83D-4392-85F9-C1F1AF7D2FB6](https://github.com/user-attachments/assets/b63f1bd7-9796-4dcf-879b-574fed1c7405)


# HOW DO YOU AUTHENTICATE, THE CORRECT WAY

![08502831-BFE0-41C2-8B7A-F1CF8406380B](https://github.com/user-attachments/assets/bf28daa0-f90a-4974-ab15-c185310fbe0e)

1. User Login Request:
Client Side (User): The user initiates the login process by providing their email and password.

This is typically a POST request to the login endpoint on the server, where the user sends their credentials.

Example:

json
Copy
Edit
{
  "email": "user@example.com",
  "password": "userPassword123"
}
<br/>
2. Server Authentication (Validation):
Server Side (Backend):

Server receives the request and starts validating the user credentials.

The server checks the provided email and password against the stored data (e.g., in a database).

Password validation typically uses hashing techniques (e.g., bcrypt) to compare the password without exposing it in plaintext.

<br/>
3. JWT Generation:
If the credentials are valid, the server generates a JWT (JSON Web Token).

This JWT typically contains user information (like the user ID or email) and expiration data (i.e., how long the token is valid for).

Example JWT structure:

json
Copy
Edit
{
  "userId": "123456",
  "email": "user@example.com",
  "exp": 1625136123  // Expiration timestamp
}
<br/>
4. Sending the JWT to Client:
The server sends the JWT back to the client in the HTTP response.

This JWT is sent as a cookie (in most cases), or it could be sent in the response body or headers.

If the JWT is sent as a cookie, the server may set the cookie with the HttpOnly flag, meaning it cannot be accessed via JavaScript on the client-side. This enhances security.

Cookie Example:

http
Copy
Edit
Set-Cookie: token=<JWT_TOKEN>; HttpOnly; Secure; SameSite=Strict;

<br/>

5. Client Stores the JWT (Cookie Storage):
The client’s browser stores the JWT in a cookie.

Cookie Storage: The browser handles cookies automatically. Whenever the JWT is stored as a cookie, it gets sent along with every subsequent request to the same server.

Cookies are stored on the user's device and are automatically included in each HTTP request header sent to the server, without needing to be manually added.

Important flags for cookies:

HttpOnly: Makes the cookie inaccessible via JavaScript, enhancing security.

Secure: Ensures the cookie is only sent over HTTPS.

SameSite: Restricts how cookies are sent with cross-site requests (important for preventing CSRF attacks).
<br/>

6. Subsequent Requests with JWT:
Client Requests Data: After the JWT is stored in the cookie, the client can make further requests (e.g., to access protected routes).

The browser automatically includes the JWT cookie in the request headers.

Example of an HTTP request with the cookie:

http
Copy
Edit
GET /profile
Cookie: token=<JWT_TOKEN>
<br/>

7. Server Validates JWT:
Server: On receiving a request with the JWT in the cookie, the server extracts and validates the JWT.

It checks whether the JWT is valid (not expired) and if the user information inside the token matches the requested resource.

If the JWT is valid, the server allows access to the requested resource (e.g., user profile, dashboard).

If invalid or expired, the server will respond with an authentication error and may ask the client to log in again.
<br/>

8. JWT Expiry & Refresh Tokens:
If the JWT has expired, the server may either:

Return a 401 Unauthorized response, requiring the user to log in again.

Or, use a refresh token (if implemented) to generate a new JWT without requiring the user to re-enter their credentials.

Key Points to Remember:
JWT: A secure token that identifies the user and is used for authentication.

Cookies: Store the JWT and are automatically included in HTTP requests by the browser.

Server: Validates the JWT on every request to ensure the user is authorized to access protected resources.

Security: HttpOnly and Secure flags are crucial for securing cookies, preventing them from being accessed or tampered with.

Example of JWT Authentication Flow:
Login:

User sends email and password → Server validates → Server generates JWT.

Token sent back:

Server sends JWT as a cookie (with HttpOnly, Secure, and SameSite flags).

Subsequent Request:

User makes another request (e.g., to access profile) → Cookie with JWT sent automatically → Server validates JWT → User is authorized.

Diagram Summary (Flow):
User sends login request → Server validates email and password.

Server generates JWT → Sends JWT back to the client as a cookie.

Client stores the JWT cookie.

Client makes subsequent requests → Browser sends JWT cookie automatically with each request.

Server validates JWT → Grants or denies access based on validity.

This process ensures a secure and stateless authentication mechanism, where the server doesn't need to store session data. The JWT handles the user's state and credentials for future requests.



