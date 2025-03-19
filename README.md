# Reflection Modul 6
Ragnall Muhammad Al Fath
2306210550 / AdPro B
---

[Commit 1 Reflection notes](#commit-1-reflection-notes)

---
 
## Commit 1 Reflection Notes
### Handle_Connection Method
The `handle_connection` function is responsible for processing incoming HTTP requests. It takes a `TcpStream` as input, allowing it to read data from the "client" (myself in this milestone). To handle this efficiently, the stream is wrapped in a `BufReader`, which enables line-by-line reading.

Each line of the request is read and stored in a `Vec<String>`, stopping once an empty line is encountered, indicating the end of the headers. The collected request data is then printed, providing a clear view of what the server receives from the client.