# Curl

[man pages](https://curl.se/docs/manpage.html)

## Useful arguments
- `-H 'Header-One: value1, value2' -H 'Header-Two: something`
    - `-H` tag per extra header you want to send
- `-i` fetch only the headers from the response
- `-X POST|GET|OPTIONS` specify the http method
- `$ curl -i http://localhost:3000 -H 'Origin: localhost' -H 'Access-Control-Request-Method: POST' -X OPTIONS` sample command to test CORS response
    - maybe a better example: `curl -i http://localhost:3000/auth/login -X OPTIONS -H 'Referer: http://localhost:8080' -H 'Origin: http://localhost:8080/' -H 'Access-Control-Request-Method: POST'`

