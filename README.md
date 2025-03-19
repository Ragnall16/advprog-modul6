# Reflection Modul 6
Ragnall Muhammad Al Fath
2306210550 / AdPro B
---

[Commit 1 Reflection notes](#commit-1-reflection-notes)
[Commit 2 Reflection notes](#commit-2-reflection-notes)

---
 
## Commit 1 Reflection Notes
### Handle_Connection Method
The `handle_connection` function is responsible for processing incoming HTTP requests. It takes a `TcpStream` as input, allowing it to read data from the "client" (myself in this milestone). To handle this efficiently, the stream is wrapped in a `BufReader`, which enables line-by-line reading.

Each line of the request is read and stored in a `Vec<String>`, stopping once an empty line is encountered, indicating the end of the headers. The collected request data is then printed, providing a clear view of what the server receives from the client.

---
## Commit 2 Reflection Notes
The addition/modification of `handle_connection` function now serves an actual HTML file instead of just printing the request. It first reads the incoming HTTP request line by line, then loads the `hello.html` file from disk using `fs::read_to_string()`. To construct a valid HTTP response, it includes a status line (`HTTP/1.1 200 OK`), a `Content-Length` header indicating the size of the response, and the actual HTML content. This response is then written to the stream using `write_all()`, allowing the browser to correctly render the page. This addition/modification introduces the basics of serving static files and highlights the importance of properly formatting HTTP responses for client compatibility.

![Commit 2 screen capture](/assets/images/commit2.png)