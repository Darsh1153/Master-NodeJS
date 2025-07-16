# Master-NodeJS

## Do you know what happens when you pass the code to V8 engine?

![5B944096-C5AA-4997-8EB0-EC1081714BC1](https://github.com/user-attachments/assets/7ed4ae71-cba5-48b3-b900-99e242604298)


## Do you know what happens when you do an async task in libUV

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
