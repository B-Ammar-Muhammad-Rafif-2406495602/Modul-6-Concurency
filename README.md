## Commit 1 Reflection notes. 

In Commit 1, I added the handle_connection function to read and print the HTTP request from the browser. The
function uses BufReader to wrap the TCP stream, making it easier to read line by line. When I accessed 127.0.0
1:7878 from my browser, the terminal printed the raw HTTP request including the method GET, the path /, and headers
like Host and User-Agent. This shows that the browser sends a lot of information with every request, not just the
URL. I also learned that if handle_connection is defined but not called inside main(), the server will connect but
never actually process the request.

i also discover some error when trying to run after i added the handle connection, turns out i havent kill the old
server so it still runs in the background. i kill it using "taskkill /f /im hello.exe"