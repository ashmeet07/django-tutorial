# Websockets & configuration with django+react
*It is use to make two way communication bi-directional used for two way communication*

- Instead of request -> ,response <- model we use client ↔ server model (bi-directional) approach.
- Used for streaming live becaues ends are open from both the side.
- Transfer happen at anytime.

Normal HTTP request works like:


`Browser ----request----> Server`
`Browser <---response---- Server`

Websockets connection flow:
    - HTTP Handshake
        - The browser first sends  a normal HTTP request
        
        ```http
                GET /chat HTTP/1.1
                Host: example.com
                Upgrade: websocket
                Connection: Upgrade
```
        
        The server replies:
        
```http
        HTTP/1.1 101 Switching Protocols
        Upgrade: websocket
        Connection: Upgrade
```

        Now HTTP becomes a WebSocket connection.
        
# Required package in python 

        - channels
        //for production
        -  channels-redis
        //same as app we have to register channel in INSTALLED_APPS

        We need to specify ASGI_APP="project.asgi.application"

## asgi.py 

This is the first file which websockets first touch. [browser -> asgi.py -> consumer]

This tells Django HTTP? || Websocket? 

so from this we have learn't there there is another connections also exist like: 

HTTP 1.1 -> 3

HTTP 1.1  It uses TCP always for each time when opening the browser 