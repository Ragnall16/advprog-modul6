# Reflection Modul 6
Ragnall Muhammad Al Fath
2306210550 / AdPro B
---

[Commit 1 Reflection notes](#commit-1-reflection-notes) <br>
[Commit 2 Reflection notes](#commit-2-reflection-notes) <br>
[Commit 3 Reflection notes](#commit-3-reflection-notes) <br>
[Commit 4 Reflection notes](#commit-4-reflection-notes) <br>
[Commit 5 Reflection notes](#commit-5-reflection-notes) <br>
[Commit Bonus Reflection notes](#commit-bonus-reflection-notes) <br>

---
 
## Commit 1 Reflection Notes
### Handle_Connection Method
The `handle_connection` function is responsible for processing incoming HTTP requests. It takes a `TcpStream` as input, allowing it to read data from the "client" (myself in this milestone). To handle this efficiently, the stream is wrapped in a `BufReader`, which enables line-by-line reading.

Each line of the request is read and stored in a `Vec<String>`, stopping once an empty line is encountered, indicating the end of the headers. The collected request data is then printed, providing a clear view of what the server receives from the client.

---
## Commit 2 Reflection Notes
The addition/modification of `handle_connection` function now serves an actual HTML file instead of just printing the request. It first reads the incoming HTTP request line by line, then loads the `hello.html` file from disk using `fs::read_to_string()`. To construct a valid HTTP response, it includes a status line (`HTTP/1.1 200 OK`), a `Content-Length` header indicating the size of the response, and the actual HTML content. This response is then written to the stream using `write_all()`, allowing the browser to correctly render the page. This addition/modification introduces the basics of serving static files and highlights the importance of properly formatting HTTP responses for client compatibility.

![Commit 2 screen capture](/assets/images/commit2.png)

---
## Commit 3 Reflection Notes
The addition/modification of `handle_connection` function now handles multiple response types by checking the request line. If the request is `"GET / HTTP/1.1"`, it serves `hello.html` with a `200 OK` status; otherwise, it returns `404.html` with a `404 NOT FOUND` status. The refactoring simplifies this logic by using a tuple to store both the status line and filename, reducing redundancy and making the code cleaner. Instead of repeating file reading and response formatting for each case, the refactored version selects the correct values first and processes them in a single step. This improves maintainability and makes it easier to extend the function for additional request handling in the future.

![Commit 3 screen capture](/assets/images/commit3.png)

---
## Commit 4 Reflection Notes
The modified `handle_connection` function introduces a delay for requests to `/sleep` using `thread::sleep(Duration::from_secs(10))`. When I opened `127.0.0.1:7878/sleep` in one browser tab and `127.0.0.1:7878` in another, I noticed that both requests were delayed. I tried only `127.0.0.1:7878` to make sure my device isn't lagging, and it displayed the html really quick. This happens because the server is handling requests sequentially in a single thread. While processing the `/sleep` request, the server is blocked and unable to respond to other incoming requests until the sleep duration is over. This demonstrates a limitation of a single-threaded server: if one request takes too long, it slows down all other requests. In a real-world scenario with multiple users, this could severely impact performance. A possible solution is to use multithreading to handle requests concurrently, ensuring that one slow request doesn’t delay others. (Which I will do in the next milestone/commit )

---
## Commit 5 Reflection Notes
The `ThreadPool` implementation allows the server to handle multiple requests concurrently instead of processing them sequentially. It works by creating a fixed number of worker threads that wait for tasks in a queue. When a new request comes in, it is assigned to an available worker instead of blocking the main thread. This prevents slow requests from delaying others and improves performance under heavy load. Each worker thread continuously picks up tasks, executes them, and then waits for the next task. This design reduces the overhead of constantly creating and destroying threads while keeping the server responsive.

---
## Commit Bonus Reflection Notes
The `build` function improves thread pool creation by explicitly returning a `Result`, providing a more structured approach to error handling. Instead of panicking when an invalid size (such as zero) is passed, it returns a `PoolCreationError`, allowing the caller to handle the failure gracefully. This makes the code more robust and predictable, aligning with Rust’s best practices for error handling. Using `Result<ThreadPool, PoolCreationError>` instead of `Option<ThreadPool>` or an immediate `panic!` ensures that errors are properly managed, promoting safer and more reliable code execution. Additionally, this approach makes it easier to extend error handling in the future without disrupting existing functionality.