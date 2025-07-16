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

const a = 100;

setImmediate(() => console.log("setImmediate"));        // A
fs.readFile("./file.txt", "utf8", () => {                // C
  console.log("File Reading CB");
});
setTimeout(() => console.log("Timer expired"), 0);       // B

function printA() {
  console.log("a =", a);                                 
}

printA();                                                
console.log("Last line of the file.");



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



