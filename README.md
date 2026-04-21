## Commit 1 Reflection notes. 

In Commit 1, I added the handle_connection function to read and print the HTTP request from the browser. The
function uses BufReader to wrap the TCP stream, making it easier to read line by line. When I accessed 127.0.0
1:7878 from my browser, the terminal printed the raw HTTP request including the method GET, the path /, and headers
like Host and User-Agent. This shows that the browser sends a lot of information with every request, not just the
URL. I also learned that if handle_connection is defined but not called inside main(), the server will connect but
never actually process the request.

i also discover some error when trying to run after i added the handle connection, turns out i havent kill the old
server so it still runs in the background. i kill it using "taskkill /f /im hello.exe"


## Commit 2 Reflection notes. 

In Commit 2, I modified handle_connection to send back an actual HTTP response to the browser. The function now
reads hello.html using fs::read_to_string and sends its content back through the TCP stream. I learned that an HTTP
response has a specific format: it starts with a status line like HTTP/1.1 200 OK, followed by headers like
Content-Length which tells the browser how many bytes the response body contains, then a blank line, and finally the
actual HTML content. Without the correct format, the browser would not know how to read the response. After running
the server and accessing 127.0.0.1:7878, I could finally see my HTML page rendered in the browser with the "Hello!"
heading and my custom message.

## Commit 3 Reflection notes

In Commit 3, I added request validation so the server can respond differently depending on what the browser asks
for. The server now reads only the first line of the HTTP request and checks if it matches GET / HTTP/1.1. If it
matches, the server returns hello.html with a 200 OK status. If the request is for any other path like /bad, the
server returns 404.html with a 404 NOT FOUND status. This taught me how web servers handle routing at a basic level
— they look at the request path and decide what to respond with. Without this, every request would return the same
page regardless of what the user asked for, which is not how real web servers work.

# Commit 4 Reflection notes

In Commit 4, I added a /sleep route that simulates a slow response by making the server wait 10 seconds using
thread::sleep. When I opened two browser tabs — one accessing /sleep and one accessing / — I noticed that the
normal / request was also stuck waiting until the /sleep request finished. This happened because our server is
single-threaded, meaning it can only process one request at a time. Any new request has to wait in a queue until the
current one is done. This is a serious problem in real applications because one slow request from one user can block
all other users from getting a response. The solution to this is to use multiple threads so that each request can be
handled independently and simultaneously, which is what we will implement in Milestone 5.