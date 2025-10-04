# curl 
---

## Basic usages
Fetch a URL (print to stdout)  
    curl https://example.com

Save output to file (short)  
    curl -o file.html https://example.com

Save output to file keeping remote filename (best-effort)  
    curl -O https://example.com/index.html

Follow redirects (3xx)  
    curl -L https://example.com

Show response headers only (HEAD request)  
    curl -I https://example.com

Verbose output (request/response details)  
    curl -v https://example.com

Silent mode (no progress meter or error messages)  
    curl -s https://example.com

Silent with progress disabled but still show errors  
    curl -sS https://example.com

Show only HTTP status code  
    curl -s -o /dev/null -w "%{http_code}\n" https://example.com

---

## HTTP methods
GET (default)  
    curl https://api.example.com/resource

POST with form data (application/x-www-form-urlencoded)  
    curl -X POST -d "key1=value1&key2=value2" https://api.example.com/login

POST with multiple form fields (shorthand)  
    curl -d "name=Bob" -d "age=30" https://api.example.com/users

POST JSON (set header)  
    curl -X POST -H "Content-Type: application/json" -d '{"user":"bob"}' https://api.example.com/users

PUT with data  
    curl -X PUT -d '{"name":"new"}' -H "Content-Type: application/json" https://api.example.com/resource/1

PATCH example  
    curl -X PATCH -d '{"status":"done"}' -H "Content-Type: application/json" https://api.example.com/resource/1

DELETE request  
    curl -X DELETE https://api.example.com/resource/1

HEAD request  
    curl -I https://example.com

---

## Headers, authentication & cookies
Add a header (custom)  
    curl -H "X-My-Header: value" https://example.com

Add multiple headers  
    curl -H "Accept: application/json" -H "X-Trace: 123" https://example.com

Set User-Agent  
    curl -A "MyAgent/1.0" https://example.com

Basic auth (user:pass)  
    curl -u username:password https://example.com/protected

Bearer token (common for APIs)  
    curl -H "Authorization: Bearer TOKEN" https://api.example.com

Send cookies inline  
    curl -b "session=abcd1234; theme=dark" https://example.com

Load cookies from file (Netscape cookie format)  
    curl -b cookies.txt https://example.com

Save cookies to a file  
    curl -c cookies.txt https://example.com

---

## Uploading files & multipart
Upload file with POST form (multipart/form-data)  
    curl -F "file=@/path/to/file.txt" -F "name=test" https://upload.example.com

Upload file raw (PUT)  
    curl -X PUT --data-binary "@/path/to/file.bin" https://example.com/upload/file.bin

Send binary data from stdin  
    cat file.bin | curl --data-binary @- https://example.com/upload

---

## Downloading options
Resume an interrupted download (continue)  
    curl -C - -O https://example.com/largefile.zip

Limit download speed (example: 100K/s)  
    curl --limit-rate 100K -O https://example.com/file

Show progress bar (default for terminal) or use `-#` for a different bar  
    curl -# -O https://example.com/file

Quiet download (no progress) and write to file  
    curl -sS -o file.zip https://example.com/file.zip

---

## Query strings & URL-encoding
Send URL-encoded data using `--data-urlencode`  
    curl --data-urlencode "q=hello world" https://api.example.com/search

Include query string manually  
    curl "https://api.example.com/search?q=hello%20world&page=2"

---

## Output formatting & response info
Write body to a file and print headers to stdout  
    curl -D - -o body.html https://example.com

Save headers to file and body to file separately  
    curl -D headers.txt -o body.html https://example.com

Show timing and other variables (customize output)  
    curl -s -o /dev/null -w "time_namelookup: %{time_namelookup}s\ntime_connect: %{time_connect}s\ntime_total: %{time_total}s\n" https://example.com

Pretty-print JSON response using `jq` (if installed)  
    curl -s https://api.example.com/data | jq

---

## TLS/SSL options
Use specific TLS version (example TLS1.2)  
    curl --tlsv1.2 https:
