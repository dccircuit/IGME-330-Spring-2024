# DRAFT: Promises/Fetch/async-await/try-catch

## All about 4 topics:
- Promises
- Fetch API
- Async-Await
- Try-Catch

### Introduction
In the world of web development, managing asynchronous operations is essential. JavaScript offers several powerful tools for handling operations that don't complete immediately, such as fetching data from a server, reading files, or executing time-consuming computations. Understanding how to manage these operations and their potential errors efficiently is crucial for any developer aiming to build responsive and robust applications.

Asynchronous programming in JavaScript can be tricky, especially when it comes to error handling. Traditional synchronous error handling methods fall short when applied to asynchronous code, leading to bugs and unresponsive applications. 

### Section 1: Understanding Promises
A Promise in JavaScript is an object representing the eventual completion or failure of an asynchronous operation. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future.

#### 1.1 What is a Promise?
A Promise is an object that encapsulates the result of an asynchronous operation. It can be in one of three states:
- **Pending**: The initial state of a Promise. The operation has not completed yet.
- **Fulfilled**: The operation completed successfully, and the Promise has a value.
- **Rejected**: The operation failed, and the Promise has a reason for the failure.

#### 1.2 Creating and Using Promises
To create a Promise, you use the `Promise` constructor which takes an executor function. This function is executed immediately by the Promise implementation, and it receives two functions as parameters: `resolve` and `reject`.
```javascript
const myPromise = new Promise((resolve, reject) => {
    // Asynchronous operation code here
    if (/* operation successful */) {
        resolve(value); // Resolve the Promise with a value
    } else {
        reject(error); // Reject the Promise with an error
    }
});
```

#### 1.3 Handling Promise Results
Once a Promise is created, you can use the `.then()`, `.catch()`, and `.finally()` methods to handle the fulfilled or rejected state.
- **`.then()`**: Attaches callbacks for the resolution and/or rejection of the Promise.
- **`.catch()`**: Attaches a callback for handling any rejection.
- **`.finally()`**: Allows executing a callback regardless of the Promise’s fate. This is useful for cleanup activities.

```javascript
myPromise
    .then(value => {
        console.log('Success:', value);
    })
    .catch(error => {
        console.error('Error:', error);
    })
    .finally(() => {
        console.log('Operation attempted.');
    });
```


### Section 2: The Fetch API Using Promises
The Fetch API is a modern interface built on Promises (which add power and flexibility) that allows you to make HTTP requests similar to XMLHttpRequest. 

#### 2.1 Introduction to the Fetch API
Fetch provides a global `fetch()` method that returns a Promise, which resolves with the `Response` object representing the response to the request. This makes it an excellent candidate for learning how to handle asynchronous HTTP requests with Promises.

#### 2.2 Making a Request with Fetch
To make a GET request to retrieve data from a server, you can simply call `fetch()` with the URL:
```javascript
// Start fetch request; returns a Promise.
fetch('https://api.example.com/data')
    .then(response => {
        // Check response status  (see 2.3 below)
        if (!response.ok) {
            // Throw error if response is not ok
            throw new Error('Network response was not ok ' + response.statusText);
        }
        // Parse JSON from response
        return response.json(); 
    })
    .then(data => {
        // Log data on successful retrieval
        console.log('Data retrieved successfully:', data);
    })
    .catch(error => {
        // Handle any errors from fetch or parsing
        console.error('Failed to fetch data:', error);
    });

```
This example shows how to handle the response by checking if the request was successful, parsing the JSON, and how to catch any errors that occur during the fetch operation.

#### 2.3 Handling Common Errors with Fetch
Understanding how to handle different types of errors with Fetch is crucial:
- **Network errors**: These are caught by the `.catch()` method.
- **HTTP errors**: Since fetch only rejects on network failures, handling HTTP errors (like 404 or 500) requires checking the response status within the first `.then()` method.

### Section 3: From Promises to Async-Await
While Promises significantly improve the way we handle asynchronous operations, `async` and `await` syntax further simplifies the code, making it cleaner and more readable.

#### 3.1 Understanding Async-Await
The `async` and `await` keywords enable asynchronous, promise-based behavior to be written in a cleaner style, avoiding the need for chaining promises.

- **`async` function**: Declaring a function as `async` means that the function always returns a promise.
- **`await` keyword**: Can only be used inside `async` functions. It pauses the function execution until the promise settles, and then resumes the function execution and returns the resolved value.

#### 3.2 Simplifying Fetch with Async-Await
Using `async-await` with the Fetch API simplifies handling asynchronous requests by making the syntax look synchronous:
```javascript
async function fetchData(url) {
    try {
        // Await response from fetch call
        const response = await fetch(url);
        // Check response status
        if (!response.ok) {
            // Throw error for unsuccessful response
            throw new Error('Network response was not ok');
        }
        // Parse JSON from response
        const data = await response.json();
        // Log successfully retrieved data
        console.log('Data retrieved successfully:', data);
        // Return data
        return data;
    } catch (error) {
        // Handle fetch or JSON parsing errors
        console.error('Failed to fetch data:', error);
    }
}

```
In this example, `await` is used to wait for the `fetch` promise to resolve or reject, and errors are handled using a `try-catch` block, which is significantly easier to read and maintain compared to chaining `.then()` and `.catch()`.

### Section 4: Deep Dive into Async-Await and Error Handling
Now that you understand the basics of async-await, let's explore how to use this powerful syntax to manage more complex asynchronous scenarios and incorporate robust error handling.

#### 4.1 Advanced Async-Await Patterns
- **Handling multiple asynchronous operations in parallel**: Use `Promise.all()` to wait for multiple promises to resolve:
  ```javascript
  async function fetchMultipleUrls(urls) {
      try {
          const responses = await Promise.all(urls.map(url => fetch(url)));
          const data = await Promise.all(responses.map(res => res.json()));
          return data; // returns an array of results
      } catch (error) {
          console.error('Error fetching multiple URLs:', error);
          throw error; // re-throw to handle the error further up the call stack
      }
  }
  ```
- **Sequential vs Parallel Requests**: Discuss when to use sequential requests with separate await calls and when to use parallel requests with `Promise.all()` to improve performance.

#### 4.2 Error Handling with Async-Await
- **Try-Catch Blocks**: The primary method for handling errors in async functions. Demonstrates how to catch both synchronous and asynchronous errors in the same block and manage in a unified way.
- **Error Propagation**: Errors can also be propagated through a chain of async functions and be caught at the appropriate level.
- There are more advanced techniques that we can not cover in this tutorial, but you can researc

### Section 5: Additional Example
This example includes the asynchronous programming and error handling techniques you've learned.

#### 5.1 Real-World Scenario
- **User Authentication**: Implements an async function to handle user login, demonstrating how to manage network requests and errors.
  ```javascript
  async function loginUser(credentials) {
      try {
          const response = await fetch('https://api.example.com/login', {
              method: 'POST',
              headers: { 'Content-Type': 'application/json' },
              body: JSON.stringify(credentials)
          });
          const data = await response.json();
          if (!response.ok) {
              throw new Error(data.message);
          }
          return data; // user data on successful login
      } catch (error) {
          console.error('Login failed:', error);
          alert('Failed to login: ' + error.message);
          throw error;
      }
  }
  ```
